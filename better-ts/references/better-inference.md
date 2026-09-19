# Better inference

Preserve useful information from values to callers. Add annotations to establish
a contract or expose a mismatch, not to restate everything the compiler knows.

## Choose the operation deliberately

| Need | Use |
| --- | --- |
| Check a value and expose a chosen public contract | A type annotation |
| Check a literal's shape while retaining its inferred detail | `satisfies` |
| Preserve literal values and readonly literal structure | `as const` |
| Derive a type from an existing value | `typeof`, `keyof`, indexed access |
| Preserve call-site literal detail in a generic API | A `const` type parameter, when supported |

`satisfies` performs an assignability check, not a runtime check or cast. Contextual
typing still affects inference: it does not promise every string or number stays
a literal, and a mutable property can become narrower than intended. Inspect the
inferred type and exercise intended mutations.

```ts
type JobSpec = { queue: "fast" | "slow"; attempts: number };

const jobs = {
  build: { queue: "fast", attempts: 3 },
  mail: { queue: "slow", attempts: 1 },
} satisfies Record<string, JobSpec>;

type JobName = keyof typeof jobs;
type Queue = (typeof jobs)[JobName]["queue"];

jobs.build.attempts = 4;
// @ts-expect-error: only declared job names are available
jobs.archive;

const modes = ["read", "write"] as const;
type Mode = (typeof modes)[number];
```

`const status = "ready"` already infers a literal. Properties of a mutable object
or elements of a mutable array commonly widen even when its binding uses `const`.
Find where information was lost before adding assertions downstream.

`as const` does not freeze an object at runtime, and mutable values referenced
inside a literal remain mutable through their existing aliases. Accept readonly
arrays when a function only reads; do not cast them to mutable arrays for convenience.

## Derive when the relationship is real

Use `Pick`, `Omit`, `Parameters`, `ReturnType`, `Awaited`, and indexed access when
the derived contract should change with its source. Prefer an exported domain type
over reverse-engineering a long chain of implementation details.

`Omit<User, "password">` does not remove a password from a runtime object. Construct
an output allowlist when removing fields matters. Likewise, `Partial` and `Readonly`
are type transformations, not patch application or runtime immutability.

Keep single-use types near their implementation. Share them at the narrowest
meaningful boundary when multiple consumers need the same concept; do not create
a global types directory or package merely because a second import exists.

## Annotate stable boundaries

Explicit parameter and return contracts help protect exported APIs, recursive
functions, and functions whose inferred result would expose implementation details.
Let local values and contextually typed callbacks infer their types. Preserve
inference where it is part of a schema, factory, or component API's usefulness.

Use `type` for unions and computed types. Interfaces can express extendable object
contracts; aliases can express object shapes too. Follow local convention unless
declaration merging, diagnostics, or measured checker cost makes the choice matter.

For generic inference failures, continue with [better-generics](better-generics.md).
For assignability errors, use [better-verification](better-verification.md).

Draws on [Matteo Collina's inference patterns](https://github.com/mcollina/skills/tree/main/skills/typescript-magician),
Matt Pocock on [return contracts](https://www.totaltypescript.com/should-you-declare-return-types)
and [type placement](https://www.totaltypescript.com/where-to-put-your-types-in-application-code),
and TypeScript's [`satisfies` semantics](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-9.html#the-satisfies-operator).
