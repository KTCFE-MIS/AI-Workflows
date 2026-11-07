# Workflow Stages — Detailed Instructions

> **Facilitation Rule:** Whenever you need input, clarification, scope decisions, or stage-transition approval from the user, ask one focused question at a time in Traditional Chinese. When helpful, offer concrete numbered or bulleted options (or a recommended default) in chat so the user can respond quickly while still being free to answer in their own words.

## Stage 1: Ideation & Problem Definition

**Goal:** Understand what the user is thinking about, who has the problem, and why solving it matters.

This is the most conversational stage. Your job is to draw out the idea and ground it in real problems
and real users — not solutions or features.

### What to Explore

- **The problem:** What pain point, inefficiency, or unmet need exists today? How do people currently
  cope with it? What happens if we don't solve it?
- **The people:** Who experiences this problem? Be specific — not "users" but "recruiters who manage
  high-volume hiring pipelines" or "operations managers monitoring SLA compliance."
- **The context:** When and where does this problem occur? What triggers it? How frequently?
- **The value:** Why does solving this matter to the business? To the users? What does success look like?
  Can we articulate ROI or business justification?
- **The boundaries:** What is explicitly NOT in scope? What adjacent problems should we resist solving?

### How to Facilitate

- Start with: "Tell me about this idea in your own words — what are you thinking?"
- Reflect back what you hear: "So if I understand correctly, the core problem is..."
- Probe gently: "Who specifically runs into this?" / "What happens today when they hit this?"
- Challenge assumptions: "You mentioned users would want X — how do we know that? Have you seen
  them try to do this today?"
- Resist solutioning: If the user jumps to features, redirect — "That's a great idea for a solution.
  Before we go there, let's make sure we have the problem nailed down."

**When the user arrives with a pre-defined feature list:** This is common and requires a specific
response. Acknowledge what they've described, then explicitly slow down: "I can see you've already
been thinking about the solution — that's great context. But before we codify those features into
requirements, I want to make sure we understand the 'why' behind each one. Let me ask some questions
about the problem you're solving." Then systematically question each proposed feature: Why this one?
Who asked for it? What happens today without it? What problem does it solve specifically?
The goal is not to dismiss their ideas — it's to ground each one in a real user need so that
when we do write the features, they're anchored in business value rather than assumed solutions.

### Stage 1 Outputs

A clear, shared understanding delivered as `1-problem-definition.md` covering:
- The problem statement (1-2 sentences)
- The primary affected users/actors (named roles, not generic "users")
- The business justification (why this matters now)
- Initial scope boundaries (what's in, what's out)

Summarize these before moving to Stage 2.

---

## Stage 2: Project Charter / PRD

**Goal:** Formalize the vision, goals, scope, and constraints into a living document that provides
authority and alignment for the work ahead.

Per the PDLC methodology, charters should only be used for large initiatives. Small enhancements
and continued feature development don't need one. Use your judgment — if the idea is a single feature
enhancement, skip to Stage 3. If it's a significant initiative, write the charter.

### What to Capture

- **Vision & Goals:** What are we building and why? What does the end state look like?
- **Scope:** What capabilities and high-level features are included? What is explicitly excluded?
- **Triple Constraint:** Document the relationship between scope, schedule, and resources.
  In agile, resources are typically fixed — scope and schedule are the levers.
  This is the basis for MVP decisions: when and what.
- **Key Epics or Features:** Large-grain requirements captured at the epic level.
  Don't detail them yet — just name them.
- **Assumptions:** List them clearly. Each assumption is a risk that needs to be resolved.
  "There is no way to code a maybe" — push for answers on assumptions.
- **Success Criteria:** How will we measure whether this initiative succeeded?
- **Key Dates & Milestones:** If known, capture external deadlines or dependencies.
- **Stakeholders:** Who needs to be involved? Who has approval authority?

### How to Facilitate

