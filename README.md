# Personal skills

Reusable agent skills, adapted to the target project's conventions.
Grouped under `skills/<group>/<name>/`; groups do not change skill names.

## Setup

Preview available skills without installing:

```sh
npx skills add basteau/skills --list
```

Run `npx skills add basteau/skills` to select skills, or add `--skill <name>`.
Select linked companion skills together; the CLI does not resolve dependencies.
Check for existing skill-name conflicts before installing.

For a live checkout, ask your agent to link each skill folder individually into
its supported skill directory, preserving existing installations.

## Use

Invoke a skill by name. Explicit-only routers load relevant references, which
are not separately installed skills. Repository maintenance instructions stay local.

Author credits and license terms: [Acknowledgements](ACKNOWLEDGEMENTS.md).
