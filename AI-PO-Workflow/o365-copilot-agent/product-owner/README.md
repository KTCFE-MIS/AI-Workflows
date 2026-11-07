# Product Owner Skill (`product-owner`) — M365 Copilot Edition

A seasoned Product Owner (PO) partner for requirements gathering and product discovery, adapted for **Microsoft 365 Copilot Agent Builder**. Guides users through the full requirements lifecycle — from a rough idea to a complete, handoff-ready requirements package in Traditional Chinese.

**Methodology grounded in:**
- *Writing Effective Use Cases* — Alistair Cockburn
- *Patterns for Effective Use Cases* — Steve Adolph & Paul Bramble
- *Applying UML and Patterns* — Craig Larman
- *User Story Mapping* — Jeff Patton
- Proven PDLC methodology (`Initiatives > Epics > Features > Stories`)

**Workflow stages & outputs:**
1. **Ideation & Problem Definition** (`1-problem-definition.md`)
2. **Project Charter / PRD** (`2-project-charter.md`)
3. **Actor & Feature Discovery** (`3-actors-and-features.md`)
4. **Feature Elaboration** (`4-features.md`)
5. **Story Decomposition** (`5-story-decomposition.md`)
6. **Diagrams & Models** (`6-diagrams.md`)
7. **User Story Map** (`7-user-story-map.md`)
8. **User Stories (Acceptance Criteria & Test Cases)** (`8-user-story.md`)
9. **Summary** (`9-summary.md`)

---

## Directory & Package Structure

```text
o365-copilot-agent/product-owner/
├── README.md                 ← Documentation (exclude when zipping)
├── SKILL.md                  ← Core skill instructions (< 20,000 chars)
└── reference/
    ├── stages.md             ← Detailed stage-by-stage facilitation guide
    └── artifact-formats.md   ← Templates & conventions for all artifact types
```

When zipping for Agent Builder, place `SKILL.md` and `reference/` at the root of the archive:
```text
product-owner.zip
├── SKILL.md
└── reference/
    ├── artifact-formats.md
    └── stages.md
```

---

## Uploading to Microsoft 365 Copilot Agent Builder

1. Zip `SKILL.md` and the `reference/` folder so `SKILL.md` is at the root of the `.zip`.
2. Open your declarative agent in **M365 Copilot Agent Builder**.
3. Go to **Configure → Skills → Add** and upload the `.zip` file.
