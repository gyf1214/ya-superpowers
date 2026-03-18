---
name: systematic-debugging
description: Use when triaging bugs, test failures, or unexpected behavior to identify likely root cause and dispatch follow-up work without persistent code changes.
---

# Systematic Debugging

## Overview

Use this skill for investigation and dispatch, not implementation.

**Core principle:** triage first, fix later.

## The Iron Law

```
NO PERSISTENT FIXES IN THIS SKILL
```

Temporary diagnostic edits are allowed. Permanent repository changes are not.

## When to Use

Use for:
- Test failures
- Runtime bugs
- Unexpected behavior
- Performance problems
- Build failures
- Integration issues

Do not use for:
- Root cause already known.
- Small coding mistake during active workflow.

## Workflow

**Step 1: Define symptom and reproduction**:
- Capture failure signal, scope, and reproduction steps.
- Note whether reproduction is consistent or intermittent.

**Step 2: Gather evidence**:
- Read full errors, traces, and logs.
- Check recent changes and environmental differences.
- For multi-component systems, instrument boundaries to locate where failure starts.

**Step 3: Use temporary diagnostics when needed**:
- Allowed: debug logs, temporary assertions, short-lived tests, scripts.
- Mark edits clearly (for example `TEMP DEBUG:`).
- Use only to improve evidence and isolate cause.

**Step 4: Form and test hypotheses**:
- Test one hypothesis at a time with minimal diagnostic change.
- If a hypothesis fails, record result and iterate.
- Do not convert diagnostics into production fixes.

**Step 5: Produce triage outcome**:
- Likely root cause (or narrowed candidate set).
- Evidence summary and confidence.
- Impacted components and risk notes.
- Clear recommended next workflow for implementation.

**Step 6: Clean up and dispatch**:
- Remove/revert temporary diagnostic changes before handoff.
- Keep only notes/artifacts in `scratch/` when useful.
- Dispatch to the appropriate workflow skill.

## Next Work Routing

**Case 1: Single-task fix**:
- If there is no design change, dispatch to `test-driven-development`.
- Then use `verification-before-completion` before any completion/correctness claim.

**Case 2: Larger issue or any design change**:
- Return to work classification and skill closeout routing in `using-superpowers`.

## Red Flags

Stop and correct if you see:
- proposing a production fix during triage
- keeping temporary diagnostic edits as permanent code
- bundling triage and implementation in one workflow pass
- dispatching without evidence or reproduction steps

## Quick Reference

| Stage | Output |
|-------|--------|
| Reproduce | Reliable symptom description |
| Evidence | Logs/traces/diffs showing failure path |
| Hypothesis tests | Confirmed/rejected causes |
| Triage result | Likely cause + confidence + impacted scope |
| Dispatch | Next workflow + cleaned repository state |

## Common Mistakes

| Mistake | Correction |
|---------|------------|
| "I already know the fix" | Finish triage evidence first, then dispatch |
| "These debug changes are useful, keep them" | Recreate intentionally in implementation workflow if still needed |
| "One quick permanent patch while I'm here" | Do not ship fixes from this skill |
| "No need to clean up temp tests/logs" | Cleanup is mandatory before dispatch |
