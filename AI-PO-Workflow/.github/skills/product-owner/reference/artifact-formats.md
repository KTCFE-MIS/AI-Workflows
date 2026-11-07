# Artifact Format Templates

## Project Charter / PRD Template

```markdown
# Project Charter: [Project Name]

## Vision
[One paragraph describing the desired end state]

## Problem Statement
[What problem exists today, who experiences it, and why it matters]

## Goals & Objectives
- [Goal 1]: [Measurable success criterion]
- [Goal 2]: [Measurable success criterion]

## Scope

### In Scope
- [Capability / feature area 1]
- [Capability / feature area 2]

### Out of Scope
- [Explicitly excluded item 1]
- [Explicitly excluded item 2]

## Constraints
- **Resources:** [Fixed team size, availability]
- **Schedule:** [Key dates, external deadlines]
- **Dependencies:** [External systems, vendor timelines, other teams]

## Key Epics
| Epic | Description |
|---|---|
| [Verb-phrase name] | [Brief description of the capability area] |

## Assumptions
| # | Assumption | Impact if Wrong | Status |
|---|---|---|---|
| A1 | [Assumption text] | [What changes] | Open / Resolved |

## Stakeholders
| Role | Name/Team | Involvement |
|---|---|---|
| [Role] | [Name] | [Approver / Contributor / Informed] |

## Success Criteria
- [Measurable outcome 1]
- [Measurable outcome 2]

## MVP Definition
[What is the minimum set of capabilities that makes this viable?]
```

---

## Feature Template (Cockburn-style)

```markdown
# Feature: [Verb-oriented Name]

**ID:** FT-[number]
**Primary Actor:** [Actor name]
**Goal:** [One sentence — what the actor is trying to accomplish]
**Scope:** [System or subsystem name]
**Level:** [User Goal / Summary / Subfunction]

## Preconditions
- [What must be true before this process can begin]
- [Another precondition]

## Postconditions (Success)
- [What is true in the system after successful completion]

## Postconditions (Failure)
- [What is true in the system if the process fails or is abandoned]

## Main Success Scenario
1. [Actor] [does something]
2. System [responds with something], displaying [specific data]
3. [Actor] [provides/selects/confirms something]
4. System [processes and responds]
5. ...

## Extensions
**3a. [Condition that triggers alternate flow]:**
  1. System [alternate response]
  2. [Actor] [alternate action]
  3. Return to step [N] / Feature ends

**4a. [Error condition]:**
  1. System [error response with specific message/behavior]
  2. [Actor] [recovery action]
  3. Return to step [N] / Feature ends

## Data Requirements
| Step | Data To System | Data From System |
|---|---|---|
| 1 | [What the actor provides] | — |
| 2 | — | [What the system displays] |
| 3 | [What the actor provides] | — |

## Special Requirements
- [Non-functional requirement specific to this feature, e.g., response time, accessibility]

## Related Features
- <<uses>> [FT-X: Shared Feature Name]
- <<extends>> [FT-Y: Optional Extension Name]

## Open Questions
- [Unresolved question discovered during elaboration]
```

### Feature Goal Levels (Cockburn)

- **Summary Level:** High-level business process spanning multiple user goals
  (e.g., "Manage Employee Lifecycle"). Usually maps to an Epic.
- **User Goal Level:** A complete, meaningful interaction an actor has with the system
  in one sitting (e.g., "Terminate Employee"). This is the sweet spot for features.
- **Subfunction Level:** A step within a larger goal that's reused across features
  (e.g., "Verify Identity"). These become <<uses>> relationships.

---

## User Story Template (Stage 5, `docs/5-story-decomposition.md`)