- Walk through each section with the user, one at a time
- Push on scope boundaries — the most common failure mode is trying to do too much
- Challenge assumptions: "What if this assumption turns out to be wrong? How does it change the plan?"
- Help articulate MVP: "If you could only ship 3 things, which 3 would make this viable?"

### Stage 2 Outputs

A Project Charter / PRD markdown document delivered as `2-project-charter.md`. Offer to write it up when the conversation has
covered enough ground.

---

## Stage 3: Actor & Feature Discovery

**Goal:** Identify all actors who interact with the system and discover the high-level features
(business processes / use cases) they need to accomplish.

This is where you shift from "what's the problem" to "who does what with the system."

### Identifying Actors

Actors are not just people. They can be:
- **People in roles:** Recruiter, Hiring Manager, System Administrator, Candidate
- **Time:** For processes that occur on a schedule (batch jobs, recurring reports)
- **External systems:** Other systems that need your system to respond to events or complete steps

Guide the user to think about:
- Who are the primary users? (The ones whose goals drive the system)
- Who are the supporting actors? (Admins, managers who configure or oversee)
- Are there external systems that trigger processes or consume data?
- Does time trigger any processes? (Daily reports, expiring records, scheduled notifications)

### Discovering Features

Features capture business processes (Cockburn-style use cases) — what actors do with the system to accomplish goals.
At this stage, keep them high-level. You're building a map, not writing detailed instructions.

**Go beyond what the user lists.** Users typically describe the core "happy path" features they've
already thought of. Your job is to systematically uncover the ones they haven't mentioned yet.
The user's initial list is a starting point, not the complete picture.

For each actor, ask:
- "What are the main things [Actor] needs to accomplish with the system?"
- "What's [Actor]'s most important goal? What about secondary goals?"
- "Are there processes that span multiple actors?"

**Then actively probe for hidden features using these patterns:**

- **Configuration & preferences:** "Before [Actor] can do [main task], do they need to set anything up?
  Configure preferences? Customize their experience?"
- **Lifecycle management:** "What happens when [thing] is created? Modified? Archived? Deleted?
  Reactivated? Each of those is likely a separate feature."
- **Monitoring & history:** "Does [Actor] ever need to review what happened? See a history of actions?
  Track status over time? Generate reports?"
- **Administration & oversight:** "Who manages the overall configuration? Who handles edge cases
  that the system can't resolve automatically?"
- **Undo & recovery:** "What if [Actor] makes a mistake? How do they correct it? Can they undo actions?"
- **Bulk operations:** "Does [Actor] ever need to do this for many items at once, or always one at a time?"
- **Notifications & awareness:** "How does [Actor] find out when something relevant happens?
  Do they need to be notified? Can they control what they're notified about?"
- **Edge-of-scope adjacencies:** "You mentioned [Feature X]. What typically happens right before
  and right after that process? Are those in scope?"

After the user describes their initial features, explicitly say something like: "Those are great —
now let me push a bit to make sure we haven't missed anything" and work through the patterns above.
Often 30-50% of the final feature list comes from this discovery step rather than the user's initial input.

### Organizing the Hierarchy

Map discovered features into the requirements hierarchy:

| Level | What It Represents | Example |
|---|---|---|
| Initiative | Internal-oriented goal | "Modernize hiring workflow" |
| Epic | Collection of related features | "Manage Candidates" |
| Feature | A specific business process (use case) | "Screen Candidate Application" |
| Story | A deliverable increment of user value covering related scenarios | "Screen candidate application" |

At this stage, you're identifying Epics and Features. Stories come in Stage 5.

### How to Facilitate

- Use a discovery approach: "Let's walk through a day in the life of a [Actor]. What do they do first?"
- Watch for features that are too big — if it has multiple actors doing multiple things, it's probably
  an epic that needs to be broken into features
- Watch for <<uses>> relationships: "It sounds like 'Verify Identity' is something that happens inside
  both 'Onboard Customer' and 'Process Loan Application' — that's a shared feature."
