# Time, schedules, and deadlines

## Bound work

Set a total budget for an operation, then choose per-attempt timeouts and retry
limits within it. A timeout inside retry bounds each attempt; a timeout around the
retrying operation bounds the whole sequence, including delays. Confirm whether
timeout failure is itself retryable and what outcome the caller receives.

Timeout initiates interruption; cancellation-resistant work or finalizers can
delay completion. Connect adapters to AbortSignal where supported. A timed-out
write can have an unknown external outcome, so retry only with the
[idempotency and error policy](effects-errors.md) appropriate to that operation.

Use bounded exponential backoff with jitter for transient contention or outages.
Apply server rate-limit hints where the protocol supplies them. Bound attempts as
well as delay, and account for retries in nested clients to avoid multiplying
requests. Check Schedule composition against the installed APIs.

## Repeat work

Use `retry` for failures and `repeat` for successful iterations. For polling,
decide whether one failed pass stops the worker, retries that pass, or records the
failure and proceeds to the next scheduled pass. Keep the worker owned by its
[service scope](lifetimes-concurrency.md).

`Schedule.spaced` waits after an iteration finishes; `Schedule.fixed` aims for a
regular cadence. Long iterations affect those policies differently. State whether
overlap is allowed; for calendar schedules, specify the time zone and what happens
after downtime. An in-process Schedule alone does not persist missed work or
coordinate a singleton job across replicas.

## Represent time

Use `Duration` for elapsed intervals and `Clock` for testable time reads. Use
`DateTime` for instants, calendar arithmetic, formatting, and time-zone conversion.
Keep elapsed deadlines distinct from wall-clock schedules, which can be affected
by clock changes and daylight-saving transitions. Make units explicit at native
API boundaries.

Test attempt counts, total budget, interruption, and long-running iterations with
TestClock and deterministic readiness signals. Test calendar edge cases with
explicit instants and zones.

Lookup: `src/Effect.ts` (timeout/retry/repeat), `src/Schedule.ts`, `src/Clock.ts`,
`src/Duration.ts`, `src/DateTime.ts`, `src/Cron.ts`.
