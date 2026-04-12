---
name: verification-before-completion
description: Use when about to claim work is complete, fixed, or passing, or before commit or workflow-boundary close where correctness must be verified.
---

# Verification Before Completion

## Overview

Completion claims without verification create avoidable rework and trust loss.
Verification prevents false completion claims and rework.

**Core principle:** Evidence before claims, always.

## The Iron Law

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

No fresh verification in this message means no pass claim.

## Gate Workflow

Before any completion/correctness claim, run this workflow in order:

1. Define the claim
   - Write the exact claim you want to make (for example: "tests pass", "task complete", "review triage complete").
2. Select proof
   - Choose the command/checklist that directly proves that claim.
   - For behavior-change or bug-fix claims, use `test-driven-development` evidence (`red -> green -> regression`).
   - For project/phase completion claims, include project index proof (phase checkbox/status and relevant section consistency).
3. Run verification
   - Execute the full verification now (fresh run, no cached/partial result).
4. Inspect evidence
   - Read full output and exit status; confirm the result matches the claim.
5. Report accurately
   - If verification fails or is incomplete: report actual status and gaps.
   - If verification passes: make the claim and include the supporting evidence.

Skip any step = unverified claim.

## Common Failures

| Claim | Requires | Not Sufficient |
|-------|----------|----------------|
| Tests pass | Test command output: 0 failures | Previous run, "should pass" |
| Linter clean | Linter output: 0 errors | Partial check, extrapolation |
| Build succeeds | Build command: exit 0 | Linter passing, logs look good |
| Bug fixed | Test original symptom: passes | Code changed, assumed fixed |
| Regression test works | Red-green cycle verified | Test passes once |
| Requirements met | Line-by-line checklist | Tests passing |
| Phase complete (project context) | Project index phase entry shows `[x]` and `done` | Verbal claim only |

## Red Flags - STOP

- Using "should", "probably", "seems to"
- Expressing satisfaction before verification ("Great!", "Perfect!", "Done!", etc.)
- About to commit without verification
- Relying on partial verification
- **ANY wording implying success without having run verification**

## Rationalization Prevention

| Excuse | Reality |
|--------|---------|
| "Should work now" | RUN the verification |
| "I'm confident" | Confidence ≠ evidence |
| "Just this once" | No exceptions |
| "Linter passed" | Linter ≠ compiler |
| "Partial check is enough" | Partial proves nothing |

## Key Patterns

**Tests:**
```
✅ [Run test command] [See: 34/34 pass] "All tests pass"
❌ "Should pass now" / "Looks correct"
```

**Regression tests (TDD Red-Green):**
```
✅ Write → Run (pass) → Revert fix → Run (MUST FAIL) → Restore → Run (pass)
❌ "I've written a regression test" (without red-green verification)
```

**Build:**
```
✅ [Run build] [See: exit 0] "Build passes"
❌ "Linter passed" (linter doesn't check compilation)
```

**Requirements:**
```
✅ Re-read plan → Create checklist → Verify each → Report gaps or completion
❌ "Tests pass, phase complete"
```

## When To Apply

Treat this as a completion gate across design, planning, execution, and review workflows.

**ALWAYS before:**
- Any claim that repository work is complete, fixed, or passing
- Any positive status statement about work quality or correctness
- Committing changes
- Declaring a `task`, `phase/workstream`, or `project` complete
- Pausing, handing off, or reporting completion before the next `task` or `phase/workstream`
- Closing a design boundary ("design ready/approved")
- Closing a planning boundary ("plan ready for execution")
- Closing a review boundary ("triage verified/complete")
- Declaring project closure readiness based on project index state

**Rule applies to:**
- Exact phrases and paraphrases
- Any communication implying completion/correctness

## The Bottom Line

**No shortcuts for verification.**

Run the command. Read the output. THEN claim the result.

This is non-negotiable.
