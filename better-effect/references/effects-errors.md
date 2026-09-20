# Effects and errors

Treat `Effect<A, E, R>` as a lazy computation with an explicit failure and service
contract. Use `Effect.gen` for sequential workflows; `Effect.fn("Domain.action")`
for reusable operations that deserve a span, and `fnUntraced` when they do not.
Keep simple `map`/`flatMap` compositions simple. In generators, use `return yield*`
for early failures so TypeScript sees the control-flow exit.

| Boundary | Constructor / decision |
| --- | --- |
| Already computed value | `Effect.succeed`; does not defer its argument |
| Synchronous side effect expected not to throw | `Effect.sync`; thrown exceptions become defects |
| Synchronous API with expected exceptions | `Effect.try` with an explicit typed error mapping |
| Promise API that may reject | `Effect.tryPromise`; create the Promise inside the thunk |
| Callback registration | `Effect.callback`; return cleanup for cancellation |
| Optional result | Explicit conversion such as `Effect.fromOption`, then map the missing-value error |

Pass the supplied AbortSignal through Promise adapters where the external API
supports cancellation. Interrupting the fiber cannot undo an already committed
external write, and an uncancellable SDK can keep running after interruption.
Compose with `yield*` or Effect combinators; run the result at the host boundary.

Expected domain failures belong in `E`; defects represent unexpected failures;
interruption belongs to lifecycle control. Preserve a useful cause when adapting
external errors. Use `Schema.TaggedError` when errors need schemas or wire
encoding, `Data.TaggedError` when they do not. Choose by the error contract, not by a blanket preference.

Recover the smallest error set at the boundary that can produce an honest result.
Use `catchTag` or `catch` for typed recovery. Cause-level recovery requires checking
that defects and interruption keep their intended behavior. Logging an error and
returning an empty value changes the contract; do it only for a real fallback.
`runSync` can throw; an Exit runner is appropriate when the host needs an outcome.

This adapter preserves a JSON syntax error in `E`. Its result is still `unknown`;
[decode its shape](schema-boundaries.md) before domain use:

```ts
import { Data, Effect } from "effect"

class InvalidJson extends Data.TaggedError("InvalidJson")<{
  readonly cause: unknown
}> {}

export const parseJson = (input: string) => Effect.try({
  try: (): unknown => JSON.parse(input),
  catch: (cause) => new InvalidJson({ cause })
})
```

Before retrying, identify transient errors, retry count/time budget, delay/backoff,
and whether the operation is idempotent or protected by an idempotency key.
A timeout may leave the external outcome unknown. Do not retry permanent schema,
authorization, or business errors indiscriminately. `retry` responds to failure;
`repeat` responds to success. For deadline placement, backoff, and polling, read
[time and scheduling](time-scheduling.md).

Lookup: `src/Effect.ts`, `src/Cause.ts`, `src/Schedule.ts`.
