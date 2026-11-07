# Product Owner Skill (`product-owner`)

A seasoned Product Owner (PO) partner for requirements gathering and product discovery. Guides you through the full requirements lifecycle — from a rough idea to a complete, handoff-ready requirements package in Traditional Chinese.

**Methodology grounded in:**
- *Writing Effective Use Cases* — Alistair Cockburn
- *Patterns for Effective Use Cases* — Steve Adolph & Paul Bramble
- *Applying UML and Patterns* — Craig Larman
- *User Story Mapping* — Jeff Patton
- Proven PDLC methodology (`Initiatives > Epics > Features > Stories`)

**Workflow stages & outputs:**
1. **Ideation & Problem Definition** (`docs/1-problem-definition.md`)
2. **Project Charter / PRD** (`docs/2-project-charter.md`)
3. **Actor & Feature Discovery** (`docs/3-actors-and-features.md`)
4. **Feature Elaboration** (`docs/4-features.md`)
5. **Story Decomposition** (`docs/5-story-decomposition.md`)
6. **Diagrams & Models** (`docs/6-diagrams.md`)
7. **User Story Map** (`docs/7-user-story-map.md`)
8. **User Stories (Acceptance Criteria & Test Cases)** (`docs/8-user-story.md`)
9. **Glossary** (`docs/9-glossary.md`)
10. **Summary** (`docs/10-summary.md`)

**Triggers on:** `/product-owner`, "I have an idea for a feature", "help me think through requirements", "write features/use cases", "product requirements", "PRD", "user stories", "story map", "需求訪談", "撰寫 PRD", "拆解 User Story", and similar.

---

## Directory Structure

```
.github/skills/product-owner/
├── README.md                 ← skill overview & documentation
├── SKILL.md                  ← core skill instructions, persona & stage overview
└── reference/
    ├── stages.md             ← detailed stage-by-stage facilitation guide
    └── artifact-formats.md   ← templates & conventions for all artifact types
```

---

## Iterating on the Skill

1. Edit `SKILL.md` for persona, core rules, or workflow overview changes.
2. Edit `reference/stages.md` for stage-specific facilitation prompts or output rules.
3. Edit `reference/artifact-formats.md` for Markdown/Mermaid artifact templates.
4. Test the skill in VS Code Copilot Chat by invoking `/product-owner` or describing a new product idea.
