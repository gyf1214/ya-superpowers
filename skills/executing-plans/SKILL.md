---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
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
2. Read the referenced canonical spec file (`scratch/designs/<component-or-feature>.md`)
3. Identify `Migration / Pending Changes` items mapped by this plan
4. Review critically - identify any questions or concerns about the plan
5. If concerns: Raise them with your human partner before starting
6. If no concerns: Create TodoWrite and proceed

### Step 2: Execute Tasks

For each task:
1. Mark as in_progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Mark as completed

As migration items are implemented:
- Update the canonical spec's `Migration / Pending Changes` section so implemented items are removed or marked as resolved
- Keep only still-unimplemented deltas in that section

### Step 3: Close the Project Cycle

After all tasks complete and verified:
- Re-open canonical spec and confirm `Migration / Pending Changes` is reconciled with delivered work
- If this plan is part of a `project` branch closure:
  - Announce: "I'm using the finishing-a-project skill to close this work."
  - **REQUIRED SUB-SKILL:** Use finishing-a-project
  - Follow that skill to verify tests/git status and create the project-closing document
- If this plan is an independent workstream/task:
  - Run verification-before-completion checks and report completion evidence without project-closure doc unless requested

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Keep canonical spec migration section synchronized with implementation progress
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent

## Integration

**Required workflow skills:**
- **writing-plans** - Creates the plan this skill executes
- **finishing-a-project** - Required when closing a project branch
