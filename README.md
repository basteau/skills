# Personal skills

Reusable agent skills, adapted to work with the target project's conventions.

## Setup

Clone this repository, then ask your agent:

> Find every SKILL.md recursively. Link each containing folder into
> ~/.agents/skills under its skill name. Include companion skills, preserve
> existing installations, and report conflicts.

Use the harness's supported skill directory if it differs. Keep the clone in
place so updates reach the links; reload the agent after setup.

## Use

Invoke a skill by name with your request. Explicit-only routers select the
relevant references; those references are not separately installed skills.

Each skill describes its scope and any dependencies. Review changes before
updating. Repository maintenance instructions stay local to this collection.
