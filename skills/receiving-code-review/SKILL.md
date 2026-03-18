---
name: receiving-code-review
description: Use when code review feedback arrives, before any implementation starts.
---

# Code Review Intake (Read-Only Dispatch)

## Overview

This skill is a read-only intake and dispatching workflow.

Do not implement production fixes here. Triage the feedback, classify the resulting work, present it to the user for approval, then close out and wait.

**Core principle:** Verify before proposing work. Ask before assuming. User approval before queue updates.

## Workflow

1. Intake
   - Read all feedback fully without reacting.
   - Restate each item as a technical requirement.
2. Clarity gate
   - If any item is unclear, stop and ask clarification questions.
   - Resume only after all items are clear.
3. Triage verification
   - Verify each item against repository reality at triage depth.
   - Optional: run existing tests or add temporary test cases to confirm behavior.
4. Classification
   - Classify follow-up work as `project`, `multiple phases`, `single phase/workstream`, or `single task`.
5. User review gate
   - Present suggested work package: name, classification, and source review document.
   - Explicitly include approve/change/reject/defer options.
6. Decision handling
   - If approved or changed: update work queue item(s) and update review document only when scope direction changed.
   - If classification is `multiple phases`, queue multiple items as needed so all approved follow-up work is represented.
   - If rejected: record outcome and do not dispatch.
   - If deferred: add a revisit item at the end of queue and do not dispatch.
7. Closeout
   - Run closeout workflow.
   - Wait for further user instruction.

## Forbidden Responses

**NEVER:**
- "You're absolutely right!" (performative, not technical)
- "Great point!" / "Excellent feedback!" (performative)
- "Let me implement that now" (this skill is read-only)

**INSTEAD:**
- Restate the technical requirement
- Ask clarifying questions
- Push back with technical reasoning if wrong
- Present proposed work and wait for user decision

## Handling Unclear Feedback

```
IF any item is unclear:
  STOP - do not classify or queue work yet
  ASK for clarification on unclear items

WHY: Items may be related. Partial understanding = wrong implementation.
```

**Example:**
```
your human partner: "Fix 1-6"
You understand 1,2,3,6. Unclear on 4,5.

❌ WRONG: Implement 1,2,3,6 now, ask about 4,5 later
✅ RIGHT: "I understand items 1,2,3,6. Need clarification on 4 and 5 before proceeding."
```

## Source-Specific Handling

Use a simple trust model:
- From your human partner: treat as trusted intent, still clarify ambiguity.
- From external reviewers: verify context more deeply before proposing work.
- For any source: no performative agreement, reason technically, and proceed to the user gate (no direct implementation).

## YAGNI Check for "Professional" Features

```
IF reviewer suggests "implementing properly":
  grep repository for actual usage

  IF unused: "This endpoint isn't called. Remove it (YAGNI)?"
  IF used: Then implement properly
```

**your human partner's rule:** "You and reviewer both report to me. If we don't need this feature, don't add it."

## Optional Validation

```
AFTER triage verification:
  - You may run existing tests to validate reported behavior
  - You may add temporary test cases to confirm behavior
  - Do not implement production code fixes in this skill
```

## Classification And User Gate

Classify accepted follow-up work as one of:
- `project`
- `multiple phases` (inside an existing project)
- `single phase/workstream`
- `single task`

At the user review gate, present:
- proposed work name
- classification
- source review document
- defer/reject options

Do not dispatch directly. Wait for user approval or scope edits.

## Queue Item Format

Use concise naming with explicit classification:

`<Classification> xxx <topic> from <review doc>.md`

Examples:
- `Phase xxx authentication hardening from review-2026-03-17.md`
- `Task xxx null-check cleanup from review-2026-03-17.md`
- Deferred revisit item (end of queue): `Task revisit xxx review from <review doc>.md`

## After User Decision

- If approved/changed: update work queue and update review document only when user-directed scope changes require it.
- If classification is `multiple phases`: queue multiple items when needed so no approved phase/workstream is lost.
- If rejected: record outcome and do not dispatch.
- If deferred: add `Task revisit xxx review from <review doc>.md` at the end of queue, record outcome, and do not dispatch.
- Run closeout workflow and wait for further instruction.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Implementing during intake | Keep this skill read-only |
| Partial understanding | Clarify all unclear items first |
| Blind agreement | Verify and reason technically |
| Auto-dispatching work | Require explicit user gate |
| Vague queue naming | Use concise classified format |
