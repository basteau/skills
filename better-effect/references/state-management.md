# State management

Choose state by its owner, update rules, and readers. Allocate shared state during
service construction; allocate request state per request. Expose domain operations
rather than a writable ref when callers must preserve an invariant. A module-level
mutable value can leak between requests, tenants, or tests.

| Need | Choose |
| --- | --- |
| Pure state transition | Ordinary immutable values and functions |
| Shared value with synchronous atomic updates | `Ref` |
| Serialized update that computes its next value effectfully | `SynchronizedRef` |
| Current value plus a stream of updates | `SubscriptionRef` |
| Atomic changes across several independent cells | One combined Ref, or `TxRef` with `Effect.tx` where available |
| UI-derived state and Effect-backed queries | Atom APIs and the framework integration |
| State that survives restart or spans processes | Database or durable storage with its own concurrency guarantees |

## Atomic updates

Use `Ref.update` or `Ref.modify` for read-modify-write operations. Separate `get`
and `set` operations can lose updates when another fiber runs between them.
`modify` returns `[result, nextState]`; keep its callback synchronous and pure.
Replace object and collection values instead of mutating objects already handed
to readers. Ref atomicity does not make a contained JavaScript object immutable.

```ts
import { Effect, Ref } from "effect"

export const makeTickets = Effect.gen(function*() {
  const next = yield* Ref.make(0)
  return {
    take: Ref.modify(next, (id) => [id, id + 1] as const)
  }
})
```

With `SynchronizedRef.modifyEffect`, the lock covers computation of the new value;
the stored value changes only on success. Keep that computation bounded. Updating
the same ref recursively can deadlock. Failure leaves the old stored value, but
cannot undo external writes or in-place object mutation performed by the callback.

For an invariant across several fields, prefer one immutable record updated in
one `modify`. When separately owned transactional cells must change together,
inspect `TxRef` and `Effect.tx`. Transaction bodies can run again after conflicts;
keep irreversible I/O outside them. An in-memory transaction does not make a
database write or HTTP call atomic with the ref updates.

## Subscribers and UI state

`SubscriptionRef.changes` emits the current value followed by updates. Read through
that stream when a separate read-then-subscribe sequence could miss a change.
It models observable state, not a durable event log. Scope subscriptions, consider
slow consumers, and use Queue/PubSub when work delivery or event fan-out is the
actual requirement.

For reactive UI, inspect `Atom`, `AtomRegistry`, and `AsyncResult` under the
installed reactivity modules. A registry owns evaluation, subscriptions, and
cached values; the same atom can have different values in different registries.
Choose registry lifetime explicitly, especially for SSR request isolation.
Keep atom identity stable where state must persist. Model loading, failure, stale
success, refresh, and mutation invalidation instead of duplicating server state
in unrelated writable atoms. Use framework adapters to own subscriptions.

Verify concurrent updates, failure before commit, subscriber startup, and state
isolation between service instances or requests.

Lookup: `Ref.ts`, `SynchronizedRef.ts`, `SubscriptionRef.ts`, `TxRef.ts`,
`Effect.ts` (transactions), and `src/unstable/reactivity` in the installed package.
