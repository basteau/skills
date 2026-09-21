# Lifetimes, concurrency, and repeated work

Assign every resource an owner and a lifetime before choosing a primitive.
`acquireRelease` registers its finalizer after acquisition succeeds. The finalizer
runs when the owning Scope closes, including after use fails or is interrupted.
If acquisition opens one resource and then fails while opening another, the outer
finalizer has not been registered. Acquire each resource with its own finalizer,
or make the acquisition operation clean up its partial state.

Acquisition is uninterruptible by default. Where the installed API supports
interruptible acquisition, opt in only when the acquisition handles cancellation
safely. A hanging uninterruptible acquisition can delay shutdown. Release has no
typed error channel; handle expected cleanup failures inside the finalizer.

Keep the scope open until every consumer finishes. A stream or callback returned
from a closed scope can retain an unusable resource. Use `acquireUseRelease` when
one operation owns acquisition, use, and release; use scoped acquisition when the
resource must be shared within a longer lifetime.

Choose `forkChild` for work owned by its parent fiber, `forkScoped` for work owned
by a scope, and `forkDetach` only for deliberate independence with shutdown and
failure observation. A child stops when its parent terminates; a scoped fiber stops when its scope
closes. For a background layer, use the layer's scope so work survives completion
of the construction effect. Observe the fiber's outcome or define how each failed
pass is handled.

| Need | Candidate and constraint |
| --- | --- |
| One eventual result | `Deferred` |
| Gate that can open/close | `Latch` |
| Limit access to a resource | `Semaphore`; size to resource capacity |
| Work distributed among consumers | `Queue`; choose capacity and overflow policy |
| Every active subscriber receives events | `PubSub`; own subscriber scopes |
| Effectful sequence with pull/cancellation | `Stream`; preserve backpressure |

For mutable values, subscriptions, or atomic updates, read
[state management](state-management.md).

Bound concurrency according to connection pools, rate limits, and memory. Inspect
ordering and failure behavior of parallel operators; racing or failing one branch
can interrupt siblings. Do not replace effectful parallelism with `Promise.all`
inside a generator and lose its lifecycle/error contract.

For independent operations, choose `Effect.all` or `Effect.forEach` with explicit
concurrency. For dependent operations, sequence with `yield*`. Decide whether a
batch fails on the first error, returns a result for every item, or accumulates
validation errors. Where supported, `Effect.all` with `mode: "result"` preserves
each typed success or failure. Defects and interruption still need their intended
semantics; collecting typed results is not a universal catch-all. Read-only
validation and a batch of irreversible writes need different failure policies.

For streams, check who starts/stops the producer, buffer capacity, failure
propagation, subscriber release, and short-circuit cleanup. Avoid collecting an
unbounded stream. At HTTP/RPC/SDK bridges, ensure disconnects stop upstream work
and framing handles chunk boundaries. Verify release-specific collection and
socket APIs; old examples can have different return types or ownership.

Lookup: `src/Effect.ts` (`acquireRelease`, fork APIs), `src/Scope.ts`,
`src/Stream.ts`, and the selected coordination primitive. For memoized or batched
work, read [caching and batching](caching-batching.md).
