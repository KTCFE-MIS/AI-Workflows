# AI Dev Workflow

A step-by-step workflow for developers using **GitHub Copilot** to turn a user story into tested code and a pull request.

- **Input:** a user story from the PO workflow, identified by its story ID (for example `S02` in `docs/8-user-story.md`). The story file and ID differ for every feature.
- **Output:** an implementation spec, a clickable prototype, tested code, and a PR description.

```text
Spec → Prototype → Refine → Build → Debug → Review → PR
```

---

## Before you start

**1. Install the external skills.** Install every skill from [mattpocock/skills](https://github.com/mattpocock/skills) and choose GitHub Copilot as the agent when prompted:

```text
npx skills add mattpocock/skills
```

**2. Add this repo to your workspace.** Clone it into the folder where you keep your repos:

```text
git clone https://sztfs.sz.kingston.corp/tfs/DefaultCollection/MIS%20DEV%20Platform/_git/AI-DEV-Workflow
```

Then, in VS Code, choose **File > Add Folder to Workspace...** and select the cloned `AI-DEV-Workflow` folder. Copilot picks up the skills in its [.github/skills/](.github/skills/):

| Skill | What it does |
| --- | --- |
| `story-to-implementation-spec` | Grills implementation decisions for a story and writes a spec |
| `reusable-prototype` | Builds a portable HTML prototype for one story |
| `match-prototype` | Ports a prototype page into the real app page |
| `guided-tdd` | Gets your approval on a build plan, then builds it test-first |

**3. Set up the MCP servers.**

- **Playwright MCP** (`@playwright/mcp`): required by `/match-prototype` to compare screenshots.
- **Figma MCP**: optional, only for Step 4.

**4. Keep coding conventions in `AGENTS.md`** at the project root. Copilot reads it on every request. See Step 9.

---

## Steps

### 1. Create the implementation spec

**When:** you pick up a new story.

**Command:** `/story-to-implementation-spec <story file> <story ID>`

**Input → Output:** user story → `implementation-docs/spec-<story-id>.md`, plus any approved changes to the source story documents.

The AI reads the story, its acceptance criteria, and the code, then asks you implementation questions in rounds. It proposes requirement changes as a table for you to approve, and writes a spec that links back to the story instead of copying it.

```text
/story-to-implementation-spec docs/8-user-story.md S01
```

### 2. Build the prototype

**When:** the implementation spec is done.

**Command:** `/reusable-prototype <implementation-doc> <css-framework> [--like <page-path>]` (one story per run; `--like` is optional and names an existing page to model on)

**Input → Output:** one story → `prototype/<page>.html` with its `css/` and `js/` files.

The prototype uses the project's own CSS and JS frameworks from a CDN. Each page opens directly in a browser with no build step, server, or install, so you can share it as a folder or ZIP. Pages are per app page, not per story: a second story on the same page extends the existing file.

```text
/reusable-prototype spec-s01.md Vuetify
/reusable-prototype spec-s02.md Vuetify --like prototype/history.html
```

### 3. Refine the prototype in the integrated browser

**When:** the prototype needs visual or interaction changes.

**Input → Output:** prototype page → updated prototype `css/` and `js/` files.

1. Open `prototype/<page>.html` in VS Code's integrated browser.
2. Select the element you want to change.
3. Add it to chat.
4. Describe the change. The AI edits the prototype's `css/` and `js/` files.

### 4. (Optional) Convert the prototype to Figma

**When:** a designer or developer prefers to work on the design in Figma.

**Input → Output:** prototype page → Figma design.

Ask Copilot, with the Figma MCP connected:

```text
Convert prototype/timer.html into a Figma design using the Figma MCP
```

### 5. Build the feature

**When:** the prototype is final.

**Command:** `/match-prototype prototype/<page>.html`

**Input → Output:** prototype page → updated app page, with tests for any behavior changes.

The AI pairs the prototype page with the app page of the same name and sorts each difference into behavior or styling. It asks blocking questions once, builds behavior changes test-first, and ports styling directly. It then compares screenshots of the prototype and the app with Playwright and runs the project's build, lint, and test scripts.

```text
/match-prototype prototype/timer.html
```

**No existing codebase?** Use `/guided-tdd` instead of Step 5 and pass it the prototype page along with the spec. Its plan scaffolds the app on the frameworks you chose for the prototype, then builds the page to match it.

```text
/guided-tdd implementation-docs/spec-s01.md prototype.html
```

**No UI?** Use `/guided-tdd` instead of Steps 2–5. It proposes the files, interfaces, test seams, and an ordered test list, waits for your approval, records the plan in the spec, then builds one behavior at a time. It also accepts a free-text request instead of a spec.

```text
/guided-tdd implementation-docs/spec-s02.md
```

### 6. Debug in the integrated browser

**When:** something in the running app looks or behaves wrong.

**Input → Output:** a bug → a fix.

1. Reproduce the bug in VS Code's integrated browser.
2. Add the element or the console error to chat and describe what you expected.
3. For hard bugs or performance problems, run `/diagnosing-bugs`.

```text
/diagnosing-bugs The timer skips a second after resuming from pause
```

### 7. Review the code

**When:** the feature works, before opening a PR.

**Command:** `/code-review <base branch or commit>`

**Input → Output:** your changes → review findings against the project's coding standards and against the spec.

```text
/code-review main
```

### 8. Improve the architecture

**When:** only when your change alters the architecture, for example it adds or moves modules or changes the boundaries between them.

**Command:** `/improve-codebase-architecture <path>`

**Input → Output:** a folder of code → an HTML report of improvement opportunities. Pick one and the AI works through it with you.

```text
/improve-codebase-architecture app/src
```

### 9. Update instructions

**When:** `/code-review` flags the same issue more than once, or the team agrees on a new rule.

**Input → Output:** a repeated issue or new rule → an updated `AGENTS.md`.

Add the rule to `AGENTS.md` at the project root so Copilot follows it from the next request on.

### 10. Write the PR description

**When:** the code is reviewed and ready for human review.

**Command:** `/pr`

**Input → Output:** your branch → a PR description for human reviewers.

```text
/pr and generate a ready to copy markdown version
```

### 11. Human Review

**You must read and understand every line of AI-generated or AI-modified code you submit.** Be able to explain what it does, why it is written that way, and how it affects existing functionality. Clarify anything you do not understand before submitting.

1. **Code must be maintainable and follow the project's existing style and architecture.** This includes naming, module responsibilities, error handling, and reuse of existing components. Avoid duplicated logic and unnecessary layers of abstraction.
2. **Complete the necessary validation and testing before submitting.** Check normal flows, edge cases, error scenarios, and existing functionality that may be affected.
3. **During human code review, please tell me directly if any of these requirements have not been met.**
