---
name: writing-plans
description: Use when phase/workstream implementation needs a written plan, or when a standalone task requires explicit planning, before execution.
---

# Writing Plans

Announce: "I'm using the writing-plans skill."

Default plan location: `scratch/plans/YYYY-MM-DD-<feature-name>.md` unless user preference overrides.

## When Required

- `phase` or `workstream` implementation
- standalone `task` work when user requests planning or planning value is high

Skip trivial standalone tasks with no practical planning value.

## Inputs And Preconditions

1. Read the approved design doc (`scratch/designs/<component-or-feature>.md`).
2. Treat the current design doc contents as authoritative repository state.
3. If it contains approved `Pending Changes` relevant to this scope, use them as the primary implementation-gap input and map them into plan tasks.
4. Set `Design Doc Execution Mode` from execution responsibility, not session history:
   - `reference-only`: execution should not modify the design doc
   - `reconcile-pending-changes`: execution should reconcile implemented `Pending Changes` items as work lands
5. Do not infer `reference-only` just because the design doc was not edited in the current planning session.
6. If design changes are needed but not approved, stop and route to `brainstorming`.
7. If the design doc is strictly reference-only for this scope, do not add unnecessary design-update tasks.

## Scope Decomposition Check

Split unrelated subsystems into separate plans.

## File Map Discipline

Before writing tasks, define intended file structure and fold it into task steps.

- Prefer small, focused files with single clear responsibility.
- Follow repository conventions and existing patterns.
- Include the design doc in the file map as `reference-only` or `reconcile Pending Changes`.
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
**Design Doc Execution Mode:** [reference-only | reconcile-pending-changes]

---
```

## Required Task Structure

Plans must include a `## Tasks` section in this format:

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
- exact create/modify/test paths in each step using explicit `Modify`, `Add`, and `Delete` lines
- TDD flow when applicable: failing test -> minimal implementation -> passing test
- verification commands
- an explicit final commit step when tracked files changed
- no file-action lines in the commit step
- when `Pending Changes` are in scope, explicit mapping from each implemented item to the task or step that lands it

If `Design Doc Execution Mode` is `reconcile-pending-changes`, include explicit steps to update the design doc as work lands.

For `Project Context` not `none`, include one early step to set project index phase status to `in progress`.

## Verification Discipline In Plans

For behavior-changing tasks, follow `test-driven-development`: include explicit red, green, and regression verification commands. Do not write vague steps like "run tests".

## Commit Guidance

Plans must include co-author trailer in commit examples:

```bash
git add <files>
git commit -m "feat: concise change summary

Co-authored-by: Codex <codex@openai.com>"
```

Never include ignored files.

## Plan Review Loop

1. Verify the plan satisfies the required header and task structure.
2. Self-review scope coverage, file-path validity, dependency order, and test flow.
3. Fix issues and re-review until clean.
4. If the loop exceeds 5 iterations or major ambiguity remains, ask user for direction.

Do not hand off an unreviewed plan.

## User Feedback And Closeout

1. Handle planning-specific deltas first: if user decisions materially change scope, order, or verification, re-run the Plan Review Loop before handoff.
2. If `Project Context` is not `none`, update the relevant project index phase entry: set status to `planned` and set the `plan doc` link.
3. Then run the canonical `Skill Closeout Workflow` from `using-superpowers`.
4. Record next work as plan execution with `executing-plans`.
5. After presenting the plan, stop. Do not invoke `executing-plans` until the user explicitly approves the plan or instructs execution.

## Red Flags

Stop and fix if you see:

- vague file paths
- implementation-first steps without failing tests
- missing verification commands
- missing `Pending Changes` mapping or design-doc reconciliation behavior when required
- handoff without user confirmation
- invoking `executing-plans` without explicit plan approval
