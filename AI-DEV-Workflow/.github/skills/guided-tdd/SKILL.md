---
name: guided-tdd
description: Build a spec or request through an approved TDD plan, optionally matching a prototype page's appearance and interactions.
argument-hint: "<spec-path-or-quoted-request> [prototype-page]"
disable-model-invocation: true
---

# Guided TDD

Wrap `/tdd` with a **gate**: before any test or production code is written, the user approves exactly what will be built. The approved **build plan** is the contract for the loop; nothing outside it is built without asking.

## 1. Establish the input and evidence

Usage: `/guided-tdd <spec-path-or-quoted-request> [prototype-page]`. Quote paths containing spaces and free-text requests when supplying the second argument. Existing free-text invocations without a prototype remain valid.

The first argument defines the work: a spec path (for example `implementation-docs/spec-s02.md`) or a free-text request. If neither is given, ask and wait.

When the optional second argument is supplied, read [PROTOTYPE.md](PROTOTYPE.md) and apply its requirements during evidence gathering, plan approval, implementation, and verification. Without it, follow the existing spec/request workflow.

Example: `/guided-tdd implementation-docs/spec-s02.md prototype/timer.html`.

- **Spec**: read it, plus the sources it links (stories, ACs, test cases). If it already contains an `Implementation Plan` section, treat that plan as settled and identify only what changed since it was written, in the spec, the code, or the supplied prototype.
- **Free text**: every decision category in step 2 is open. If the request reads like a whole user story rather than a contained change, suggest running `/story-to-implementation-spec` first, then continue if the user prefers.

Read the code the work touches, the glossary, ADRs in the area, and existing tests as prior art. Explore as much as a confident proposal needs; delegate broad, multi-location searches to an Explore sub-agent so only conclusions return.

For every web app, with or without a prototype, inspect its source, dependency declarations, and build tooling. Preserve the existing framework and package manager. Identify missing setup for the build-workflow proposal in step 2.

Complete this step when every module, interface, and test pattern the proposal will name has been observed in the repository or is explicitly marked as new.

## 2. Run the gate

Present one message with three parts:

1. **From spec**: decisions the spec (or existing plan) already settles, one line each, so the user can spot misreadings.
2. **Proposal**, covering each category:
   - **File plan**: exact files to create or modify, and each one's responsibility.
   - **Interfaces**: signatures or contracts with inputs, outputs, and error behavior. Sketches only; no implementation.
   - **Data and state shape**: new states, fields, or contracts.
   - **Dependencies**: any new library, or "none".
   - **Build workflow**, for every web app: a reproducible project with declared dependencies, tooling that builds production output from app source, and exact commands to install dependencies, start a development server, build, and serve the output over HTTP. Propose missing setup and production browser checks covering the in-scope behavior. For example, a new Vue app may use Vite with `npm install`, `npm run dev`, `npm run build`, and `npm run preview`.
   - **Seams**: the public boundaries under test, each with a one-line note on what it catches and what it misses.
   - **Test list**: ordered, one line per vertical slice, each linked to its TC/AC ID where one exists. This is exactly the set of red → green cycles the loop will run.
   - **Untouched**: what is explicitly out of scope.
   - **Prototype target**, when supplied: the reference page, in-scope visual states and interactions, resolved conflicts, and visual and interaction checks from `PROTOTYPE.md`.
3. **Questions**: the real gaps only, run as grilling rounds. Find the `grilling` skill using the lookup order in step 4 and follow its round format: numbered questions, each with a recommended answer, "yes" accepts.

Repeat rounds, updating the proposal, until no question is open. Then ask for approval.

Complete this step when the user explicitly approves the plan. Questions, feedback, or answers to rounds are not approval.

Approval is the **go** signal for the whole build: from here, run steps 3 → 4 → 5 straight through in the same turn, pausing only for step 4's stop-and-ask triggers.

## 3. Record the plan in the spec

Append `## Implementation Plan` to the spec, directly before `Out of Scope`, in the spec's language. Open it with one line marking it as the build plan for this implementation (the one section of the spec where file paths belong). Include the file plan, interface signatures, seams, and the test list as unchecked checkboxes with TC/AC links. When re-running against an existing plan, edit it in place.

For free-text input with no spec, keep the approved plan in chat; step 2's approval completes this step.

For web apps, record the build workflow and its verification checks as unchecked checkboxes, separate from the TDD test list.

When a prototype is supplied, also record the prototype target and its verification checks as unchecked checkboxes, separate from the TDD test list.

Once the section is written, re-read it against the plan approved in step 2 and correct any line that reads differently. Link to it in one line.

Complete this step when the section reflects every approved decision, then begin step 4 immediately. The spec update is a waypoint on the way to working code; the approval from step 2 already covers the build.

## 4. Run /tdd against the plan

Load `/tdd`: if a `tdd` skill is invocable through the Skill tool, invoke it; otherwise find `tdd/SKILL.md` in this order and stop at the first match: `.github/skills/`, `~/.copilot/skills/`, `~/.claude/skills/`, `~/.agents/skills/`. If it is missing, report the missing dependency and ask for its location. Follow it for every cycle, writing real tests and production code, with these overrides:

- **Seams and the test list are pre-agreed.** Use the approved ones; skip /tdd's seam confirmation.
- **Start** at the first unchecked test in the plan.
- **Report** one line per finished slice (`✓ T3 paused tick keeps remaining time`) and continue without pausing.
- **Decide freely**: internal implementation choices, and extra tests for agreed behavior at an agreed seam (report each as added).
- **Stop and ask** before changing a public interface, the scope, a seam, a dependency, or adding new behavior. Explain the proposed change and wait for approval; once approved, update the plan section immediately.

For web apps, run the approved production build, serve its output over HTTP, and perform the planned browser checks against that output. Run relevant project lint and test scripts as well.

Complete this step when every test in the plan is green and, for web apps, the production build and served-output browser checks pass. If a required check is blocked, report the blocker and leave it unverified; the implementation is incomplete.

When a prototype is supplied, implement the approved visual target alongside the behavior slices and complete the verification in `PROTOTYPE.md` before declaring the work complete.

## 5. Wrap up

Tick the completed tests in the plan section and add a short `Deviations` list under it (what changed from the approved plan, and why), or "None". Report the planned test list against what was built, and the final test-run result. Leave committing to the user.

For web apps, tick verified build-workflow checks and hand off the recorded commands and verification results. Leave blocked or unverified checks unchecked.

When a prototype is supplied, also tick verified prototype checks and report visual and interaction results, with unresolved or unverified items left unchecked.
