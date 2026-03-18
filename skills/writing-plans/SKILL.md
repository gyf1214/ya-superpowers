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

Before tasking, verify scope shape.

If design doc still bundles multiple independent subsystems, split into separate plans (one per subsystem). Each plan must deliver testable value independently.

Do not create one monolithic plan for unrelated subsystems.

## File Map Discipline

Before writing tasks, define intended file structure:

- exact files to create/modify/test
- responsibilities per file
- interfaces/boundaries between units

Rules:

- Prefer small, focused files with single clear responsibility.
- Follow repository conventions and existing patterns.
- Include the design doc in the file map as `reference-only` or `update Pending Changes`.

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

Every task must include:

- exact create/modify/test paths
- TDD flow when applicable: failing test -> minimal implementation -> passing test
- verification commands with expected outcomes
- design-doc handling instruction (`reference-only` or `update Pending Changes`)
- commit step when tracked files changed

Keep steps bite-sized (roughly 2-5 minutes each).

For `Project Context` not `none`, include one explicit early execution step to update project index phase status to `in progress`.

## Verification Discipline In Plans

For behavior-changing tasks, plan steps must explicitly include:

1. command to run failing test
2. expected failure signal
3. command to run passing test
4. expected pass signal
5. broader regression/targeted suite verification when appropriate

Do not allow vague statements such as "run tests" without concrete commands.

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

1. Self-review scope coverage, file-path validity, dependency order, test flow, and design-doc alignment.
2. If `Project Context` is not `none`, verify the plan includes the early project-index `in progress` sync step.
3. Verify each task has clear completion evidence.
4. Fix issues and re-review until clean.
5. If loop exceeds 5 iterations or major ambiguity remains, ask user for direction.

Do not hand off an unreviewed plan.

## User Feedback And Closeout

After plan review passes:

1. Handle planning-specific deltas first: if user decisions materially change scope/order/verification, re-run the Plan Review Loop before handoff.
2. If `Project Context` is not `none`, update the relevant project index phase entry: set status to `planned` and set the `plan doc` link.
3. Then run the canonical `Skill Closeout Workflow` from `using-superpowers`.
4. Record next work as plan execution with `executing-plans`.

Use this confirmation format:

"Plan complete and saved to `scratch/plans/<filename>.md`. Open questions were reviewed, next work is execution with `executing-plans`, and memory is consolidated. Confirm and I will proceed."

## Red Flags

Stop and fix if you see:

- vague file paths
- implementation-first steps without failing tests
- missing verification commands
- missing design-doc reconciliation behavior
- handoff without user confirmation
