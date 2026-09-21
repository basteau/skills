---
name: better-ts
description: "Explicitly invoked router that loads only the TypeScript guidance relevant to the user's prompt."
disable-model-invocation: true
---

# Better TypeScript

Read the smallest relevant set of references. These are Markdown concepts, not
separate skills. Follow their deeper links only when the task needs that detail.

| Prompt concerns | Read |
| --- | --- |
| Domain states, unions, optional fields, brands, non-empty collections | [better-modeling](references/better-modeling.md) |
| External data, schemas, guards, assertions, runtime validation | [better-boundaries](references/better-boundaries.md) |
| Widening, annotations, `satisfies`, `as const`, deriving and locating types | [better-inference](references/better-inference.md) |
| Generic APIs, constraints, callbacks, overloads, correlated arguments, builders | [better-generics](references/better-generics.md) |
| Conditional or mapped types, `infer`, distribution, template literals, recursion | [better-type-operators](references/better-type-operators.md) |
| Compiler flags, ESM/CJS, module resolution, declarations, JS migration | [better-configuration](references/better-configuration.md) |
| Type errors, narrowing surprises, compiler reproduction, type and runtime tests | [better-verification](references/better-verification.md) |
| Slow checking, editor latency, huge instantiations or declarations | [better-performance](references/better-performance.md) |
| TypeScript review or audit spanning domains | [better-ts-review](references/better-ts-review.md), plus relevant domains |

Route by the problem, not isolated words. An HTTP response typed incorrectly needs
boundary guidance; a generic that loses its return type needs inference or generics.
A compiler configuration question does not require an application-wide type audit.
Load all domains only when the requested scope needs them.

Identify the target project's compiler version, runtime, configuration, conventions,
and check commands before applying version-sensitive advice. Use its installed
toolchain and existing schemas, libraries, and error conventions. Prefer the
simplest accurate type; advanced machinery must preserve a useful relationship.

Answer, build, fix, or review as requested. Reviews are read-only unless fixes were
requested. Do not expand a local fix into a dependency upgrade, framework migration,
configuration overhaul, or repository-wide ban. If scope is missing, use task
context or ask for the missing problem.

Static types describe contracts; runtime code must uphold them. Verify changed
contracts with relevant compiler checks and behavior tests, and state what remains
unverified.
