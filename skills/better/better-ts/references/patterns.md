# TypeScript patterns

Code examples for each rule in `SKILL.md`. Make illegal states unrepresentable, establish trust at boundaries, and keep business logic separate from framework adapters.

## Branded types

Brand primitives so they can't be mixed up. Validate once at creation; downstream code trusts the type.

```ts
type AgentId = string & { readonly __brand: "AgentId" };

function focusAgent(id: AgentId): void {
  /* input is trusted */
}
```

Match the `readonly __brand: 'X'` shape; don't invent a new convention. The schema-backed constructor in **Expected errors** checks the value before branding it.

## Discriminated unions

If a bug forces the question "wait, can this combination actually happen?", the type is too loose. Model variants with a literal discriminant: every variant shares the field name and each variant's value is unique, so impossible combos can't be represented.

```ts
// Don't. Boolean + optionals lets contradictory states exist.
type DiffState = { loading: boolean; diff?: GitDiff; error?: string };

// Do. Only valid states exist.
type DiffState =
  | { kind: "loading" }
  | { kind: "ready"; diff: GitDiff }
  | { kind: "error"; error: string };
```

Pick one discriminant name (`kind`, `type`, `tag`) and stick to it.

## `unknown` over `any`

`any` disables type checking for everything it touches. External data is always `unknown`. Narrow before use.

```ts
// Don't
function handle(input: any) {
  return input.foo.bar;
}

// Do
function handle(input: unknown) {
  if (typeof input === "object" && input !== null && "foo" in input) {
    // narrowed; compiler verifies access
  }
}
```

External sources include RPC payloads, `JSON.parse`, `postMessage`, IPC, file contents, environment variables, database results. Narrowing a property does not establish a complete contract; parse with its schema before treating the value as that contract.

## No `as` casts

Don't assert an unknown payload into a domain type. Parse it and consume the schema's typed output instead; see **Expected errors** for a complete example.

When refactoring an `as` out of existing code, identify why TypeScript can't infer:

- Missing discriminant: add one, switch to a discriminated union.
- Overly wide source type (e.g. `Record<string, unknown>`): narrow it.
- Untyped boundary: add a schema-backed parse function.
- Genuinely inexpressible invariant: isolate and justify the assertion in a checked constructor or adapter.

`as const` preserves literal/readonly inference; it is not an unchecked payload assertion. `satisfies` checks assignability, not unknown runtime data.

## Narrowing hierarchy

From best to last-resort:

1. **Discriminated union switch / if.** Compiler narrows automatically.
2. **`in` operator.** `"key" in obj` narrows to variants containing that key.
3. **`typeof` / `instanceof`.** For primitives and class instances.
4. **User-defined type guard.** When the above aren't enough.
5. **`as` cast.** Only after validation.

```ts
function area(s: Shape): number {
  if ("radius" in s) return Math.PI * s.radius ** 2; // narrowed to circle
  return s.width * s.height; // narrowed to rect
}
```

## Type guards

A guard must actually verify the claim. A lying guard is worse than `as` because the bug hides behind a name that says it's safe.

```ts
function isCircle(s: Shape): s is Shape & { kind: "circle" } {
  return s.kind === "circle";
}
```

Prefer discriminant narrowing when possible. The guard adds a layer the reader has to follow. Both outcomes must be truthful: a guard claiming `value is number` must not also reject numbers on business grounds.

## Exhaustiveness

In default arms, assign the value to a `never`-typed local. The compiler errors if a new variant is added without handling.

```ts
// Value-returning switch
function area(s: Shape): number {
  switch (s.kind) {
    case "circle":
      return Math.PI * s.radius ** 2;
    case "rect":
      return s.width * s.height;
    default: {
      const _exhaustive: never = s;
      return _exhaustive;
    }
  }
}

// Void switch
function handle(s: Shape): void {
  switch (s.kind) {
    case "circle":
      drawCircle(s);
      break;
    case "rect":
      drawRect(s);
      break;
    default: {
      const _exhaustive: never = s;
      void _exhaustive;
    }
  }
}
```

Return-style in value-returning switches; void-style in statement switches.

## `satisfies` over `as`

`satisfies` checks assignability without replacing the expression's inferred type. Contextual typing still influences inference; it doesn't preserve every literal automatically.

```ts
type Config = { theme: "dark" | "light"; cols: number };

// Don't. Asserts the wider type.
const config = { theme: "dark", cols: 3 } as Config;

// Do. Checks the shape and retains useful inferred detail.
const config = { theme: "dark", cols: 3 } satisfies Config;
// config.theme is "dark" (literal), not the whole theme union
```

