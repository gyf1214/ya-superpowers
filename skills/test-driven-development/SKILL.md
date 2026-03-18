---
name: test-driven-development
description: Use when implementing any feature or bugfix, before writing implementation code
---

# Test-Driven Development (TDD)

## Overview

Write the test first. Watch it fail. Write minimal code to pass.

**Core principle:** If you didn't watch the test fail, you don't know if it tests the right thing.

## When to Use

Strict TDD applies to ANY testable + behavioral changes.

- Non-behavioral code changes (refactor/renaming): Constrained Workflow.
- Non-testable code: Constrained Workflow REQUIRES special instruction or user approval.
- docs/chore: not required.

### Identify Test Infrastructure

- Test infrastructure exists -> strict TDD required.
- Code requires heavy integration -> check for special instruction; ask user if not clear.
- No test infrastructure -> ask user.

## The Iron Law

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

If code is written before a failing test, discard that implementation and restart with TDD.

Do not keep pre-test implementation code as reference or adapt it while writing tests.

Implement fresh from tests. Period.

## Red-Green-Refactor

### RED - Write Failing Test

Write one minimal test for one behavior with a clear name.

**Requirements:**
- One behavior
- Clear name
- Real code (no mocks unless unavoidable)

### Verify RED - Watch It Fail

Required.

Run the smallest relevant automated test command for the behavior under change.

Confirm:
- Test fails (not errors)
- Failure message is expected
- Fails because feature missing (not typos)

**Test passes?** You're testing existing behavior. Fix test.

**Test errors?** Fix error, re-run until it fails correctly.

### GREEN - Minimal Code

Write the simplest code that makes the failing test pass.

Don't add features, refactor other code, or "improve" beyond the test.

### Verify GREEN - Watch It Pass

Required.

Re-run the same targeted test command, then run the broader suite needed to confirm no regressions.

Confirm:
- Test passes
- Other tests still pass
- Output pristine (no errors, warnings)

**Test fails?** Fix code, not test.

**Other tests fail?** Fix now.

### REFACTOR - Clean Up

After green only:
- Remove duplication
- Improve names
- Extract helpers

Keep tests green. Don't add behavior.

### Repeat

Next failing test for next feature.

## Constrained Workflow

Use only when routed from `When to Use`:
1. Read code and verify pre-change assumptions.
2. Code change.
3. Build + relevant unit tests + integration checks as applicable.
4. Read code and verify outcome matches goal.

## Good Tests

| Quality | Good | Bad |
|---------|------|-----|
| **Minimal** | One thing. "and" in name? Split it. | `test('validates email and domain and whitespace')` |
| **Clear** | Name describes behavior | `test('test1')` |
| **Shows intent** | Demonstrates desired API | Obscures what code should do |

## Rationalization Traps

When any red flag appears, reset and apply strict TDD.

**Red Flags**
- Code before test
- Test after implementation
- Test passes immediately
- Can't explain why RED failed
- Rationalizing "just this once"
- Treating Constrained Workflow as optional shortcut

If any red flag appears, follow routing in `When to Use` (strict TDD by default).

## Example: Bug Fix

**Bug:** Empty email accepted

**RED**
```typescript
test('rejects empty email', async () => {
  const result = await submitForm({ email: '' });
  expect(result.error).toBe('Email required');
});
```

**Verify RED**
Run focused test command -> expected failure for missing validation.

**GREEN**
```typescript
function submitForm(data: FormData) {
  if (!data.email?.trim()) {
    return { error: 'Email required' };
  }
  // ...
}
```

**Verify GREEN**
Re-run focused test command -> pass, then run broader regression suite.

**REFACTOR**
Extract validation for multiple fields if needed.

## Verification Checklist

Choose checklist by mode selected in `When to Use`; do not mix modes.

### Strict TDD

Before marking work complete:
- [ ] Failing test written before production code
- [ ] RED failure observed for expected reason (missing behavior, not typo/setup error)
- [ ] Minimal code change made to satisfy the failing test
- [ ] GREEN pass observed on the targeted test
- [ ] Relevant regression suite passed
- [ ] Tests avoid non-behavioral shape/signature assertions unless public API contract requires them

If any item is unchecked, strict TDD is incomplete; restart the cycle.

### Constrained Verification Checklist

Before marking work complete:
- [ ] Constraint source is explicit (repository instruction/notes or direct user instruction)
- [ ] Pre-change assumptions verified
- [ ] Code change completed
- [ ] Build + relevant unit tests + integration checks passed
- [ ] Post-change outcome verified against goal

If any item is unchecked, constrained verification is incomplete.

## When Stuck

| Problem | Solution |
|---------|----------|
| Don't know how to test | Write wished-for API. Write assertion first. Ask your human partner. |
| Test too complicated | Design too complicated. Simplify interface. |
| Must mock everything | Code too coupled. Use dependency injection. |
| Test setup huge | Extract helpers. Still complex? Simplify design. |

## Debugging Integration

Bug found? Write failing test reproducing it. Follow TDD cycle. Test proves fix and prevents regression.

Never fix bugs without a test.

## Testing Anti-Patterns

When adding mocks or test utilities, avoid:
- Testing mock behavior instead of real behavior
- Adding test-only methods to production classes
- Mocking without understanding dependencies
- For refactors, asserting implementation shape instead of behavior
- Writing tests that lock internal signatures/structure unless public API compatibility is the required behavior

Exceptions require explicit user permission.
