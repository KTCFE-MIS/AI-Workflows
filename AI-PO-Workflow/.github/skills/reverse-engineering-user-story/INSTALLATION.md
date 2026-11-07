# Installation

- Requested listing: https://tessl.io/registry/skills/github/salemaziel/digital-innovation-agents/reverse-engineering
- Downloaded repository: https://github.com/salemaziel/digital-innovation-agents
- Source: `main` archive, downloaded 2026-11-04.
- Archive SHA-256: `1A31EBB5CE026F061E051E60D398D47194A5C3C8A59E03B9F8CB84B476EC0618`.
- License: [MIT](upstream/LICENSE).

The upstream `skills/`, `tools/`, and `docs/` directories are bundled unchanged
to preserve cross-skill dependencies, templates, and helper scripts. The local
`SKILL.md` is an entry point that resolves upstream paths for this installation.
Upstream repository hooks and platform configuration were not installed.

## Try it

Ask your coding assistant:

> Use `.github/skills/reverse-engineering-user-story/SKILL.md` to analyze [codebase path].
> Use PoC scope and generate evidence-backed epics, features, and a BA draft.
> For this trial, write documentation only; leave commits, tags, PRs, hooks,
> migrations, and source-code edits out of scope. Flag user stories that need
> business input.

PoC scope includes feature extraction. Upstream Simple Test scope only runs
phases 0–2, producing technical context and ADRs rather than epics/features.
User stories can require the subsequent business-analysis and
requirements-engineering workflows, both included in the bundle.

Python was unavailable during installation. The bundled Python tools were
not executed; install Python 3 before requesting automated validation or
migration. Installation alone does not run reverse engineering.
