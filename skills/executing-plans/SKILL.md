---
name: executing-plans
description: Use when executing a written implementation plan.
---

# Executing Plans

Execute the plan exactly, verify continuously, and close the boundary cleanly.

Announce at start: "I'm using the executing-plans skill to implement this plan."

This is the default implementation workflow for planned phase/workstream work and planned standalone tasks.

## Step 1: Load And Review

Before touching code:

1. Read the plan file fully.
2. Run `git status --short --branch`.
3. Confirm workspace/branch/worktree matches plan header (`Work Level`, `Project Context`, scope).
4. Read referenced canonical design doc.
5. Read `Design Doc Execution Mode` (`reference-only` or `reconcile-pending-changes`).
6. If `Project Context` is not `none`, read the referenced `scratch/project-index/<project-slug>.md` and identify the phase entry for this plan.
7. If `reconcile-pending-changes`, identify mapped `Pending Changes` items.
8. If `reference-only`, treat design doc as reference-only.
9. Critically review for gaps, ambiguity, or sequencing risks.

If critical concerns exist, raise them and pause. Do not start execution while unresolved.

Create and maintain a task checklist before execution begins.

## Step 2: Execute Tasks

For each task:

1. Mark task in progress.
2. Follow task steps exactly (including TDD and verification steps).
3. Treat the planned commit step as part of task execution, not optional cleanup.
4. Before any planned commit step, run `verification-before-completion` for the exact commit claim.
5. Execute the planned `git add` and `git commit` commands when the task includes a commit step.
6. After the commit, run `git status --short` and confirm the task's tracked changes were committed as intended.
7. Run listed verifications and capture results.
8. Mark task complete only after every planned step, including commit, is finished.

Do not skip verification steps. Do not silently alter scope.
Do not ask for approval between planned tasks unless blocked, the plan must change, or the user explicitly requested checkpoints.

If plan mode is `reconcile-pending-changes`, synchronize canonical design doc `Pending Changes` as mapped work lands.
If project-context, keep project index phase status current (`planned -> in progress -> done` or `blocked`) as execution state changes.

## Checkpoint Cadence

Execution should remain inspectable:

- After each major task boundary, summarize what changed and what passed.
- If a task is large, report an internal checkpoint before continuing.
- Keep reports evidence-first (commands/results) rather than conclusion-first.

If user asks to pause mid-run, stop at the nearest safe checkpoint and report current state.

## Plan Deviation Protocol

If execution pressure suggests changing order/scope:

1. Stop and call out the exact mismatch.
2. Propose minimal plan delta.
3. Wait for user approval before deviating.
4. Re-verify affected assumptions before proceeding.

Do not treat implicit drift as acceptable.

## Step 3: Reconcile Execution State

After all tasks complete:

- `reconcile-pending-changes`: confirm `Pending Changes` now contains only remaining unimplemented deltas
- `reference-only`: confirm no design-doc mutation occurred

Re-open affected files and verify this explicitly.
If project-context, verify project index phase status and links match delivered design/plan/review artifacts.

## Step 4: Report, Feedback, Consolidation

1. Report implemented work, verification outcomes, and unresolved items.
2. Include command-level verification evidence in the report, not only conclusions.
3. Handle execution-specific follow-up: apply required changes and re-verify affected areas.
4. Then run the canonical `Skill Closeout Workflow` from `using-superpowers`.

## Stop Conditions

Stop immediately and ask for help if:

- plan has critical gaps blocking progress
- instructions are unclear
- dependencies are missing
- verification fails repeatedly
- actual repository state conflicts with plan assumptions

Never guess through blockers.

## Discipline Rules

- Follow plan order unless user explicitly approves change.
- Keep branch hygiene visible via `git status` checks when context shifts.
- Respect design-doc execution mode (`reference-only` vs `reconcile-pending-changes`) strictly.
- Keep project index phase status synchronized for project-context execution.
- Do not claim completion without verification evidence.
- Do not skip required skills referenced by plan tasks.

## Typical Next Work

After this skill completes and consolidation is done, common next steps are:

- `finishing-a-project`
- code review workflow
- next phase/workstream planning/execution

## Red Flags

Stop and correct if you see:

- checking off tasks without running listed verifications
- editing outside plan scope without user approval
- treating `reconcile-pending-changes` design-doc mode as optional
- reporting "done" before reconciliation and consolidation
