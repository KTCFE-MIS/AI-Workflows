---
name: m365-agent-packager
description: >
  Converts any VS Code / GitHub Copilot skill in `.github/skills/<skill-name>` into a Microsoft 365 Copilot
  Agent Builder skill package under `o365-copilot-agent/<skill-name>/` and generates the corresponding raw English
  agent instructions in `o365-copilot-agent/AGENT-INSTRUCTIONS.md`. Use when converting, syncing, or packaging
  repository skills for O365 / M365 Copilot declarative agents.
disable-model-invocation: true
---

# M365 Agent Packager (`m365-agent-packager`)

You convert skills authored for VS Code / GitHub Copilot (located in `.github/skills/<skill-name>/`) into:
1. **An M365 Copilot Agent Builder skill directory** under `o365-copilot-agent/<skill-name>/`
2. **A raw English agent instruction file** at `o365-copilot-agent/AGENT-INSTRUCTIONS.md` (or `o365-copilot-agent/<skill-name>-AGENT-INSTRUCTIONS.md` if the user asks for a skill-specific instruction file) containing only the instruction text ready to paste directly into the M365 Copilot Agent Builder **Instructions** field.

Do **not** create a `.zip` archive unless the user explicitly asks you to create one; the user packages the files manually.

---

## Microsoft 365 Copilot Platform Constraints

Always enforce these platform limits from Microsoft Learn:

1. **Skill `SKILL.md` character limit**: Must be strictly **under 20,000 characters** (including YAML frontmatter).
2. **Skill YAML frontmatter**:
   - Required at the top of `SKILL.md` delimited by `---`.
   - Must include `name: <skill-name>` and a clean, single-line `description: <concise summary of what the skill does and when to use it>`.
3. **Directory structure & depth**:
   - `SKILL.md` sits at the root of `o365-copilot-agent/<skill-name>/`.
   - Supporting files (such as `reference/*.md`) sit in subdirectories up to a maximum depth of **3**.
   - Allowed resource file extensions: `.json`, `.xml`, `.yaml`, `.yml`, `.ini`, `.config`, `.utf8`, `.docx`, `.doc`, `.docm`, `.pdf`, `.txt`, `.rtf`, `.md`, `.ppt`, `.pptx`, `.ppsm`, `.xlsx`, `.xls`, `.xlsm`, `.csv`, `.tsv`, `.html`, `.htm`, `.png`, `.jpg`, `.jpeg`, `.gif`, `.bmp`, `.log`.
   - Allowed script extensions: `.py`, `.js`, `.mjs`, `.cjs`, `.ts`, `.mts`, `.sh`, `.bash`.
4. **Agent Instructions character limit**:
   - The `o365-copilot-agent/AGENT-INSTRUCTIONS.md` file must be strictly **under 8,000 characters**.
   - Written in clear, actionable **English** (while specifying the target user interaction and output language required by the skill, e.g., Traditional Chinese).
   - Must contain **only** the instruction text to paste into Agent Builder (no wrapper setup guide, no surrounding code fences, no preview test tables).

---

## Step-by-Step Conversion Workflow

### Step 1: Inspect the Source Skill

1. Identify the target skill under `.github/skills/<skill-name>/` (ignore `m365-agent-packager` itself). If only one convertible skill exists (e.g., `product-owner`), default to it unless the user specifies another.
2. Read the source `SKILL.md` and all supporting files (e.g., `reference/*.md`, `README.md`).

### Step 2: Transform Skill Files for M365 Copilot (`o365-copilot-agent/<skill-name>/`)

Create or update the files in `o365-copilot-agent/<skill-name>/` by applying these transformations to `SKILL.md` and `reference/*.md`:

1. **Normalize YAML Frontmatter in `SKILL.md`**:
   - Keep `name: <skill-name>`.
   - Format `description:` as a single-line string summarizing the skill's purpose, outputs, and triggers.
2. **Replace VS Code / IDE Tool Dependencies**:
   - Remove references to VS Code–specific tools such as `ask_user`, `view`, `edit`, `create`, `grep`, `glob`, or IDE slash commands.
   - Replace `ask_user` prompts/forms with natural chat facilitation: ask one focused question at a time in the target language, offering concrete numbered or bulleted options (and recommended defaults) inline in chat while allowing freeform answers.
