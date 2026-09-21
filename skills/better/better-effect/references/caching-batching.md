# Caching and batching

| Need | Choose |
| --- | --- |
| Reuse one computation's result | `Effect.cached` or `cachedWithTTL` |
| Reuse lookups by key | `Cache` |
| Combine calls to a batch-capable backend | `RequestResolver` |
| Share owned resources by key | Inspect `LayerMap` or `RcMap` |

## Own the cache

`Effect.cached(work)` returns an Effect that creates a memoized Effect. Evaluate
the outer Effect once at the cache owner's lifetime and reuse the returned value.
Saving the outer Effect in a variable and evaluating it for every request still
creates a new cache each time.

A memoized Effect retains `work`'s service requirements. Decide which service
context is allowed to populate the shared result. If the value depends on the
request's tenant or authorization, use an isolated cache or include that context
in a keyed lookup. Keep resource-backed values within their resource's lifetime.

## Define cached outcomes

Inspect how the selected API handles success, failure, and interruption. A cached
failure can suppress a later retry; an interrupted first computation can affect
subsequent callers. Choose expiry and invalidation deliberately for each outcome.
Check whether TTL starts at acquisition or completion, how concurrent misses
share pending work, and how writes invalidate stale values.

Use `Cache` for distinct keys. Use `RequestResolver` when the backend can resolve
several requests together; ensure every request receives a success or failure,
including missing results and partial batch failures. Batching alone does not
define cache lifetime or invalidation.

Lookup: `src/Effect.ts` (cache constructors), `src/Cache.ts`,
`src/RequestResolver.ts`, `src/LayerMap.ts`, `src/RcMap.ts`.
