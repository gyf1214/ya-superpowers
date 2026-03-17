---
name: using-superpowers
description: Use when starting any conversation to classify work and invoke the correct skills before any response or action.
---

# Using Superpowers

## Non-Negotiable Rule

Before any response or action, decide whether a skill might apply. If there is even a 1% chance, load the skill first.

Do not do "quick checks" before this. Do not rely on memory of old skill versions. Do not take one implementation step and "come back" to workflow later.

## Instruction Priority

1. User instructions (`AGENTS.md`, direct user requests)
2. Superpowers skills
3. Default system behavior

If a skill conflicts with explicit user direction, follow the user.

## Work Hierarchy First

For work that may change repository state, classify first:

- `project`
- `multiple phases` (inside existing project)
- `single phase/workstream`
- `single task`

Default workflow: classify -> design -> plan -> execute.

Use `repository/repo` terminology.

For read-only requests (for example investigation, explanation, or review), select skills directly from the request type instead of forcing work-hierarchy classification.

Treat classification as session state:

- Reuse existing classification when still correct.
- Reclassify explicitly when scope changes.
- If scope emerges during discussion, state inferred class and continue.
- If unclear and high-risk, ask one concise question.
- Otherwise pick best-fit and proceed.
- Precedence: explicit user statement -> persisted session memory -> current-request inference.

## Skill Ordering

When multiple skills apply:

1. Process skills first (classification, brainstorming, debugging)
2. Execution-quality skills second (`test-driven-development`, `verification-before-completion`)

## Routing

- Design work -> `brainstorming`
- Plan creation -> `writing-plans`
- Plan execution -> `executing-plans`
- Behavior changes during implementation -> `test-driven-development`
- Before completion claims -> `verification-before-completion`
- Read-only bug investigation -> `systematic-debugging`
- Code review flows -> `requesting-code-review` or `receiving-code-review`

If already inside an active workflow, continue it unless the user changed scope.

## Skill Closeout Workflow

For workflow skills that end a boundary (for example design, planning, execution), use this canonical closeout contract:

1. Report outcomes and unresolved items.
2. Go through unresolved questions one at a time with the user.
3. Apply each user decision immediately; if no decision is made, keep it explicit as an open item.
4. Repeat until all currently known unresolved questions are addressed or explicitly tracked.
5. Record next work.
6. Run `memory-consolidation`.
7. Confirm/wait for user instruction before starting recorded next work.

If a workflow skill has phase-specific closeout steps, apply them in addition to this contract. If there is a conflict, this closeout contract wins unless the user explicitly overrides it.

## Red Flags

If you think any of these, stop and load the relevant skill:

- "This is too simple for a skill"
- "I need to inspect files first"
- "I remember how this skill works"
- "I’ll just do one quick step"
