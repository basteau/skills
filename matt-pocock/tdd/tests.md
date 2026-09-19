# Tests that survive refactoring

Test behavior at an appropriate interface. A good regression gives a concrete input and an independently known result; the implementation can change without rewriting the assertion.

```ts
// Good: independently known output through the existing public interface.
const result = parseDuration("2h 30m")
expect(result).toEqual({ minutes: 150 })
```

Assert the details relevant to the regression, such as exact values, error types, or locations. For invalid input, verify the documented failure behavior rather than merely checking that execution did not throw. Use positive cases to establish what remains allowed.

Avoid tests that:

- Verify calls between private collaborators instead of returned behavior.
- Recalculate the expected result using the production algorithm.
- Pass because they only assert an empty or truthy value unrelated to the requirement.
- Lock down incidental formatting or internal object structure.
- Use broad snapshots when a few precise assertions explain the contract better.

One test should explain one coherent behavior; several related assertions are fine. Parameterized tests are useful for meaningful input variants. Prefer focused regressions over large fixture matrices without a clear purpose.

Existing module-level tests remain useful when they reach a meaningful behavior boundary. During refactoring, remove a test only after replacement coverage demonstrably preserves its guarantee.
