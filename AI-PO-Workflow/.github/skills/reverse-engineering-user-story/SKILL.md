---
name: reverse-engineering
description: >
  Reverse-engineer an existing codebase into evidence-backed epics, features,
  requirements, architecture documentation, and a backlog using Digital
  Innovation Agents. Use for brownfield onboarding, extracting requirements
  from code, or documenting implemented capabilities.
---

# Digital Innovation Agents: Reverse Engineering User Story

Read [the upstream workflow](upstream/skills/reverse-engineering/SKILL.md)
and follow it for the user's requested scope.

## Local bundle paths

The DIA bundle root is `upstream/` beside this file. Resolve upstream
`skills/...`, `tools/...`, and `docs/...` references against that bundle root.
Named DIA skills such as `/consistency-check` and `/business-analysis` resolve
to `upstream/skills/<name>/SKILL.md`; read those files when the workflow calls
for them. Resolve bare template filenames within the corresponding bundled
skill's `templates/` directory.

Run bundled tools by their absolute paths, with the working directory set to
the codebase being analyzed. If a tool requires `DIA_PLUGIN_ROOT`, set it to
the absolute bundle root for that invocation. Generated `_devprocess/`,
`docs/`, and source documentation belong in the analyzed codebase, not in
this installation directory.

Python-based validation and migration tools require a working Python 3
installation. Check prerequisites before using those tools and report any
checks that could not run. A documentation-only trial can still extract
evidence, but must not claim the automated quality gates passed.

Read [INSTALLATION.md](INSTALLATION.md) for provenance and trial usage.
