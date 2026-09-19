# Interface review

Use this reference for a review spanning domains. A focused build, fix, or question
needs only its relevant domain guidance, not this review workflow.

## Scope and evidence

Resolve the requested screen, flow, feature, or change first. For a branch, commit
range, pull request, or uncommitted work, inspect the relevant diff and surrounding
behavior; distinguish introduced issues and regressions from pre-existing ones.
No separate review skill is required.

Identify the project's platform, components, styling, tokens, conventions, and
verification commands. Inspect the relevant normal, empty, loading, error, and
constrained-size states. For a scope too large to inspect credibly, choose a complete
flow centered on the request and state the boundary. Never imply unseen coverage.

Every finding needs source evidence at `path/to/file:line`, or an exact screen and
component when source is unavailable. Inspect runtime behavior when appearance,
input, or motion determines the result. A taste preference alone is not a finding.

## Select the domains

Read only the domains needed for the requested review. For an explicitly
comprehensive review, cover the applicable domains in this order:

1. [better-accessibility](better-accessibility.md)
2. [better-layout](better-layout.md)
3. [better-writing](better-writing.md)
4. [better-typography](better-typography.md)
5. [better-colors](better-colors.md)
6. [better-polish](better-polish.md)
7. [better-animation](better-animation.md)
8. [better-pwa](better-pwa.md) for mobile web and installed web apps

These are local Markdown references. Follow their supporting links only when a
finding or verification step needs them. Take domain principles and checks from
them; the shared severity and report format here replace their individual formats.

Animation owns motion behavior and performance. PWA owns mobile browser viewport,
touch, scroll, and chrome behavior. Accessibility owns reduced-motion
preferences, equivalent input access, and static state cues. Colors owns contrast
measurement and palette fixes; accessibility identifies the applicable requirement.
Assign one root cause to one owner and mention secondary effects in its explanation.
Apply each rule in the target platform's idiom.

## Rank by user impact

- **HIGH:** blocks a task, misleads users, hides content or controls, risks data
  loss, or creates a repeated systemic failure.
- **MEDIUM:** meaningfully harms comprehension, efficiency, adaptability, or consistency.
- **LOW:** isolated polish with limited task impact.

Confirmed accessibility and task blockers take priority: an unnamed control,
missing visible focus, an inaccessible input path, ignored reduced-motion settings,
failed required contrast, color-only or motion-only meaning, unreachable content,
a destructive action with no confirmation, undo, or distinct treatment, or an error
with no recovery path. Apply supported viewport and scaling requirements, including
320px reflow and 200% zoom where the web rules apply.

Within a severity, prioritize reach and the benefit of a shared fix. Report a token
or component cause once with all confirmed affected locations. Do not turn an
unverified risk or disagreement with a deliberate style choice into a blocker.

## Prefer the smallest effective fix

Consider removing unnecessary decoration, using platform behavior, reusing existing
components or tokens, and correcting values before adding new machinery. Preserve
the user's requested design and functionality. Fix the shared cause rather than
patching each visible symptom.

## Verify and report

Reviews are read-only unless fixes were requested. When implementing findings,
keep to the agreed scope and re-run relevant verification. Inspect rendered states
and run the project's safe, relevant checks. Name unavailable checks as not verified.

Use [the review format](better-interface/review-format.md) for one ranked report.
Aim for at most 15 actionable findings, with blockers first; explain any omitted
coverage or findings rather than silently hiding blockers. No findings is valid.
For change reviews, separate existing issues and keep them out of the change verdict.
