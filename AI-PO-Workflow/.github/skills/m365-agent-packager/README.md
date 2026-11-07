# M365 Agent Packager Skill (`m365-agent-packager`)

Converts any VS Code / GitHub Copilot skill under `.github/skills/<skill-name>` into a **Microsoft 365 Copilot Agent Builder** skill under `o365-copilot-agent/<skill-name>/` and generates the raw English agent instructions in `o365-copilot-agent/AGENT-INSTRUCTIONS.md`.

## What It Does

1. **Adapts the Skill for M365 Copilot (`o365-copilot-agent/<skill-name>/`)**:
   - Preserves the multi-file structure (`SKILL.md` + `reference/*.md`) while keeping `SKILL.md` under the 20,000-character limit.
   - Normalizes YAML frontmatter (`name` and single-line `description`).
   - Replaces VS Code–only tools (such as `ask_user`) with conversational single-question turns in chat with inline options.
   - Replaces local `docs/` filesystem write assumptions with M365 Copilot artifact delivery (downloadable Markdown files when supported or copyable Markdown sections in chat).
2. **Generates Raw English Agent Instructions (`o365-copilot-agent/AGENT-INSTRUCTIONS.md`)**:
   - Produces the raw English instruction text (strictly `< 8,000` characters) ready to copy and paste directly into the M365 Copilot Agent Builder **Instructions** field.
   - References the attached custom skill and its `reference/` templates while enforcing interactive stage-by-stage facilitation, scope guardrails, and M365 delivery rules.
3. **Leaves Zipping to You**:
   - Does not auto-generate a `.zip` file so you can inspect and zip `SKILL.md` + `reference/` manually when ready.

## Usage

Invoke `/m365-agent-packager` in Copilot Chat, or ask:
- *"Use `m365-agent-packager` to convert `product-owner` to the O365 agent skill and update `AGENT-INSTRUCTIONS.md`."*
