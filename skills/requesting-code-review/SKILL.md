---
name: requesting-code-review
description: Use when completing tasks, implementing major features, or before merging to verify work meets requirements
---

# Requesting Code Review

Prepare a focused review request document and hand off to a separate review session.

**Core principle:** Review early, review often.

**Boundary:** This skill prepares the request only. It does not perform the review or process feedback.

## When to Request Review

**Mandatory:**
- After completing major feature
- Before project handoff

**Optional but valuable:**
- When stuck (fresh perspective)
- Before refactoring (baseline check)
- After fixing complex bug

## How to Request

**1. Get git SHAs:**
```bash
BASE_SHA=$(git rev-parse HEAD~1)  # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
```

**2. Create review request document:**
- Path: `scratch/review_requests/YYYY-MM-DD-<topic>-review-request.md`
- Keep review request artifacts local (do not commit them)

Use this template:

```markdown
# Code Review Request: <topic>

## 1. Context
- Task/Workstream: <name>
- Goal: <what this change should achieve>
- Requirements Source: <spec/issue/plan path or brief requirements>

## 2. Change Summary
- What changed:
  - <key change 1>
  - <key change 2>
- Why this approach:
  - <brief rationale and key tradeoff>

## 3. Diff Scope
- Base SHA: `<sha>`
- Head SHA: `<sha>`
- Touched files:
  - `<path1>` - <why touched>
  - `<path2>` - <why touched>

## 4. Reviewer Focus Items (Required)
1. <question or focus statement>
2. <question or focus statement>
3. <question or focus statement>

## 5. Validation Evidence
- Tests run:
  - `<command>` -> <result>
- Manual checks:
  - <what was verified>
- Known limitations / not tested:
  - <gaps reviewer should know>

## 6. Review Output Location
- Store review at: `scratch/review_requests/YYYY-MM-DD-<topic>-review.md`
```

**3. Record next work**
- Record next work as: `code review on request <request_path>`
- Example: `code review on request scratch/review_requests/2026-03-17-auth-review-request.md`

**4. Run closeout workflow**
- Apply `Skill Closeout Workflow` from `using-superpowers`.
- Keep unresolved items explicit.
- Run `memory-consolidation`.

**5. Announce handoff**
- Tell the user to start a new session for the review task.
- Include the request path in that handoff message.

## Next Work

`code review on request <request_path>`

## Red Flags

**Never:**
- Skip review because "it's simple"
- Mix review execution rules into this request-preparation skill
- Start implementing review feedback in the same request-preparation flow
