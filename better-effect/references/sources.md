# Provenance and maintenance

Synthesized from the user's Effect v4 research pack dated 2026-09-20: 42 repository
snapshots, 218 skill files (including duplicates and historical versions), official
guides, and release probes. The pack informed topic selection and failure cases;
this skill does not vendor its repository trees, community skill text, or harnesses.
It needs no temporary research directory at usage time.

Scope: Effect v4 across releases. Research snapshots establish provenance, not a
required package version or a guarantee that every example fits every v4 release.
Resolve API details from the target project's installed v4 package.

Primary evidence:

- The installed v4 package: packaged agent guide, examples, source, and declarations when available.
- [Pinned canonical source](https://github.com/Effect-TS/effect/tree/7869f54af4bd6b2bd58ff45edcb16ba7fe0a69e4): `LLMS.md`, `ai-docs/src`, `migration`, core source/tests. A source snapshot can include changes beyond the published artifact.
- [Official v4 guides](https://effect.website/docs/v4/getting-started) and [API](https://effect.website/docs/v4/api): human explanations and current navigation; match to the installed version.
- [Official skills](https://github.com/Effect-TS/skills): setup and migration workflow context; their setup actions are not inherited here.

The research also assessed [Kit Langton](https://github.com/kitlangton/skills),
[makisuo](https://github.com/makisuo/skills),
[mpsuesser](https://github.com/mpsuesser/opencode-effect-enforcer), and
[Esteban Marin](https://github.com/EstebanMarin/effect-ts-workshop). These are discovery
and design context, not API authority or bundled dependencies. Author preferences
such as Struct-first versus Class-first were deliberately left as choices.

Maintenance: keep the principles general to v4 and verify examples against the
v4 versions actually being evaluated. Record those versions in validation results
without making them requirements for using the skill. Probe failure versus defect,
Option conversion, and valid dual APIs when relevant. Revisit lifecycle guidance
with behavior tests when semantics change; do not infer compatibility from a date
or global string replacement. Keep research inventories outside the usage path.

Adaptation and license notices: [Acknowledgements](../ACKNOWLEDGEMENTS.md).

Writing review used Matt Pocock's
[writing-for-agents](https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-for-agents/SKILL.md)
and its skill mechanics reference: shorter routing, explicit completion conditions,
and fewer duplicated instructions. These informed editing; they are not runtime
dependencies of this skill.

The source review checked `Effect.ts` and `internal/effect.ts` for acquisition,
fibers, and caching; `Layer.ts` for scope and sharing; `ManagedRuntime.ts` for
runtime ownership; HTTP client modules for status filtering; and the monorepo's
`packages/vitest/src/index.ts` for scoped test signatures. Six isolated runtime
probes covered acquisition failure, partial cleanup, interruption, layer lifetime,
cache allocation, and cached failures. Both documentation examples passed strict
TypeScript checking and runtime checks. These checks used the research artifact;
they do not establish compatibility with every v4 release.
