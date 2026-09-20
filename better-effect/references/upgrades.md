# Upgrades within v4

Use this reference only for a requested change between v4 releases. Confirm both
source and target are v4, and establish the intended scope. Ordinary usage does
not require upgrading. Migrations between major versions are outside this skill.

1. Read the target release notes and inspect its exports and relevant signatures.
   A registry tag does not establish the major version or compatibility.
2. Check companion runtime, driver, framework, and test package compatibility.
   Update the affected workspace using its package manager, preserving unrelated
   dependencies and local changes.
3. Follow changed APIs through one coherent dependency path. Use compiler feedback
   to find affected callers; do not hide errors with casts or blanket suppression.
4. Exercise changed behavior, especially cleanup, cancellation, recovery,
   serialization, equality/keys, and integration boundaries.

| Changed area | Verify beyond spelling |
| --- | --- |
| Services and layers | Construction inputs, exposed outputs, memoization, acquisition lifetime |
| Errors and fibers | Recovery coverage, cause handling, interruption, ownership, observed outcomes |
| Schema | Decoded/encoded types, service requirements, defaults, optional keys, transformations |
| Runtime and Scope | Host execution, disposal, finalizer ordering, request isolation |
| Scheduling and caching | Retry budgets, timing, cache keys, failure policy, invalidation |
| Platform and unstable modules | Export paths, wire contracts, buffering, driver/provider compatibility |

Prefer release-specific upgrade notes over historical rename tables. Inspect
[source evidence](source-resolution.md) when documentation and code disagree.
For an uncertain behavior, reproduce it against the source and target packages
without changing the application's runtime merely to investigate.

If target evidence is unavailable, state which API or behavior remains unresolved.
Do not infer a shipped fix from a closed issue or assume every v4 release shares
identical signatures. Report the versions actually checked and the remaining
verification limits.
