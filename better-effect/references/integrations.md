# Runtime and ecosystem boundaries

Use the platform's `runMain` at a process entrypoint. Use `ManagedRuntime` to bridge
an existing framework or callback host: build the application graph at its intended
lifetime, reuse it, and dispose it on shutdown. Avoid rebuilding pools and clients
per request unless request isolation requires it. Pass request-specific services
at the request boundary and propagate host cancellation through supported APIs.

Keep HTTP handlers focused on decoding, authorization, invoking the domain service,
and translating outcomes. Prefer Effect platform adapters where they preserve
resource/error semantics; a small native SDK adapter is also valid. Keep effectful domain
operations Effect-returning; pure calculations can remain ordinary functions.

Core APIs are exported from `effect`; integrations such as HTTP, SQL, and RPC
also use `effect/unstable/*`. Runtime implementations, drivers, and providers can
remain in companion packages. Inspect exports instead
of guessing paths from older examples. Align packages from the same release
family; do not assume independent tooling has the same version number. Unstable
modules need exact-version verification even across minor updates.

For outgoing HTTP, decide which status codes are failures. Transport success does
not imply a successful HTTP status; use status filtering such as `filterStatusOk`
when that matches the client's contract, then decode the body.

Find the selected topic in the packaged guide or source. The table names modules
to search rather than depending on numbered example directories.

| Integration | Module / source | Decisions to verify |
| --- | --- | --- |
| Existing host | `ManagedRuntime` | Runtime reuse, disposal, cancellation, error translation |
| Outbound HTTP | `HttpClient`, `HttpClientResponse` | Status handling, body decoding, response lifetime, retry safety |
| HTTP server | `HttpApi`, `HttpApiBuilder`, `HttpApiTest` | HttpApi schemas, middleware services, auth, typed client contract |
| SQL | `SqlClient`, `Model`, selected driver | Transaction scope, rollback, schema mapping, connection release |
| Process / CLI | `ChildProcessSpawner`, CLI modules | Exit status, stderr, cancellation, child cleanup |
| AI and tools | `LanguageModel`, `Toolkit`, selected provider | Input/output schemas, tool authorization, stream interruption, retry cost |
| RPC / sockets | `src/unstable/rpc`, `src/unstable/socket` | Serialization, request scopes, disconnects, buffering |
| Cluster / workflow / persistence | Cluster, workflow, persistence modules | Delivery, replay, idempotency, durable state, shutdown |
| Reactive UI / atoms | [State management](state-management.md), reactivity and framework modules | Registry/request isolation, subscriptions, async state, invalidation |

Do not infer exactly-once external execution from a durable workflow API. Identify
what survives a crash, what is replayed, and what protects a side effect from
repetition. For SQL or RPC changes, tests should exercise the actual transaction
or wire boundary when that boundary carries the risk.
