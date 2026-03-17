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
3. Confirm workspace/branch/worktree matches plan header (`Work Level`, `Parent Context`, scope).
4. Read referenced canonical design doc.
5. Read `Design Doc Status In This Plan` (`unchanged` or `updated`).
6. If `updated`, identify mapped `Pending Changes` items.
7. If `unchanged`, treat design doc as reference-only.
8. Critically review for gaps, ambiguity, or sequencing risks.

If critical concerns exist, raise them and pause. Do not start execution while unresolved.

Create and maintain a task checklist before execution begins.

## Step 2: Execute Tasks

For each task:

1. Mark task in progress.
2. Follow task steps exactly (including TDD and verification steps).
3. Run listed verifications and capture results.
4. Mark task complete.

Do not skip verification steps. Do not silently alter scope.

If plan status is `updated`, synchronize canonical design doc `Pending Changes` as mapped work lands.

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

- `updated`: confirm `Pending Changes` now contains only remaining unimplemented deltas
- `unchanged`: confirm no design-doc mutation occurred

Re-open affected files and verify this explicitly.

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
- Respect design-doc mode (`updated` vs `unchanged`) strictly.
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
- treating `updated` design-doc mode as optional
- reporting "done" before reconciliation and consolidation
