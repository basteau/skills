# Verify semantics, not stylistic compliance

Use the target project's compiler, diagnostics, and test commands. First reproduce
the failure at its real boundary; reduce it if version behavior is uncertain.
Effect-aware tests and explicit test layers preserve the environment contract.
Do not import another repository's custom `testEffect` wrapper by assumption.

## Build the test boundary

Test pure transformations directly. For effectful services, exercise the real
service implementation with small test layers for external dependencies. Supply
those layers before construction so the service captures the intended test
implementation. Match the service interface, including typed failures; a success-only
stub cannot verify recovery behavior.

Allocate mutable test state per test unless sharing is the subject of the test.
The `@effect/vitest` `layer(...)` helper shares a layer across its test block, so
state can persist between tests. Use fresh provisioning or reset state deliberately
when tests need isolation. Keep a separate integration test for adapters whose
wire protocol, transaction, or cleanup cannot be established by an in-memory fake.

Assert the expected error tag or inspect `Exit` for failure/defect/interruption
behavior. For cancellation tests, wait until acquisition or subscription is ready,
interrupt, await termination, then assert cleanup. For retries, count attempted
operations and use TestClock for delays; also verify a permanent error is attempted
only once under the chosen policy.

| Changed contract | Useful evidence |
| --- | --- |
| Expected failure / recovery | Exact typed error or Exit; unrelated failures still propagate |
| Resource ownership | Each acquired resource releases once; failed acquisition cleans up partial state |
| Shared state | No lost updates, preserved invariants on failure, subscription startup, request isolation |
| Layer sharing | Acquisition count and release at intended owner shutdown |
| Retry / timeout | Attempt count, delay/budget, cancellation, external idempotency |
| Schema | Invalid input, missing/null/default distinctions, decode and encode |
| Parallel work / streams | Capacity/order guarantees, interrupted siblings, short-circuit cleanup |
| Cache | Concurrent misses, TTL, failure policy, key isolation, invalidation |
| Framework bridge | Error response, request cancellation, runtime disposal |

Use `TestClock` for clock-dependent unit behavior and deterministic synchronization
such as Deferred for readiness. Avoid arbitrary sleeps. Real sockets, subprocesses,
and filesystem integration can require live test services; virtual time does not
advance external processes. In `@effect/vitest`, `it.effect` supplies test services
and Scope; `it.live` supplies Scope with live services. Check the installed tester
signature before adding a separate scope wrapper. Import test services such as
`TestClock` from `effect/testing` when supported by the target package.

Keep span boundaries around meaningful operations. Structured logs should retain
useful domain/request identifiers and preserve failure propagation. Keep secrets
out of telemetry. Testable current time
should come from Effect's clock-aware APIs rather than hidden `Date.now` calls.
Use the existing telemetry setup; read the packaged observability examples only
when changing it. Measure operation latency, failures, retry counts, and queue
saturation where they explain service behavior. Keep metric label values bounded;
request IDs belong in logs or traces, not as unbounded metric dimensions. Propagate
trace context at transport boundaries and scope request annotations to the request.

## Review mode

Trace one relevant path from input → decode → service → dependency/resource →
recovery → output → test. Report concrete impact, location, triggering conditions,
and a narrow fix. Expand to other paths only when the review scope warrants it.
Treat syntax searches as candidates, never proof of bugs.

Base findings on observable effects: lost cancellation, an incorrect fallback,
missing provisioning, premature scope closure, unsafe retries, or unchecked
external data. Resolve disputed APIs using [source resolution](source-resolution.md)
and distinguish project conventions from correctness requirements.

A typecheck proves neither cleanup nor delivery semantics. Say which behavior was
actually exercised. Existing Effect diagnostics can help if they support the
project's Effect/TypeScript versions; installing language tooling is a separate
setup task, not a prerequisite for a focused change.