- Watch for <<extends>> relationships: "So 'Apply Discount' only happens under certain conditions
  during 'Process Order'? That's an extension."
- Define the system boundary: What's inside the system we're building vs. external?

### Stage 3 Outputs

Delivered as `3-actors-and-features.md`:
- A list of all actors with brief descriptions
- A list of features organized by epic, showing which actors participate
- Notes on <<uses>> and <<extends>> relationships between features
- A clear system boundary statement

---

## Stage 4: Feature Elaboration

**Goal:** Write detailed features with pre/post conditions, main success scenarios, alternate flows,
and extension points.

This is the most detailed stage and where the real requirements emerge. Each feature discovered
in Stage 3 gets fleshed out into a structured document.

### Feature Structure (Cockburn-style)

For each feature, capture:

1. **Feature Name:** Verb-oriented, aligned with the actor's goal
2. **Primary Actor:** The actor who initiates the feature
3. **Goal:** What the actor is trying to accomplish (one sentence)
4. **Scope:** System or subsystem boundary
5. **Preconditions:** What must be true before this process can begin
6. **Postconditions (Success):** What is true after the process completes successfully
7. **Postconditions (Failure):** What is true if the process fails
8. **Main Success Scenario:** The "happy path" — numbered steps showing the actor-system interaction
9. **Extensions:** Alternate flows and error handling, keyed to the step number they branch from
10. **Special Requirements:** Non-functional requirements specific to this feature
11. **Data Requirements:** What information flows to and from the system at each step

### How to Facilitate

- Take one feature at a time. Don't rush.
- Start with the main success scenario: "Walk me through the ideal case. Everything goes right.
  What does the actor do first? What does the system respond with?"
- Then explore extensions: "What could go wrong at step 3? What if the data is incomplete?
  What if the actor cancels?"
- Push on preconditions: "What needs to be true before [Actor] can even start this?"
- Push on postconditions: "After this completes successfully, what should be verifiably true in the system?"
- Watch for scope creep: New features will emerge during elaboration. Capture them in the backlog
  but don't elaborate them now unless the user explicitly wants to.
- Data matters: At each step, ask "What information does the actor provide here?" and
  "What information does the system show them?"

### Decomposition Signals

If during elaboration you notice any of these, the feature is probably too large and should be
split into separate features or epics:

- Multiple actors doing multiple things
- A single actor completing multiple distinct goals
- Multiple actors doing the same thing in fundamentally different ways
- Actions spanning long time periods (days, weeks)
- Error handling that is itself a complex process
- The title is noun-oriented rather than verb-oriented

### Stage 4 Outputs

Fully elaborated feature documents for each feature, delivered as `4-features.md`. Each one following the
structured template above.

---

## Stage 5: Story Decomposition

**Goal:** Break each feature into user stories based on deliverable user value.

Stories are the bridge between features and work that can be estimated, prioritized, and scheduled.
Each story represents a meaningful, deliverable increment and may cover several related scenarios.

### How Stories Map to Features

| Feature Element | Default Treatment |
|---|---|
| Main Success Scenario | A starting point for a story, together with the conditions needed to make it usable |
| Extensions, alternate flows, and edge cases | Acceptance criteria and test cases within the original story |
| Complex error handling | Keep within the story unless the splitting criteria below justify a separate recovery story |
| Different actor paths through the same feature | Keep together when they serve the same deliverable goal; evaluate distinct goals for splitting |

Split a scenario into a separate story only when it provides independent user value or there is a
justified need for phased delivery. For a phased split, record the release rationale and dependencies;
the earlier increment must remain usable and satisfy its required business rules. Required validation
and error handling stay with that increment. Complexity or an extra branch alone does not justify a split.

For example, "Submit application" includes an acceptance criterion that missing required fields prevent
submission and show how to correct them. This does not need a separate story. "Save an application draft
and resume later" may be a separate story because it delivers a distinct user benefit and can be scheduled
separately from immediate submission.

