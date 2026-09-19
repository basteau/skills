# Review output format

Use this for cross-domain reviews. Focused questions, implementations, and fixes
should use the response appropriate to the user's request instead.

## Scope and coverage

State the surface or change reviewed, project conventions, and review boundaries.

| Domain | Evidence inspected | Result |
| --- | --- | --- |
| Relevant domain | Files, components, states, or checks | Findings count, Clear, or Not verified |

For a comprehensive review, include accessibility, layout, writing, typography,
colors, polish, and animation. For a focused review, list the selected domains and
state what is outside scope. Clear means inspected with no actionable findings;
Not reviewed or Not verified needs a reason.

## Findings

One row per root cause, ordered by severity and reach:

| Severity | Domain | Location | Before | After | Why |
| --- | --- | --- | --- | --- | --- |

Use the severity in [better-interface](../better-interface.md#rank-by-user-impact).
Cite `path/to/file:line`, or an exact screen and component when no source exists.
Show the current implementation and concrete correction. Explain the principle
and user impact. Consolidate repeated locations; do not pad the report.

For changes, label findings Introduced, Regression, or Pre-existing. Put existing
issues in their own section and keep them out of the change verdict. With no
findings, state "No actionable interface findings."

## Verification and verdict

List relevant commands or interactions and observed results. Separate passed checks
from unverified ones. End with Block when a HIGH remains, or Approve for the
inspected scope when verification supports it. If missing evidence prevents a
verdict, say verification is incomplete. Never approve coverage you did not inspect.