3. **Replace Local Workspace File Paths with M365 Artifact Delivery**:
   - Replace assumptions of direct local disk writes (such as saving to `docs/1-problem-definition.md` or relative repository paths like `../../../docs/4-features.md`) with M365 Copilot delivery rules:
     - Keep the base filenames (e.g., `1-problem-definition.md` … `10-summary.md`) as recommended artifact filenames/titles.
     - Instruct the agent to generate downloadable Markdown files when file generation is available in the M365 Copilot environment, or output clean, copyable Markdown sections in chat otherwise.
     - Explicitly forbid claiming that files were written to a local `docs/` directory or saved to OneDrive/SharePoint unless an available and authorized M365 capability actually completed the save.
   - Keep internal skill links between `SKILL.md` and `reference/*.md` intact (e.g., `[stages.md](./reference/stages.md)` and `[artifact-formats.md](./artifact-formats.md)`).
4. **Update `README.md` (Optional Documentation)**:
   - If creating/updating `o365-copilot-agent/<skill-name>/README.md`, note that `README.md` is documentation for the repo and should be excluded when manually zipping `SKILL.md` and `reference/` for Agent Builder.
5. **Enforce the 20,000-Character Limit on `SKILL.md`**:
   - Verify the character count of `o365-copilot-agent/<skill-name>/SKILL.md` is `< 20,000`. If it exceeds 20,000 characters, move detailed templates or stage instructions into `reference/*.md` and link to them from `SKILL.md`.

### Step 3: Generate Raw English Agent Instructions (`o365-copilot-agent/AGENT-INSTRUCTIONS.md`)

Generate or update `o365-copilot-agent/AGENT-INSTRUCTIONS.md` in **English**, containing **only** the raw instruction text to paste into the M365 Copilot Agent Builder **Instructions** field (no wrapper commentary or outer markdown fences).

Follow Microsoft's official declarative agent instruction guidelines:

1. **Purpose & Role**:
   - State the agent's persona, core philosophy, and boundaries clearly in the opening paragraph.
2. **High-Level Skill Reference**:
   - Explicitly instruct the agent when to invoke and follow the attached `<skill-name>` custom skill (listing trigger scenarios and artifact types) and state that the skill defines the detailed workflow, templates, and rules.
3. **Language & Tone**:
   - Specify the language for user interaction and generated deliverables (e.g., Traditional Chinese for questions and artifacts, preserving standard acronyms, IDs, proper nouns, Mermaid syntax, and Given/When/Then keywords where appropriate).
   - Specify tone and collaborative behavior (e.g., challenging vague inputs, probing assumptions, guarding scope, asking one focused question at a time).
4. **Interactive Workflow & Stage Gate Rules**:
   - Define the turn-by-turn interaction contract so the agent does not dump all stages in a single turn:
     - Start at Stage 1 for new ideas, reflect the idea, state the stage and goal, ask ONE focused clarification question, and stop.
     - Track current stage, confirmed facts, open questions, and stage coverage before proposing a transition.
     - Require explicit user confirmation before advancing to the next stage.
     - Handle existing artifacts, targeted requests, or explicit batch-drafting requests appropriately.
5. **Domain & Scope Guardrails**:
   - Include the skill's core rules (e.g., business outcomes vs. technical implementation, hierarchy structure, story-splitting rules, diagram requirements, and UI concept boundaries if applicable).
6. **M365 Grounding, Evidence & Delivery**:
   - Instruct the agent to distinguish confirmed facts, proposals, assumptions, and open questions.
   - Deliver Markdown and fenced Mermaid code blocks; use downloadable file creation or M365 connectors only when available and authorized, and never fabricate saved links or local paths.
   - Include a fallback behavior if the `<skill-name>` custom skill fails to load.

### Step 4: Verify Character Counts & Report

Before finishing, verify and report to the user:
1. Character count of `o365-copilot-agent/<skill-name>/SKILL.md` (must be `< 20,000` chars).
2. Character count of `o365-copilot-agent/AGENT-INSTRUCTIONS.md` (must be `< 8,000` chars).
3. Summary of files created or updated.
