# Better boundaries

Find where a claim about data becomes trustworthy. Network payloads, storage,
environment values, messages, and untyped libraries need an explicit contract at
entry. A TypeScript annotation on their result is not runtime verification.

## Parse into the domain

Reuse the repository's schema library and existing schema when available. Derive
types from that schema rather than maintaining a parallel interface and validator.
Distinguish schema input from output when parsing coerces or transforms values.
For a small dependency-free boundary, construct a checked value directly:

```ts
type User = { id: string; displayName: string };

function parseUser(input: unknown): User {
  if (
    typeof input !== "object" || input === null ||
    !("id" in input) || typeof input.id !== "string" ||
    !("displayName" in input) || typeof input.displayName !== "string"
  ) {
    throw new Error("Invalid user payload");
  }
  return { id: input.id, displayName: input.displayName };
}

async function fetchUser(url: string): Promise<User> {
  const response = await fetch(url);
  if (!response.ok) throw new Error(`Request failed: ${response.status}`);
  const payload: unknown = await response.json();
  return parseUser(payload);
}
```

This checks these field types and returns an allowlisted shape. It does not establish
that the identifier exists, belongs to the caller, or satisfies a richer ID format.
Add those checks where the real contract requires them. Choose how to handle unknown
fields from compatibility and security needs; neither dropping nor accepting them
is a universal policy.

A generic `fetchJson<T>()` implemented by asserting JSON to `T` lets the caller
invent evidence. Accept a decoder or use a validated/generated client instead.
Generated declarations alone do not establish what an untrusted server returned.
Handle transport, decoding, and domain failures through the project's error model;
do not replace them with an empty success value or a new Result library by default.

## Guards must justify both outcomes

Prefer compiler-visible narrowing when it expresses the check. A user-defined
`value is T` predicate is trusted by the compiler, not proven by its body. Checking
only that keys exist does not establish their value types.

The false branch matters too: a predicate claiming `value is number` must not also
reject large numbers on business grounds. Otherwise the false branch may wrongly
exclude numbers. Return a boolean for that business test, a parsed result, or a
properly validated branded subset.

Assertion functions must throw or otherwise not return when their claim fails.
Function declarations are convenient; explicitly typed arrow bindings also work:

```ts
const assertString: (value: unknown) => asserts value is string = (value) => {
  if (typeof value !== "string") throw new Error("Expected a string");
};
```

## Keep escape hatches small

Treat unchecked `as`, non-null `!`, and value-level `any` as claims to investigate.
First improve the source contract, narrow the value, or handle absence. `satisfies`
checks assignability; it cannot validate unknown runtime input. `as const` controls
inference and is not the same operation as asserting a payload to a domain type.

An unavoidable assertion belongs in one narrow adapter or constructor, with the
invariant that justifies it and verification of that invariant. Avoid chains such
as `as unknown as T` used only to silence an error. Do not replace a narrow,
explainable assertion with a lying guard. Type-only function constraints have
different variance concerns; see [better-generics](better-generics.md).

Once a boundary has established a contract, pass its domain value inward. Recheck
when trust changes through mutation, persistence, another process, or an untyped
dependency, rather than redundantly validating at every internal function call.

Adapted from [poteto's boundary patterns](https://github.com/backnotprop/pstack/tree/main/skills/typescript-best-practices)
and [Matteo Collina's narrowing guidance](https://github.com/mcollina/skills/tree/main/skills/typescript-magician).
Predicate semantics are explained in [TypeScript's inferred-predicate notes](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-5.html#inferred-type-predicates).
