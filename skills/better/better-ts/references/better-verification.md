# Better verification

Reproduce a type problem with the project's compiler and configuration. Establish
the baseline, fix its cause, and rerun the relevant check. A clean isolated example
does not prove that the application's build, declarations, or runtime behavior work.

## Diagnose before changing the contract

Use the repository's typecheck command. If invoking `tsc` directly, use the intended
project configuration; passing individual source files does not apply the normal
project config. Build-mode or framework projects may need their existing orchestrator
rather than a standalone `--noEmit` invocation. Separate pre-existing errors from
ones introduced by the requested change.

Read the whole diagnostic chain, including the deepest incompatible property.
Compare the actual and expected types at that boundary. Inspect installed library
declarations, overloads, and inferred intermediate values before adding a cast.
Reduce an unclear failure to a small reproduction with the same relevant flags.

| Symptom | Investigate |
| --- | --- |
| Literal unexpectedly became `string` | Where a mutable object, annotation, or generic widened it |
| Indexed access may be absent | Bounds, key validity, dictionary semantics, and unchecked-index settings |
| Callback or wrapper rejects a function | Parameter variance and loss of argument/return correlation |
| No overload matches | Public signatures and whether a union argument is actually supported |
| Property disappeared from a union | Narrowing, common keys, or a non-distributive transformation |
| Editor and CI disagree | Compiler version, resolved config, file inclusion, dependency versions |
| Types resolve but runtime imports fail | Runtime resolution and emitted artifacts, not just declarations |

Do not assume old narrowing limitations still apply. Modern TypeScript can preserve
some aliased conditions, infer some predicates, and retain narrowing in some closures.
Check the actual version and mutation behavior. Capturing a stable narrowed value can
clarify ownership; asserting across a later mutation hides the problem.

## Test the contract callers experience

Reuse the project's type-testing facilities. A positive test should exercise useful
inference or assignment. Pair it with a narrowly targeted rejected call when invalid
input is part of the contract. A type alias evaluating to `false` is not a failed
test unless something constrains it to the expected result.

```ts
type Equal<A, B> =
  (<T>() => T extends A ? 1 : 2) extends
  (<T>() => T extends B ? 1 : 2) ? true : false;
type Expect<T extends true> = T;

function head<T>(items: readonly T[]): T | undefined {
  return items[0];
}

const value = head([1, 2]);
type HeadResult = Expect<Equal<typeof value, number | undefined>>;

// @ts-expect-error: a possibly empty input cannot promise a number
const definitelyPresent: number = head<number>([]);
```

Equality helpers are useful for ordinary inference regressions, not universal proofs
about `any`, overloads, or all structural equivalences. Prefer assignability tests
when compatibility is the promise. Check that an expected error is the intended one;
`@ts-expect-error` rejects an unused suppression but does not identify its diagnostic.
Keep such negative cases in type fixtures, not ordinary production suppressions.

For library or generic contracts, cover relevant unions, readonly inputs, optional
fields, invalid key/value pairings, and supported compiler versions. Include `never`,
`unknown`, or `any` only when those are meaningful inputs to the utility.

## Verify runtime evidence separately

Type tests cannot prove a parser, guard, assertion, overload, or builder's runtime
claim. Test malformed inputs and both outcomes, including zero, empty strings,
nullish values, and mutation or branching where relevant. A type cast that makes a
fixture compile is not evidence that its input satisfies the boundary contract.

For schema changes, test accepted and rejected payloads and transformations. For
module changes, run the built or packed output in its supported execution path.
For performance claims, use [better-performance](better-performance.md).

Report the behavior or contract changed, the relevant commands and results, and any
remaining uncertainty. Do not require new test infrastructure for a trivial local
annotation that the existing check already covers.

Adapted from [Matteo Collina's diagnostic approach](https://github.com/mcollina/skills/tree/main/skills/typescript-magician)
and [Seth Hobson's type-testing guidance](https://github.com/wshobson/agents/tree/main/plugins/javascript-typescript/skills/typescript-advanced-types).
Version-sensitive narrowing is documented in [TypeScript 5.4](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-4.html)
and [TypeScript 5.5](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-5.html).
