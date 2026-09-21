---
name: better-ts
description: "TypeScript best practices. Use when reading, writing, or reviewing TypeScript, including embedded TypeScript in framework components."
---

# TypeScript best practices

Make illegal states unrepresentable; parse external data into trusted types.

| Rule | Summary |
|------|---------|
| Discriminated unions | Model variants with a `kind` literal discriminant so impossible states can't be represented. No optional-field bags. |
| Branded types | Brand primitives with `& { readonly __brand: "X" }` so they can't be mixed up. Validate once at creation. |
| `unknown` over `any` | External data is `unknown`. `any` disables type checking everywhere it touches. |
| No `as` casts | Avoid unchecked assertions. Narrow or parse instead; isolate unavoidable casts behind a verified invariant. `as const` is not a payload assertion. |
| Narrowing hierarchy | Discriminant switch > `in` operator > `typeof`/`instanceof` > user-defined type guard > `as`. |
| Type guards | Must verify the claim. A lying guard is worse than `as` because the bug hides behind a name that says it's safe. Name them `isX` or `hasX`. |
| Exhaustiveness | Inline `const _exhaustive: never = x;` in default arms so the compiler errors when a new variant is added. |
| `satisfies` over `as` | Checks assignability without replacing the expression's inferred type. Not runtime validation. |
| Boundary validation | Always parse external `unknown` with a Standard Schema–compliant library when its expected schema is known. Reuse existing schemas, consume the parsed output, and trust validated types inside. |
| Schema-derived types | Infer parsed types from schemas; distinguish input from output. Reuse generated contracts and reach for `Pick`/`Omit`/`Parameters`/`ReturnType`/`Awaited`/`typeof` before declaring a new interface. |
| Object args | Pass objects, not positional, so argument order is self-documenting. Skip on hot paths (per-frame render, tokenizers, parsers). |
| Expected errors | Always represent expected failures with `better-result`'s `Result<T, E>` and explicit `TaggedError` variants. Handle or propagate them; keep defects exceptional and infallible functions plain. |
| Strict configuration | Use the strictest type-checking settings applicable to the project's TypeScript version and toolchain. Document necessary exceptions; fix errors rather than weakening checks. |

Examples: [references/patterns.md](references/patterns.md). Load only the sections relevant to the task.

For advanced TypeScript, refer to the [typescript-magician reference](references/advanced.md).

Use the project's toolchain and check commands; verify changed contracts with type checks and relevant runtime tests. Keep changes scoped to the request rather than starting an unrelated dependency migration or configuration overhaul.