### Story Format

```
As a [Actor from the feature],
I want to [Goal — the deliverable user outcome],
so I can [Business value / benefit].
```

### Story Quality Checklist

Each story should be:
- **Verb-oriented title:** "Terminate User" not "User Termination"
- **Coherent goal:** Related success, alternate, and error scenarios support the same user outcome
- **Scenario coverage:** Identify the covered feature paths and extensions; retain them in acceptance criteria and test cases
- **Business-value focused:** Delivers something releasable and meaningful
- **Actor has a non-developer identity:** If you can't name a real user role, this might be a technical task
- **Small enough to estimate:** Aim for stories that could be 8 story points or less
  (you don't estimate them — that's for the technical team — but use this as a sizing heuristic)

### How to Facilitate

- Work through one feature at a time
- Start with the main success scenario and include related extensions in the same story by default
- Evaluate potential splits using the criteria above; record the value or phased delivery rationale for each split
- For each story, verify: "Does this deliver a meaningful user outcome, and are its release dependencies explicit?"
- Watch for stories that are really epics in disguise (multiple actors, multiple goals)
- Remind the user: "Remember, stories are a promise to have a conversation at a later date.
  We're capturing enough context to understand and prioritize, not writing specs."

### Stage 5 Outputs

A structured list of user stories organized by parent feature, with clear titles
and the standard "As a / I want to / So I can" format, delivered as `5-story-decomposition.md`.
Identify each story's covered scenarios and any split rationale or release dependencies so Stage 8
can carry those scenarios into acceptance criteria and test cases.

---

## Stage 6: Diagrams & Models

**Goal:** Produce system-level visual models that complement the text artifacts and reveal overall functional structure and domain relationships that may not be obvious from text alone.

All diagrams should be produced in **Mermaid** syntax so they can be viewed or rendered in Markdown-compatible tools.

### Diagram Types

#### Feature Diagram
Shows actors, features, system boundaries, and relationships (<<uses>>, <<extends>>).
Produces at the system level — one diagram per major subsystem or capability area.

Purpose: High-level functional decomposition. Shows who interacts with what and
how features relate to each other. Similar to a functional decomposition view.

#### Domain Model Diagram
Models the concepts/entities in the problem domain and their relationships.
NOT a technical database or code design — this captures the business domain.

How to derive it:
- Extract nouns from feature text — these are candidate classes (entities)
- Extract verbs — these are relationships between classes
- Note cardinalities (one-to-many, many-to-many, etc.)
- Watch for state changes that span multiple features (e.g., Reservation vs. Ticket vs. Boarding Pass
  may be states of the same entity, not separate classes)
- Think in terms of roles and "is a" relationships for generalization

Purpose: Gives the technical team a head start on domain modeling. Captures the business
language and relationships that the system needs to represent.

### How to Facilitate

- Produce diagrams after the relevant text artifacts are complete
- Walk the user through each diagram and ask: "Does this capture it correctly?"
- Use diagrams to discover gaps: "I notice there's no relationship showing how [Entity A] connects to [Entity B]. Should there be?"

### Stage 6 Outputs

- Feature Diagram(s) and Domain Model Diagram in Mermaid, delivered as `6-diagrams.md`

---

## Stage 7: User Story Map

**Goal:** Produce a visual overview of the whole project in the layered story-map layout (Jeff Patton /
Planio style, aligned with Azure DevOps terminology). Read it top-down to see the top-level epics,
the narrative flow of the features under each one, the stories behind each feature, and how the work
is sliced into releases.

The map has four layers, top to bottom:

1. **Epic** — top-level feature groups / epics from Stages 2–4. Large blocks, one per epic, in workflow order
2. **Feature** — the narrative flow (Backbone / user tasks). Under each epic, the goal-level features run
   left to right in the order the user performs them. Titles are verb-oriented
3. **Details** — the user stories for each feature, stacked under their feature. Core user outcomes first, then
   separately justified increments. Card content: Story ID and title only
