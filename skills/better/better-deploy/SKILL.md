---
name: better-deploy
description: Deploy projects to exe.dev over SSH, manually or through GitHub/GitLab CI, with a repeatable workflow in DEPLOY.md. Use when explicitly requested.
disable-model-invocation: true
---

# Better Deploy

Use the project's tools and the simplest suitable deployment. Brief downtime is fine;
recovery and zero-downtime releases are optional. Finish steps in order unless noted.

## 1. Resolve

Read project instructions, `DEPLOY.md` (or its existing equivalent), and relevant
scripts/runtime definitions; derive commands from them and the installed tools.
Settle these once, reopening only if inputs change; ask only material open choices:

- **Source:** an exact revision. Ask whether to include uncommitted pre-existing
  application changes unless specified; deployment files created in this task need
  no question. Deploying local changes need not require committing or pushing.
- **Target:** default to one VM per project with a readable project-derived name.
  Inspect existing targets; ask if ownership or intent is ambiguous.
- **Access:** public or private web access. Use the provider URL unless a custom
  domain is already configured or requested.
- **Scope:** manual, unless automation or CI trigger changes are requested; then
  also follow [CI deployment](references/ci.md).

## 2. Preflight

Before builds, uploads, or remote changes, run required local tools (such as the
package manager's `--version`) rather than only locating them; report a broken
launcher's cause and change tool installation or invocation only with approval.
Confirm provider access read-only. Create and remove a probe under every remote path
the deployment mutates, including parents holding locks or releases; fix ownership
(explicitly, when provisioning) before uploading. Local preparation may proceed while
a new target is provisioned. Send each remote command as one correctly quoted string;
for any SSH failure, including to the VM, use [SSH access](references/ssh-access.md).

For exe.dev, use `DEPLOY.md` first (project memory, not a frozen specification), then
`ssh exe.dev help` and `help <command>` for operations this run needs; read a
[docs](https://exe.dev/docs.md) page only if a question remains; stop once answered.

## 3. Prepare

Run the project's checks and production build for the selected source, reusing
results while source, configuration, and dependencies are unchanged. On a first
deployment, fit transfer and build location to the runtime (rsync, Git, or images),
reusing existing tooling; supervise long-running services across disconnects and
reboots; keep provisioning and data setup out of recurring commands. Follow existing
database/service arrangements, else the simplest setup on the same VM. Keep data and
secrets outside replaceable code; get missing secrets via the project's mechanism.

## 4. Deploy

Transfer only the selected inputs, preserving remote secrets and data (also under
rsync deletion). Run dependency, build, and migration steps in project order,
stopping dependent steps on failure. Prepare before interrupting the app; restart only
affected services. Before risky migrations, secure a backup or maintenance step;
reverting code does not reverse data. Hold one per-target lock from build selection
through activation, or refuse to activate a release older than the active one.

## 5. Verify and record

Check the live target through the exact recurring entry point; run a created or
repaired deploy script end to end before calling it verified:

- **Identity:** served content or a release marker matches the new artifact. HTTP 200
  may be the old release; redirects and login pages prove no identity or behavior.
- **Access:** anonymous requests reach a public site; a private one requires login.
- **Behavior:** one meaningful behavior works (a non-index route or asset for static
  sites; a service or worker check where there is no web page).

On failure, inspect logs and use an established safe recovery path if one exists.
Keep `DEPLOY.md` short, reusing an existing equivalent: target, URL, access mode,
first-time setup, and tested deploy/verify commands with directories, preferably
project scripts; add services, secret names/locations, recovery notes, or limitations
only where relevant. Never record secret values; leave no empty sections.

Done means verification passes and `DEPLOY.md` is updated (CI setup: ci.md's ready
and verified states). Report configured versus deployed and verified state, calling
partial evidence partial. Commit, push, or install skills only on separate request.
