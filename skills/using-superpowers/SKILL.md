---
name: using-superpowers
description: Use when starting any conversation to classify work and invoke skills before responding.
---

# Using Superpowers

## Non-Negotiable Rule

Before any response or action, decide whether a skill might apply. If there is even a 1% chance, load it.

## Instruction Priority

1. User instructions (`AGENTS.md`, direct user requests)
2. Superpowers skills
3. Default system behavior

If a skill conflicts with explicit user direction, follow the user.

## Work Classification First

For repository-changing work, classify first:

- `project`
- `multiple phases` (inside existing project)
- `single phase/workstream`
- `single task`

Use `repository` to refer to the workspace.

For read-only requests, select skills directly from request type.

Temporary diagnostic edits may skip classification only when the active skill allows it, and must be reverted before that workflow ends.

Treat classification as session state:

- If already classified and still valid, do not classify again.
- Reclassify only when scope changes or new work is needed.

## General Workflow

- new `project` work -> `starting-a-project` -> design -> plan -> execute
- `single task` with design/behavior change -> design -> execute
- `single task` without design/behavior change -> execute
- `single phase/workstream` or larger -> design -> plan -> execute

For project-context work, keep `scratch/project-index/<project-slug>.md` synchronized at phase boundaries.

## Code Review Workflow

- Prepare a review package/request -> `requesting-code-review`
- Intake review feedback and dispatch follow-up work (read-only) -> `receiving-code-review`

For review-driven follow-up, return to general workflow with approved classification.

## Skill Quick Reference

- Start new project branch/index -> `starting-a-project`
- Design work -> `brainstorming`
- Plan creation -> `writing-plans`
- Plan execution -> `executing-plans`
- Create/edit/verify skills -> `writing-skills`
- Behavior changes during implementation -> `test-driven-development`
- Before completion claims -> `verification-before-completion`
- Prepare code review request -> `requesting-code-review`
- Intake code review feedback (read-only dispatch) -> `receiving-code-review`
- Project-level closure on project branch -> `finishing-a-project`
- Read-only bug investigation -> `systematic-debugging`

## Skill Ordering

When multiple skills apply:

1. Process skills first (classification, brainstorming, debugging)
2. Execution-quality skills second (`test-driven-development`, `verification-before-completion`)

If already inside an active workflow, continue it unless the user changed scope.

## Skill Closeout Workflow

Use this closeout contract if the active skill requires it:

1. Report outcomes and unresolved items.
2. Go through unresolved questions one at a time with the user.
3. Apply each user decision immediately; if no decision is made, keep it explicit as an open item.
4. Repeat until all currently known unresolved questions are addressed or explicitly tracked.
5. Record next work.
6. Run `memory-consolidation`.
7. Confirm/wait for user instruction only after a workflow boundary completes, not between internal steps or planned tasks.

If a workflow skill has phase-specific closeout steps, apply both.

## Red Flags

If you think any of these, stop and load the relevant skill:

- "This is too simple for a skill"
- "I need to inspect files first"
- "I remember how this skill works"
- "I’ll just do one quick step"