## Boundary validation

Always parse external `unknown` with a Standard Schema–compliant library when its expected schema is known. Parse before using it as that contract; trust validated types inside.

- Reuse the project's compliant library and schemas. Native parsing APIs are fine; the Standard Schema interface is useful for library-independent integrations, not mandatory application plumbing.
- Consume the parsed output, not the original input. Parsing may transform values.
- Existing runtime boundary validation counts; generated declarations alone do not validate external data.
- If the expected shape is genuinely unknown, retain `unknown` until a consumer establishes its requirements. Don't invent a schema or cast.
- Choose whether to reject, strip, or preserve unknown fields from the contract's compatibility and security needs.
- **Persisted JSON:** decode, validate its versioned schema, and represent expected decoding or validation failures explicitly.
- **Don't re-validate** deep in call chains. Recheck when data crosses another trust boundary, not on every internal call.
- Schema validity does not establish authorization or state-dependent business rules. Check those where the operation requires them.

Keep business logic separate from boundary and framework adapters. Propagate expected errors internally; translate them to responses or exit codes at the outer boundary.

## Schema-derived types

When a `.proto`, OpenAPI spec, GraphQL schema, or database migration already defines a shape, derive from the generated types instead of duplicating them.

```ts
// Don't. Duplicate shape, drifts when the schema changes.
type CheckSummary = {
  totalCount: number;
  checks: { name: string; status: string }[];
};
function renderChecks(s: CheckSummary) {
  /* ... */
}

// Do. Derive from the generated schema type.
import type { ChecksMessage } from "<generated module>";
function renderChecks(s: Pick<ChecksMessage, "totalCount" | "checks">) {
  /* ... */
}
```

Reach for `Pick`, `Omit`, `Parameters`, `ReturnType`, `Awaited`, `typeof` before writing a new interface. For runtime schemas, infer the parsed output type; input and output can differ when parsing transforms values.

## Object args

```ts
// Don't. Swap two args, still compiles.
openFile(uri, {
  startLineNumber: 10,
  startColumn: 1,
  endLineNumber: 10,
  endColumn: 1,
});

// Do. Order-independent, self-documenting.
openFile({
  uri,
  selection: {
    startLineNumber: 10,
    startColumn: 1,
    endLineNumber: 10,
    endColumn: 1,
  },
});
```

Skip on hot paths: per-frame render, tokenizers, parsers, anything in a tight loop where the allocation cost matters.

## Expected errors

Always represent expected failures with `better-result`'s `Result<T, E>` and explicit `TaggedError` variants. Invalid input, rejected credentials, missing required records, and unavailable upstream services are failures callers can act on. Handle, translate, or propagate their typed union; don't hide it with `unwrap()` or an empty success value.

This example uses Valibot and better-result 3.x; the rule allows any Standard Schema–compliant library. Match APIs to the project's installed versions.

```ts
import * as v from "valibot";
import { Result, TaggedError } from "better-result";

type AgentId = string & { readonly __brand: "AgentId" };

const AgentIdSchema = v.pipe(
  v.string(),
  v.uuid(),
  // The schema verifies the invariant before introducing the semantic brand.
  v.transform((value): AgentId => value as AgentId),
);

class InvalidAgentId extends TaggedError("InvalidAgentId")<{
  message: string;
}> {}

function parseAgentId(
  input: unknown,
): Result<v.InferOutput<typeof AgentIdSchema>, InvalidAgentId> {
  const parsed = v.safeParse(AgentIdSchema, input);
  if (!parsed.success) {
    return Result.err(new InvalidAgentId({ message: "Expected an agent UUID" }));
  }
  return Result.ok(parsed.output);
}
```

- Use `Result.try` / `Result.tryPromise` narrowly around throwing external APIs, translating expected failures into meaningful error variants. Don't blanket-catch a whole workflow and relabel programming bugs as domain errors.
- Compose Result-returning operations and preserve their error unions. Async operations return `Promise<Result<T, E>>`.
- Infallible functions return plain values. Absence is not automatically an error if it is a valid outcome of the operation.
- Bugs and broken invariants remain exceptional. `better-result` treats unexpected callback failures as defects (`Panic`), not recoverable error variants.
- Adapt to framework-required response or exception conventions at the outer boundary; don't mix competing error models throughout business logic.

## Strict configuration

Use the strictest type-checking settings applicable to the project's installed TypeScript version, runtime, and toolchain. Inspect inherited configuration and project references before changing settings. Document necessary exceptions; fix errors rather than weakening checks. Verify with the project's type-check and relevant runtime-test commands, without turning a local task into an unrelated configuration migration.
