---
name: writing-plans
description: Use when phase/workstream implementation needs a written plan, or when a standalone task requires explicit planning, before execution.
---

# Writing Plans

Produce an executable plan that a skilled engineer with little repository context can follow without guesswork.

Announce at start: "I'm using the writing-plans skill to create the implementation plan."

Default plan location: `scratch/plans/YYYY-MM-DD-<feature-name>.md` (user preference overrides).

## When Required

Use this skill for:

- `phase` or `workstream` implementation
- standalone `task` work when user requests planning or planning value is high

May be skipped for trivial standalone tasks with no practical planning value.

## Inputs And Preconditions

1. Read the approved design doc (usually `scratch/designs/<component-or-feature>.md`).
2. Use `Pending Changes` as primary implementation-gap input when applicable.
3. If design changes are needed but not approved, stop and route to `brainstorming`.
4. If design is approved and unchanged for this scope, do not add unnecessary design-update tasks.

## Scope Decomposition Check

Before tasking, split unrelated subsystems into separate plans. Each plan must deliver testable value independently.

## File Map Discipline

Before writing tasks, define intended file structure and fold it into the task steps.

- Prefer small, focused files with single clear responsibility.
- Follow repository conventions and existing patterns.
- Include the design doc in the file map as `reference-only` or `update Pending Changes`.
- Do not add a separate `File Map` section to the plan unless the user asks for it.

## Required Plan Header

```markdown
# [Feature Name] Implementation Plan

**Execution Requirement:** Use `executing-plans` to implement this plan.
**Goal:** [one sentence]
**Architecture:** [2-3 sentences]
**Tech Stack:** [key tools]
**Work Level:** [phase | workstream | task]
**Project Context:** [scratch/project-index/<project-slug>.md, branch <branch-name> | none]
**Design Reference:** [approved design doc path]
**Design Doc Status In This Plan:** [unchanged | updated]

---
```

## Required Task Structure

Plans must include a `## Tasks` section and use this task format:

```markdown
## Tasks

### Task 1: [Title]
[Short summary of the task scope and purpose]

**Step 1: [Title]**
Modify [path]
Add [path]
Delete [path]
[Step description]

**Step 2+: [Title]**
[Use `Modify`, `Add`, and `Delete` lines as needed.]
[Step description]

**Step N: Commit**
[`git add` and `git commit` command with co-author trailer]
[Commit the tracked changes from earlier steps. Do not modify files in this step.]
```

Every task must include:

- a short summary of scope and purpose
- exact create/modify/test paths in each step using explicit `Modify`, `Add`, and `Delete` lines as needed
- TDD flow when applicable: failing test -> minimal implementation -> passing test
- verification commands with expected outcomes
- an explicit final commit step when tracked files changed
- no file-action lines in the commit step

Keep steps bite-sized (roughly 2-5 minutes each).

If `Design Doc Status In This Plan` is `updated`, include an explicit step to update the design doc.

For `Project Context` not `none`, include one explicit early execution step to update project index phase status to `in progress`.

## Verification Discipline In Plans

For behavior-changing tasks, follow `test-driven-development`: include explicit red, green, and regression verification commands. Do not write vague steps such as "run tests".

## Commit Guidance

Plans must include co-author trailer in commit examples:

```bash
git add <files>
git commit -m "feat: concise change summary

Co-authored-by: Codex <codex@openai.com>"
```

Never include ignored files.

## Plan Review Loop

After drafting:

1. Verify the plan satisfies the required header and task structure.
2. Self-review scope coverage, file-path validity, dependency order, and test flow.
3. Fix issues and re-review until clean.
4. If loop exceeds 5 iterations or major ambiguity remains, ask user for direction.

Do not hand off an unreviewed plan.

## User Feedback And Closeout

After plan review passes:

1. Handle planning-specific deltas first: if user decisions materially change scope/order/verification, re-run the Plan Review Loop before handoff.
2. If `Project Context` is not `none`, update the relevant project index phase entry: set status to `planned` and set the `plan doc` link.
3. Then run the canonical `Skill Closeout Workflow` from `using-superpowers`.
4. Record next work as plan execution with `executing-plans`.

## Red Flags

Stop and fix if you see:

- vague file paths
- implementation-first steps without failing tests
- missing verification commands
- missing design-doc reconciliation behavior
- handoff without user confirmation
