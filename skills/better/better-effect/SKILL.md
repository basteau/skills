---
name: better-effect
description: "Effect v4 guidance for implementation, debugging, review, and upgrades; load only the topics your task needs."
disable-model-invocation: true
---

# Better Effect

Use this skill for **Effect v4** implementation, debugging, explanation, review,
and upgrades within v4. Confirm the workspace's resolved major version before
applying it; other majors and cross-major migrations are outside its scope.

Read the installed package's `AGENTS.md` when present, then the relevant topic
examples. Use installed declarations and source to settle API differences. For
missing packages or conflicting guidance, read
[source resolution](references/source-resolution.md).

| Task concerns | Read |
| --- | --- |
| Missing export, conflicting examples, version or import uncertainty | [Source resolution](references/source-resolution.md) |
| Effect creation, async adapters, typed failures, recovery, retries | [Effects and errors](references/effects-errors.md) |
| Dependency injection, services, Layer composition, configuration | [Services and layers](references/services-layers.md) |
| Parsing, models, brands, optional fields, encoding | [Schema boundaries](references/schema-boundaries.md) |
| Shared state, atomic updates, subscriptions, transactions, UI atoms | [State management](references/state-management.md) |
| Retries, timeouts, polling, scheduled jobs, dates and time zones | [Time and scheduling](references/time-scheduling.md) |
| Cleanup, fibers, parallelism, queues, streams | [Lifetimes and concurrency](references/lifetimes-concurrency.md) |
| Cache keys, TTL, memoization, batching | [Caching and batching](references/caching-batching.md) |
| Runtime hosts, HTTP, SQL, RPC, AI, CLI, distributed features | [Integrations](references/integrations.md) |
| Tests, debugging, clocks, logs, traces, semantic review | [Verification](references/verification.md) |
| Explicitly requested upgrade between v4 releases | [V4 upgrades](references/upgrades.md) |

Read only the references needed for the task. For example, a leaking HTTP stream
needs lifetime guidance; an unresolved service requirement needs layer guidance.

Trace the affected operation's success `A`, expected error `E`, required services
`R`, resource owner, and runtime boundary. Follow the project's conventions and
check commands. Keep reviews read-only unless fixes are requested; change
packages only when setup or an upgrade is part of the request.

For code changes, finish when the requested behavior works, changed type contracts
pass the project's checks, and tests cover the affected failure or lifetime
behavior. For explanations, answer from the relevant source; for reviews, report
source-backed findings. State unresolved checks and version-dependent assumptions.

Read [sources and maintenance](references/sources.md) when refreshing this skill.