```markdown
### [Story ID]: [Verb-oriented Title]

**Parent Feature:** FT-[number]: [Feature Name]
**Scenarios Covered:** [List all covered paths, e.g., Main Success, Extension 3a, Extension 4a]

> As a [specific actor role],
> I want to [specific goal — the deliverable user outcome],
> so I can [business value or benefit].

**Notes:**
- [Any context that helps understand the covered scenarios]
- [For a split: independent user value or phased delivery rationale, and any release dependencies]
```
*(Note: Each story's Mermaid Flow Chart and Summary Sequence Diagram are produced in Stage 8 inside the User Story Block (`docs/8-user-story.md`).)*

### Story Naming Conventions

Stories derived from features follow a pattern. Alternate flows, recovery, and actor variations become
separate stories only when justified by the splitting criteria in [Stage 5](./stages.md#stage-5-story-decomposition);
otherwise, describe them in the original story's acceptance criteria.

| Source | Story Title Pattern | Example |
|---|---|---|
| Main Success Scenario | Same as feature name | "Terminate User" |
| Alternate flow | Describes the variation | "Terminate Disabled User" |
| Error recovery | Describes the recovery | "Recover from Error Terminating User" |
| Different actor path | Includes the actor distinction | "Terminate User as HR Admin" |

---

## Feature Diagram (Mermaid)

Use Mermaid flowchart syntax to represent feature diagrams (Cockburn/UML use case diagrams) since Mermaid doesn't have
a native use case diagram type.

```mermaid
flowchart LR
    subgraph System["System Name"]
        FT1["Manage Users"]
        FT2["Terminate User"]
        FT3["Create User"]
        FT4["Verify Identity"]
    end

    Actor1([Recruiter]) --> FT1
    Actor1 --> FT2
    Actor2([HR Admin]) --> FT2
    Actor2 --> FT3

    FT2 -.->|"<<uses>>"| FT4
    FT3 -.->|"<<uses>>"| FT4

    Actor3([External Auth System]) --> FT4
```

Conventions:
- Actors use `(["Actor Name"])` (stadium shape)
- Features use `["Feature Name"]` (rectangle) inside a system boundary subgraph
- Solid arrows `-->` for actor-to-feature relationships
- Dashed arrows `-.->` with labels for <<uses>> and <<extends>>
- Keep each line inside a node label to **≤ 10–12 Chinese characters** (insert `<br/>` if longer) and wrap labels in double quotes so CJK text is never clipped by Mermaid's 200px label width cap

---

## Summary Sequence Diagram (Mermaid)

```mermaid
sequenceDiagram
    actor R as Recruiter
    participant S as System

    R->>S: Select candidate to screen
    S-->>R: Display candidate profile (name, resume, application date)
    R->>S: Review qualifications and enter screening notes
    S-->>R: Confirm notes saved
    R->>S: Set screening decision (advance / reject)
    S-->>R: Display confirmation with next steps
    Note over S: Candidate status updated to "Screened"
```

Conventions:
- Lines going toward the system (`->>`) are **events** (actor actions)
- Lines coming from the system (`-->>`) are **operations** (system responses)
- Include the data exchanged in parentheses after the action description
- Use `Note over` for important state changes or postconditions
- One diagram per significant scenario covered by a story

---

## Story Flow Chart (Mermaid)

Every user story gets its own flow chart, in addition to the Feature Diagram. It shows the
decision points and branches of the related scenarios the story covers.

```mermaid
flowchart TD
    Start(["Recruiter opens<br/>pending requisition"]) --> Check{"Budget<br/>verified?"}
    Check -- "Yes" --> Approve["Approve requisition<br/>with comments"]
    Check -- "No" --> Reject["Return requisition<br/>with reason"]
    Approve --> Done(["Status set to Approved"])
    Reject --> Done2(["Status set to Returned"])
```

Conventions:
- Use `flowchart TD` (top-down) so each story's flow reads from top to bottom
- Start and end points use stadium shapes `(["..."])`
- Actions use rectangles `["..."]`; decisions use diamonds `{"..."}`
- Label decision branches with `-- "是" -->` / `-- "否" -->` (or the relevant condition, ≤ 8–10 Chinese characters)
- **Prevent CJK text clipping (required):** Mermaid's `flowchart` renderer caps HTML label containers at `wrappingWidth: 200px` and does not auto-wrap Chinese text without spaces across display DPI scales, clipping single lines longer than ~12–14 Chinese characters on the right. Keep every line in a node (`["..."]`, `(["..."])`, `{"..."}`) to **≤ 10–12 Chinese characters** (≤ 8–10 in diamonds `{"..."}`). Whenever a label has multiple clauses (e.g., separated by `，` or `；`) or exceeds 12 Chinese characters, split it across lines using `<br/>` (for example, `Paused["倒數停止，顯示暫停狀態<br/>保留剩餘時間"]`). Always wrap labels in double quotes
- Cover the main success path and the extension/alternate branches the story addresses
- Keep the story title and flow aligned with its user outcome; multiple branches alone do not require a split

---

## Domain Model Diagram (Mermaid)

```mermaid
classDiagram
    class Candidate {
        +name
        +email
        +phone
        +applicationDate
        +status
    }

    class JobRequisition {
        +title
        +department
        +openDate
        +closeDate
        +status
    }

    class Application {
        +submissionDate
        +status
        +screeningNotes
    }

    class Recruiter {
        +name
        +email
        +assignedRequisitions
    }

    Candidate "1" --> "*" Application : submits
    JobRequisition "1" --> "*" Application : receives
    Recruiter "1" --> "*" JobRequisition : manages
    Recruiter "1" --> "*" Application : screens
```

Conventions:
- Capture domain concepts from feature text (nouns = classes, verbs = relationships)
- Include key attributes (not exhaustive — this is analysis, not design)
- Show cardinalities
- Use relationship labels that describe the business relationship
- This is NOT a database schema or technical class diagram

---

## User Story Map (Stage 7, `docs/7-user-story-map.md`)

Produce the map as a Markdown table grid, saved to `docs/7-user-story-map.md`. It follows the layered
layout: Epic → Feature → Details → Release slices. Do not use Mermaid for the
story map; Mermaid cannot render this grid as a table.

### Markdown Table Grid

The first column holds the layer/release slice label. Epics are the group headers, Features form the
first body row (backbone), and each following row is one detail position. A blank cell means nothing is placed there.

```markdown
| Epic | Epic 1：建立申請 | | Epic 2：審查候選人 | | Epic 3：安排面試 | |
|---|---|---|---|---|---|---|
| **Feature** | **提交申請** | **上傳履歷** | **篩選申請** | **排序候選人** | **預約面試** | **取消面試** |
| **Release 1（MVP）** | US-01 提交申請 | US-02 上傳履歷 | US-04 篩選申請 | US-05 排序候選人 | US-07 預約面試 | |
| | US-03 撤回申請 | | | | | US-08 取消面試 |
| **Release 2** | | US-06 補充履歷 | | US-09 標記候選人 | | |
| **Later** | | | | | | US-10 改期面試 |
```

Note: the table has one header row, so the Epic names sit in the header and the Features
sit in the first body row. Each Epic's Features are the consecutive columns under it. Release
slices are proposed here and confirmed in Stage 10.

**Column count (required):** every row, including the header and separator, must have exactly the
same number of cells. The number of columns equals 1 (label) + the total number of Features.
An Epic that spans n Features uses its name in the first of those n columns and leaves the other
n−1 cells blank (Markdown has no colspan). Before saving, count the cells in each row and fix any
row that is short or long. A row with fewer cells renders as a broken table.

Conventions:
- **Epic** is the header group above its Features, in workflow order
- **Feature** is the second row (backbone), verb-oriented, in the order the user performs them
- **Details** are stories stacked under their Feature, core user outcomes first, then separately justified increments
- **Release slices** are the first column; the release label appears only on the first row of its block

- Card content is Story ID and title only


---

## User Story File Skeleton (Stage 8, `docs/8-user-story.md`)

The file is long, so it opens with a linked table of contents. Lay the file out in this order:

1. H1 title and intro (sources, numbering conventions)
2. Global Definition of Done
3. Table of contents, marked with `<a id="toc"></a>`
4. User story blocks, in story-list order (optionally grouped under `## Epic …` headings)
5. Confirmed decisions and open questions

```markdown
# 8. 使用者故事（User Stories）

[Intro: sources, numbering conventions]

---

## 全域完成定義（Definition of Done）

[Global DoD]

---

<a id="toc"></a>
## 目錄

- **[Epic name, e.g. Epic A：撰寫每日與每週日誌]**
  - [[Story ID] [Story title]](#[story-id-lowercase]) — [Parent Feature IDs, e.g. FT-01、FT-02]
  - [[Story ID] [Story title]](#[story-id-lowercase]) — [FT-03]
- **[Next Epic name]**
  - [[Story ID] [Story title]](#[story-id-lowercase]) — [FT-04]

---

[User Story Blocks, see template below]

---

## 已確認決策與待確認事項

[Confirmed decisions and open questions]
```

TOC rules:
- Group stories under bold, unlinked Epic labels, taking Epics from Stage 3 / Stage 7. Within an Epic, list stories in story-list order.
- One line per story, and link it to the story's anchor. After an em dash, list only the story's **parent** Feature IDs, not features it merely cross-references.
- List stories only. Do not add entries for acceptance criteria, test cases, the Definition of Done, or the decisions section.
- Anchor IDs are the story ID lowercased exactly as written: `S01` → `s01`, `S-01` → `s-01`. Never derive anchors from heading text. Auto-generated anchors for CJK headings differ across renderers and break when titles change.
- Keep the TOC in sync whenever stories are added, removed, or renamed.

---

## User Story Block Template (Stage 8, `docs/8-user-story.md`)

One block per user story. Keep the order: anchor, story, description, scenario coverage, business value, inline Mermaid flow chart, inline Mermaid summary sequence diagram, acceptance criteria, test cases, back-to-TOC link.

```markdown
<a id="[story-id-lowercase]"></a>
### [Story ID] [Verb-oriented story title]

- **故事 / Story**: As a [actor], I want to [goal], so that [benefit].
- **描述 / Description**: [Explain the usage context, what the user does, and the expected system response or outcome in plain language, using confirmed requirements.]
- **涵蓋情境 / Scenarios Covered**: [Parent Feature FT-XX; list all covered main, alternate, and error paths]
- **商業價值 / Business value**: [One or two sentences on the value this story delivers]
- **流程圖 / Flow chart**:
```mermaid
flowchart TD
    Start([Start]) --> Step1[Action]
    Step1 --> Done([End])
```
- **摘要序列圖 / Summary sequence diagram**:
```mermaid
sequenceDiagram
    actor A as Actor
    participant S as System

    A->>S: Perform action (input data)
    S-->>A: Return response (output data)
```

**驗收標準 / Acceptance criteria**

- **AC-[NN]-1**: Given [precondition], When [action], Then [expected outcome].
- **AC-[NN]-2**: Given [precondition], When [action], Then [expected outcome].

**測試案例 / Test cases**

#### TC-[NN] [Descriptive name]

- **對應 / Covers**: [Story ID] / AC-[NN]-1
- **前置條件 / Preconditions**: [setup requirement]
- **測試資料 / Test data**: [specific input data]

| # | 操作 / Action | 預期結果 / Expected result |
|---|---|---|
| 1 | [What the tester does] | [What should happen] |
| 2 | [Next action] | [Expected response] |

- **後置條件 / Postconditions**: [what should be verifiably true after the test]

[↑ 回到目錄](#toc)
```

---

## Acceptance Criteria Format

Embed acceptance criteria directly inside each story's block in `docs/8-user-story.md`. Always use Given/When/Then:

```markdown
**驗收標準 / Acceptance criteria**

- **AC-[NN]-1**: **Given** [precondition/context], **When** [actor performs action], **Then** [expected system behavior/outcome].
- **AC-[NN]-2**: **Given** [another precondition], **When** [another action], **Then** [another outcome].
```

---

## Glossary Format

Format for `docs/9-glossary.md`. Steps and term rules live in Stage 9 of [stages.md](./stages.md).

Order rows by Domain Model class: each class, then its key attributes and states, then the business actions
and remaining terms.

```markdown
# [Initiative/Project Name] — 詞彙表（Glossary）

| 詞彙（繁體中文） | 英文名稱（English Term） | 定義 |
|---|---|---|
| [繁體中文領域詞彙] | [Canonical English Term] | [業務定義與規則說明] |
```

---

## Summary Format

Format for `docs/10-summary.md`. Steps and section structure live in Stage 10 of [stages.md](./stages.md).

- **Table of contents** at the top, linking to each section.
- **Relative links** to sibling stage files, e.g. `[4-features.md](./4-features.md)`.
- **Story anchors:** each row in the story table links to its Stage 8 block with the lowercase story-ID
  anchor, e.g. `[S-01](./8-user-story.md#s-01)`.
- **Diagrams:** copy the Feature Diagram and Domain Model Diagram inline from `docs/6-diagrams.md`; link to
  `docs/8-user-story.md` for per-story flow charts and sequence diagrams.
- **Glossary:** link to `./9-glossary.md` from the Document Index. The term table lives only in the glossary
  file.
