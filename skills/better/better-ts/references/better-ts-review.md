# Better TypeScript review

Use this reference for a requested review or audit. A focused implementation or
question needs its relevant domain guidance, not this workflow. Reviews are
read-only unless fixes were requested.

## Establish scope and evidence

Resolve the files, change, API, or flow being reviewed. Inspect the relevant diff,
callers, configuration, and tests. For change reviews, distinguish regressions
from existing debt. Identify compiler and runtime versions before applying advice
that depends on them. State coverage limits instead of implying a complete audit.

Select only the applicable domains through the router. Comprehensive reviews should
consider modeling, boundaries, inference, generic contracts, transformations,
configuration, verification, and checker cost. An absent concern needs no invented
finding. Do not turn a runtime defect into a type-only fix.

## Prioritize observable problems

- **High:** admits invalid data into a trusted path, loses data, breaks a supported
  consumer, or makes a material runtime guarantee the implementation cannot uphold.
- **Medium:** loses important inference, rejects valid calls, permits meaningful
  invalid combinations, or introduces demonstrated maintenance or checker cost.
- **Low:** local clarity or consistency with a concrete benefit.

An `any`, assertion, interface, alias, or missing annotation is not by itself a
finding. Explain the failing contract and affected usage. A type assertion behind
a validated adapter differs from unchecked JSON accepted as a domain object.
Respect intentional project conventions and supported compiler versions.

Report each root cause once. Assign ownership to the earliest inaccurate contract:
modeling defines valid states, boundaries establish runtime trust, inference and
generics preserve relationships, and configuration determines checking and execution.

## Keep the result actionable

For each finding provide severity, an exact source location, a failing or misleading
use, its impact, and the smallest effective correction. Include evidence from the
compiler or runtime when the claim depends on it. Prefer a short ranked list over
an exhaustive style checklist; no findings is a valid outcome.

End with checks performed and meaningful gaps. When implementing requested fixes,
verify both the corrected contract and any changed behavior. Do not describe a
successful typecheck as proof of runtime validation or package compatibility.