4. **Release slices** — horizontal bands that group the details by release (for example "R1 核心 MVP",
   "R2 邊界與補強", "Later"). Each story is placed in a **proposed** slice here; the user confirms
   the assignment in Stage 9

Rules:
- **Scope:** One map per project. Split only if it exceeds about 15 stories per release slice
- **Format:** Markdown table grid only (see [artifact-formats.md](./artifact-formats.md)). Do not draw the story map in
  Mermaid, because Mermaid cannot render it as a table. Every row must have the same number of cells
  as the header, so check the column count before delivering
- **Grid:** Columns are features grouped under their epic; the first column holds the
  release slice; each row is one detail position under its feature

Walk the user through the map before Stage 8: "Is the epic and feature sequence right, does each feature
capture a real user goal, is any story missing from a feature, and do the release slices match what you
want shipped first?"

### Stage 7 Outputs

- User Story Map delivered as `7-user-story-map.md` (layered Markdown table grid)

---

## Stage 8: User Stories (Diagrams, Acceptance Criteria & Test Cases)

**Goal:** Model each story's flow and actor-system interactions visually, define what "done" means for each story, and derive test cases that can verify it.

This stage connects requirements to quality. Pre/post conditions from features become the foundation
for per-story diagrams, acceptance criteria, and test cases.

**Output layout:** Group everything by user story, not by artifact type. Each story is one block that
keeps the story text, its description, its business value, its Story Flow Chart, its Summary Sequence Diagram, its acceptance criteria, and its test cases together,
so a reader never has to jump between sections to see the full picture of one story. Deliver the result as
`8-user-story.md` (user stories).

### Story Flow Charts

Every user story gets a Mermaid flow chart (`flowchart TD`) showing the scenarios it covers,
including decision points and alternate/error branches. This is required for every story.

**Line-length & wrapping rule (required to prevent CJK text clipping):**
Mermaid's `flowchart` renderer caps HTML label containers at `200px` (`wrappingWidth`) and does not reliably auto-wrap Chinese text without spaces across display DPI scales, causing single-line node labels over ~12–14 Chinese characters to be cut off on the right. Before saving:
- Keep every single line in a node (`["..."]`, `(["..."])`, `{"..."}`) to **at most 10–12 Chinese characters** (and ≤ 8–10 Chinese characters in decision diamonds `{"..."}` or edge labels `-- "..." -->`).
- Insert explicit `<br/>` line breaks between clauses (such as at `，` or `；`) whenever a label exceeds 12 Chinese characters (e.g., `Paused["倒數停止，顯示暫停狀態<br/>保留剩餘時間"]`).
- Always wrap node and edge labels in double quotes.

Purpose: Makes each story's logic, branches, and end states explicit so developers and testers can
see the paths that the acceptance criteria and test cases must cover.

### Summary Sequence Diagrams

Every user story gets a Mermaid sequence diagram (`sequenceDiagram`) showing the chronological interaction between an actor and the system for the story's scenario(s).
Lines toward the system are events (actor actions). Lines from the system are operations (system responses).
Include the data exchanged at each step.

Purpose: Clarifies the step-by-step interaction of the story's scenario(s), especially what data is exchanged and what state changes occur at each step.

### Acceptance Criteria

For each user story, define acceptance criteria that answer: "How will we know this story is complete?"

Format: always **Given/When/Then**: "Given [precondition], when [action], then [expected result]".
Do not ask the user to choose a format, and do not use checklists.

Acceptance criteria should be:
- Testable (someone can verify pass/fail)
- Specific (no ambiguity about what "works" means)
- Aligned with preconditions and postconditions from the feature

Per the PDLC methodology: acceptance criteria are reviewed and approved by the Product Owner.
They also include the ability to promote the functionality to production — preventing the buildup
of stories that are "almost done" but never quite complete.

### Test Cases

