# PO Workflow (`/product-owner`)

## Purpose

`/product-owner` acts as a seasoned Product Owner partner for product discovery and requirements gathering. It guides you from a rough idea to a complete, handoff-ready requirements package in `docs/` without drifting into technical implementation details (no DB schemas, APIs, or architecture decisions).

- **Use-case & feature driven**: Grounds requirements in actors, goals, pre/post-conditions, and scenarios (Cockburn, Adolph & Bramble, Larman) rather than isolated one-line user stories.
- **Interactive & probing**: Challenges vague assumptions, guards against scope creep, and asks one focused question at a time (in Traditional Chinese).
- **Ready for Dev handoff**: Produces structured Markdown documents and Mermaid diagrams that feed directly into the [Dev Workflow](./Dev-workflow.md) (`/grill-me`, `/to-spec`, `/tdd`).

---

## 1. Start or Resume the Workflow

1. Kick off from a rough idea (starts at Stage 1)
   ```text
   /product-owner I want to build <rough product idea>. Help me define the requirements.
   ```
2. Resume or jump to a specific stage using existing `docs/`
   ```text
   /product-owner Review the existing docs in docs/ and continue from Stage <N>.
   ```

---

## 2. Guided Discovery & Elaboration Stages

1. **Stage 1: Ideation & Problem Definition** (`docs/1-problem-definition.md`) — clarify the core problem, target personas, pain points, and value proposition.
2. **Stage 2: Project Charter / PRD** (`docs/2-project-charter.md`) — formalize product vision, business goals, in/out-of-scope boundaries, constraints, and success metrics.
3. **Stage 3: Actor & Feature Discovery** (`docs/3-actors-and-features.md`) — identify all primary/supporting actors, system boundaries, and verb-oriented high-level features.
4. **Stage 4: Feature Elaboration** (`docs/4-features.md`) — detail each feature with Cockburn-style templates: preconditions, postconditions, main success scenario, and alternate/error extensions.
5. **Stage 5: Story Decomposition** (`docs/5-story-decomposition.md`) — slice features into user stories by deliverable business value (never technical tasks).

---

## 3. Visual Modeling, Mapping & Handoff Package

1. **Stage 6: Diagrams & Models** (`docs/6-diagrams.md`) — generate Mermaid visual models to expose gaps that text misses:
   - Feature Diagram (`flowchart LR`)
   - Domain Model Diagram (`classDiagram` for domain concepts)
2. **Stage 7: User Story Map** (`docs/7-user-story-map.md`) — organize Epics → Features → Stories into prioritized release slices (MVP vs. future releases) in a layered Markdown table grid.
3. **Stage 8: User Stories & Acceptance Criteria** (`docs/8-user-story.md`) — assemble self-contained story blocks with business value, inline story flow charts (`flowchart TD`), inline summary sequence diagrams (`sequenceDiagram`), Given/When/Then acceptance criteria, and step-by-step test cases.
4. **Stage 9: Glossary** (`docs/9-glossary.md`) — map every Traditional Chinese domain term to one canonical English term so developers name code consistently.
5. **Stage 10: Summary & Master Index** (`docs/10-summary.md`) — compile an executive overview, traceability matrix, open assumptions, and index across all `docs/1-*.md` to `docs/9-*.md` artifacts for engineering handoff.
