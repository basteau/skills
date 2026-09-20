# Resolve evidence before syntax

From the workspace containing the affected file, inspect package manifests,
lockfile, overrides/patches, and the actual resolved package. A root dependency
range or registry tag does not prove which copy a workspace imports. Use its
package manager's resolution facilities; for ordinary Node resolution:

```sh
node -p 'require.resolve("effect/package.json")'
node -p 'require("effect/package.json").version'
```

If package exports or the host prevent this, resolve `effect` through the project
runtime and locate the owning manifest. Do not install a different version just
to make lookup work. Also resolve runtime, driver, framework, and test packages.

Evidence order: resolved artifact and patches → its declarations/implementation
and packaged examples → exact-release upstream tests → version-matched official
guides → current main/release discussions → community guidance. An example can
be stale even in official migration prose. Confirm disputed behavior with a small
reproduction; confirm accepted argument types with the target compiler.

Look for `AGENTS.md`, `ai-docs/src`, `src`, and `dist` in the installed package.
Read the guide when present; search selected modules instead of dumping directories.
For example, with `effect_dir` set to the resolved package directory:

```sh
rg -n 'cachedWithTTL|fromOption' "$effect_dir/src/Effect.ts"
rg -n 'provideMerge|fresh' "$effect_dir/src/Layer.ts"
rg --files "$effect_dir/ai-docs/src"
```

When local docs are missing, use the exact release in
[Effect source](https://github.com/Effect-TS/effect), or the explicitly versioned
[v4 guides](https://effect.website/docs/v4/getting-started) and
[API reference](https://effect.website/docs/v4/api). Current main can be ahead of
published packages. Archived `effect-smol` is historical evidence.

## Check compatibility without freezing recipes

V4 releases can differ in service constructors, schema error constructors, Config
casing, recovery combinators, scoped layer APIs, and arbitrary generation. When a
recipe fails, inspect the relevant module's exports and signature before choosing
a replacement. Do not turn one release's rename into a rule for every v4 release.

For values such as Option, check explicit conversions (`Effect.fromOption`) rather
than assuming an iterator or an `.asEffect()` method makes them Effect-compatible.
For scoped acquisition, inspect the installed Layer and Scope semantics rather
than guessing an API from its name.

Do not reject `Data.TaggedError`, `Effect.cachedWithTTL`, or data-first
`Effect.mapError(effect, f)` based on community prohibition tables. Determine
availability from the installed package, then judge whether the semantics fit.
Separate a missing API from a valid API used incorrectly or a mere style choice.
