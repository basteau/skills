# Personal skills

Clone this repo and ask your agent to set up the skills you want.

| Folder | Contents |
| --- | --- |
| `show-me/` | HumanLayer's visual explanations |
| `agent-browser/` | Vercel's browser automation |
| `unslop/` | poteto's prose editing |
| `install-anti-slop/` | Dillon Mulroy's Oxlint setup |
| `better-ui/` | One explicit-only router over interface references from Jakub Krehel and Emil Kowalski |
| `matt-pocock/` | Ten adapted planning, implementation, and review skills |
| `engineering-principles/` | Compact, language-independent principles distilled from pstack |

## Setup

> Find every SKILL.md in this repo. Link its containing folder into
> ~/.agents/skills under its skill name. Include nested folders and companion
> skills. Preserve existing installations and report conflicts.

[Pi](https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/skills.md#locations)
and [Codex](https://learn.chatgpt.com/docs/build-skills) support that location.
Use `.agents/skills/` inside a project for project-only setup. Keep the clone in
place; `git pull` updates linked files. Reload your agent after setup.

## Matt Pocock workflow

`grill-me` → `to-spec` → `to-tickets` → `implement`

Companions: `grilling`, `tdd`, `code-review`, `codebase-design`,
`improve-codebase-architecture`, and `wait-what`. Install the whole family;
its skills reference one another.

Adapted from the setup in Selfix. Specs and tickets live in the target project's
`.agents/tickets/`. Checks and domain constraints come from that project;
commits use Conventional Commits. No glossary or tracker service is required.

## Notes

- `engineering-principles` stands alone: experience, design, debugging, models,
  ownership, delivery, automation, and verification. Use it alongside any workflow.
- `agent-browser` needs its [CLI and browser](https://github.com/vercel-labs/agent-browser#installation).
  Its full workflows come from the installed CLI version.
- `install-anti-slop` needs Node and the target project's package manager.
- Invoke `better-ui` explicitly with your prompt, for example `$better-ui fix the
  dropdown animation`. It loads only the relevant Markdown concepts and supporting
  references. Full cross-domain reviews are available when requested.
- `better-ui/` contains only one skill. Accessibility, layout, writing, typography,
  colors, polish, animation, mobile web/PWA, and review guidance ship with it as
  references. When upgrading an older setup, replace the old individual `better-*`
  links with the single `better-ui` link after checking their targets.
- `show-me` includes a macOS `open` example; adapt it on other systems.

Review diffs when updating. This repo's `AGENTS.md` is maintenance guidance, not
global setup.
