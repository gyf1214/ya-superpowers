---
name: executing-plans
description: Use when executing a written implementation plan
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**Note:** In this repository, this is the default implementation execution workflow.

**Work hierarchy:** This skill executes planned `phase/workstream` work and planned standalone `task` work.

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Run `git status --short --branch` to verify current branch and worktree status
3. Confirm the current workspace is aligned with the plan header (`Work Level`, `Parent Context`, and intended execution scope)
4. Read the referenced canonical design doc file (`scratch/designs/<component-or-feature>.md`)
5. Read `Design Doc Status In This Plan` (`unchanged` or `updated`) from the plan header
6. If status is `updated`, identify mapped `Pending Changes` items
7. If status is `unchanged`, treat the design doc as reference-only for this execution run
8. Review critically - identify any questions or concerns about the plan
9. If concerns: Raise them with your human partner before starting
10. If no concerns: Create and maintain a task checklist and proceed

### Step 2: Execute Tasks

For each task:
1. Mark as in_progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Mark as completed

If the plan header says `Design Doc Status In This Plan: updated`:
- As mapped `Pending Changes` items are implemented, update the canonical design doc's `Pending Changes` section
- Keep only still-unimplemented deltas in that section

### Step 3: Reconcile Execution State

After all tasks complete and verified:
- If plan status is `updated`, re-open canonical design doc and confirm `Pending Changes` is reconciled with delivered work
- If plan status is `unchanged`, confirm no design-doc mutation was performed during execution

### Step 4: User Feedback And Consolidation

- Present execution results and unresolved items to the user, then go through open questions one by one
- If user feedback requires changes, apply them and re-verify affected work before finishing this step
- Ask the user what the next work should be, then record that next work
- Run `memory-consolidation` after user feedback is addressed

## Post-Execution Next-Work Confirmation

After Step 4 is complete (including `memory-consolidation`):
- Wait for user instruction to start the recorded next work.
- Common next-work examples:
  - `finishing-a-project`
  - code review
  - begin a new phase/workstream

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Respect `Design Doc Status In This Plan`:
  - `updated`: keep canonical design doc `Pending Changes` synchronized with implementation progress
  - `unchanged`: keep design doc reference-only
- Stop when blocked, don't guess
