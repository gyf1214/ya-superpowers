---
name: receiving-code-review
description: Use when code review feedback arrives, before any implementation starts.
---

# Code Review Intake

## Overview

This skill is a read-only intake and dispatching workflow.

Do not implement production fixes here. Triage the feedback, classify the resulting work, present it to the user for approval, then close out and wait.

**Core principle:** Verify before proposing work. Ask before assuming. User approval before queue updates.

## Workflow

**Step 1: Intake**
- Read all feedback fully without reacting.
- Restate each item as a technical requirement.

**Step 2: Clarity Gate**
- If any item is unclear, stop; do not classify or queue work yet. Ask clarification questions.
- Resume only after items are clear.

Example:
`your human partner: "Fix 1-6"` -> if items 4 and 5 are unclear, ask for clarification before classification or queue updates.

**Step 3: Triage Verification**
- Verify each item against repository reality at triage depth.
- Optional: run existing tests or add temporary test cases to confirm behavior.

**Step 4: Classification**
- Classify follow-up work as `project`, `multiple phases`, `single phase/workstream`, or `single task`.

**Step 5: User Review Gate**
- Present suggested work package: name, classification, and source review document.
- Explicitly include approve/change/reject/defer options.
- Do not dispatch directly; wait for user approval or scope edits.

**Step 6: Decision Handling**
- If approved or changed: update work queue item(s) and update review document only when user-directed scope changes require it.
- If approved/changed work is project-context, update affected phase entry in `scratch/project-index/<project-slug>.md` (status and/or review link) before closeout.
- If classification is `multiple phases`, queue multiple items as needed so no approved phase/workstream is lost.
- If rejected: record outcome and do not dispatch.
- If deferred: add `Task revisit xxx review from <review doc>.md` at the end of queue, record outcome, and do not dispatch.

**Step 7: Closeout**
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

## Source-Specific Handling

Use a simple trust model:
- From your human partner: treat as trusted intent, still clarify ambiguity.
- From external reviewers: verify context more deeply before proposing work.
- For any source: no performative agreement, reason technically, and proceed to the user gate (no direct implementation).

## YAGNI Check for "Professional" Features

- If reviewer asks to "implement properly", check repository usage first.
- If unused, propose removal (`YAGNI`) instead of adding complexity.
- If used, keep it in scope for follow-up work.

**your human partner's rule:** "You and reviewer both report to me. If we don't need this feature, don't add it."

## Optional Validation

- After triage verification, you may run existing tests to validate reported behavior.
- You may add temporary test cases to confirm behavior.
- Do not implement runtime code fixes in this skill.

## Queue Item Format

Use concise naming with explicit classification:

`<Classification> xxx <topic> from <review doc>.md`

Examples:
- `Phase xxx authentication hardening from review-2026-03-17.md`
- `Task xxx null-check cleanup from review-2026-03-17.md`
- Deferred revisit item (end of queue): `Task revisit xxx review from <review doc>.md`

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Implementing during intake | Keep this skill read-only |
| Partial understanding | Clarify all unclear items first |
| Blind agreement | Verify and reason technically |
| Auto-dispatching work | Require explicit user gate |
| Vague queue naming | Use concise classified format |