For each story, derive test cases from:
- Preconditions → what must be set up before testing
- Main scenario steps → the sequence to execute
- Postconditions → what to verify after execution
- Extensions → additional test cases for alternate/error paths

Each test case captures:
- Setup / preconditions
- Step-by-step actions
- Expected results at each step
- Input data requirements

### User Story Block Layout (`8-user-story.md`)

Each user story becomes one block, in the order of the story list, using the User Story Block Template in
[artifact-formats.md](./artifact-formats.md). Each block contains, in this order:

1. The story text ("As a [actor], I want to [goal], so that [benefit]")
2. A description immediately after the story text, following the template's plain-language guidance
3. Scenario coverage (parent feature and all main, alternate, or error paths covered)
4. The business value: a short plain-language statement of what this story delivers to the business or user
5. The story's Flow Chart (`flowchart TD`), inline as a Mermaid code block
6. The story's Summary Sequence Diagram (`sequenceDiagram`), inline as a Mermaid code block
7. Acceptance criteria (Given/When/Then, each with an AC ID such as `AC-01-1`)
8. Test cases that verify those criteria (TC IDs, steps table, postconditions, test data)

Keep the document organized with a table of contents, a global definition of done at the top, and a section
for confirmed decisions and open questions at the bottom. Number test cases sequentially across the whole
document so TC IDs stay unique.

### How to Facilitate

- Work story by story
- Walk through the story's flow chart and sequence diagram to confirm all branches and actor-system interactions: "I notice there's no interaction showing how [Actor] gets notified when this completes. Should there be?"
- Ask: "If a QA engineer tested this story, what would they need to see to say 'it works'?"
- Push for specificity: "When you say 'the system shows an error,' what error? What does the user see?"
- Connect back to feature pre/post conditions

### Stage 8 Outputs

- `8-user-story.md`: one block per user story, containing the story text, description, business value, Story Flow Chart, Summary Sequence Diagram,
  acceptance criteria, and test cases (see User Story Block Layout above)
- Every story has at least one test case; every acceptance criterion is covered by at least one test case

---

## Stage 9: Summary

**Goal:** Produce a high-level requirements summary and master index (`9-summary.md`) that gives readers a quick,
bird's-eye view of the entire project and references the detailed Stage 1–8 documents.

### Summary Structure

Compile a concise overview and navigation document with this structure:

```
# [Initiative/Project Name] — 需求總覽（Summary）

## 1. Executive Summary
   - Problem statement
   - Vision and goals
   - Scope (in/out)
   - Success criteria
   - Release slices overview

## 2. Project Charter / PRD
   - Concise summary of constraints, epics, stakeholders, and milestones (referencing `2-project-charter.md`)

## 3. Actors
   - Actor descriptions, goals, and system boundary

## 4. Requirements Hierarchy
   - Epics → Features → Stories mapping table with release slices and dependencies

## 5. Features (Summary)
   - Concise summary table of each feature and key cross-cutting rules (referencing `4-features.md` for full details)

## 6. User Stories
   - Summary table of stories referencing `8-user-story.md` (which holds full story blocks, flow charts, sequence diagrams, AC, and test cases)

## 7. Diagrams
   - High-level overview diagrams inline (Feature Diagram and Domain Model Diagram, from `6-diagrams.md`)
   - References to `8-user-story.md` for Story Flow Charts and Summary Sequence Diagrams

## 8. Test Cases
   - Total test case count and coverage summary, referencing `8-user-story.md`

## 9. Open Questions & Assumptions
   - Unresolved items, resolved key decisions, confirmed assumptions, and known limitations

## 10. Glossary
   - Domain terms defined during the process
```

### How to Facilitate

- Review the summary with the user before finalizing
- Flag any gaps, resolved decisions, or unresolved questions across Stages 1–8
- Ensure all references to `1-problem-definition.md` through `8-user-story.md` are consistent so `9-summary.md` serves as a reliable entry point

### Stage 9 Outputs

A concise requirements summary and master index delivered as `9-summary.md`.
