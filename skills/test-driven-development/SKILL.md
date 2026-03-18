---
name: test-driven-development
description: Use when implementing any feature or bugfix, before writing implementation code
---

# Test-Driven Development (TDD)

## Overview

Write the test first. Watch it fail. Write minimal code to pass.

**Core principle:** If you didn't watch the test fail, you don't know if it tests the right thing.

## When to Use

Strict TDD applies to testable behavioral changes.
Always use Strict TDD for testable bug fixes.

- Non-behavioral code changes (refactor/renaming): Constrained Workflow.
- Non-testable code: Constrained Workflow REQUIRES special instruction or user approval.
- docs/chore: not required.

### Identify Test Infrastructure

- Test infrastructure exists -> strict TDD required.
- Code requires heavy integration -> check for special instruction; ask user if not clear.
- No test infrastructure -> ask user.
- Don't know how to write the test -> ask user before implementation.

## The Iron Law

```
NO RUNTIME CODE WITHOUT A FAILING TEST FIRST
```

If runtime code is written before a failing test, discard it and restart the cycle.

## Red-Green-Refactor

### RED - Write Failing Test

Write one minimal test for one behavior with a clear name.

Criteria:
- One behavior
- Clear name
- Real code (no mocks unless unavoidable)

### Verify RED - Watch It Fail

Run the smallest relevant automated test command for the behavior under change.

Expected:
- Test fails
- Failure message is expected
- Failure is for missing behavior (not typos/setup)

If test passes, tighten the test.
If test fails for unexpected reasons, fix setup and re-run RED.

### GREEN - Minimal Code

Write the simplest code that makes the failing test pass.

Don't add features, refactor other code, or "improve" beyond the test.

### Verify GREEN - Watch It Pass

Re-run the same targeted test command, then run the broader suite needed to confirm no regressions.

Expected:
- Test passes
- Other tests still pass
- Output is clean (no errors/warnings)

If target test fails, fix runtime code.
If regression tests fail, fix before continuing.

### REFACTOR - Clean Up

After green only:
- Remove duplication
- Improve names
- Extract helpers

Keep tests green. Don't add behavior.

### Repeat

Start the next behavior with a new RED test.

## Constrained Workflow

Use only when routed from `When to Use`:
1. Read code and verify pre-change assumptions.
2. Code change.
3. Build + relevant unit tests + integration checks as applicable.
4. Read code and verify outcome matches goal.

## Test Criteria

**Clear:**
- good: `test('rejects empty email')`
- bad: `test('test1')`

**Minimal:**
- good: one behavior per test name/assertion
- bad: `test('validates email and domain and whitespace')`

**Shows Intent:**
- good: test demonstrates expected API behavior
- bad: test obscures what runtime code should do

**Isolated:**
- good: touch only the target component, wire fixtures
- bad: wire everything in unit test

**AVOID anti-patterns:**
- over-mocking that hides the runtime code of target component
- introducing unnecessary test-only methods to runtime code
- asserting internal state rather than observable behavior
- locking internal signature or structure

## Rationalization Traps

**Red Flags**
- Code before test
- Test after implementation
- Test passes immediately
- Can't explain why RED failed
- Rationalizing "just this once"
- Treating Constrained Workflow as optional shortcut

If any red-flag appears, stop and restart TDD.

## Example: Bug Fix

**Bug:** Empty email accepted

**RED**
Add a test that submits an empty email and expects an `Email required` error.

**Verify RED**
Run focused test command -> expected failure for missing validation.

**GREEN**
Add minimal runtime validation that rejects empty/blank emails with `Email required`.

**Verify GREEN**
Re-run focused test command -> pass, then run broader regression suite.

**REFACTOR**
Extract validation for multiple fields if needed.

## Verification Checklist

Choose checklist by mode selected in `When to Use`; do not mix modes.

### Strict TDD

Before completion:
- [ ] Test written before runtime code
- [ ] RED observed for missing behavior
- [ ] Minimal runtime change made
- [ ] GREEN observed on targeted test
- [ ] Relevant regression suite passed
- [ ] Test avoids anti-patterns

If any item is unchecked, strict TDD is incomplete; restart the cycle.

### Constrained Verification Checklist

Before completion:
- [ ] Constraint source is explicit
- [ ] Pre-change assumptions verified
- [ ] Runtime change completed
- [ ] Build + relevant checks passed
- [ ] Outcome verified against goal

If any item is unchecked, constrained verification is incomplete.
