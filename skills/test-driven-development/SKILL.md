---
name: test-driven-development
description: Use when implementing any feature or bugfix, before writing implementation code
---

# Test-Driven Development (TDD)

## Overview

Write the test first. Watch it fail. Write minimal code to pass.

**Core principle:** If you didn't watch the test fail, you don't know if it tests the right thing.

**Violating the letter of the rules is violating the spirit of the rules.**

## When to Use

High-level rule:
- If a code change is behavioral and unit-testable, TDD is required.
- If there is even a single behavioral change, use strict Red-Green-Refactor.
- Use Constrained Workflow only when strict TDD is blocked and that constraint is explicitly authorized by repository instruction/notes or direct user instruction, or when refactor scope is non-behavioral.

**Required examples + work mapping:**
- New features: required.
- Bug fixes: required.
- Refactors with any behavior/API-contract change: required.
- `phase/workstream` tasks with behavioral changes: required unless user explicitly overrides.
- Standalone behavioral task: required by default.
- Non-behavioral code tasks (mechanical rename/non-behavioral refactor): use Constrained Workflow.
- Non-code changes (docs/chore): not required.

### Identify Test Infrastructure

Before coding, determine which path applies:
- Repository has usable test infrastructure and target area is unit-testable: strict TDD required.
- Target area is integration-dependent or not unit-testable: require explicit repository instruction/notes or direct user instruction before using Constrained Workflow; ask user if instruction is missing/unclear.
- Repository has no test infrastructure: require direct user instruction before using Constrained Workflow.

Thinking "skip TDD just this once"? Stop. That's rationalization.

## The Iron Law

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

Write code before the test? Delete it. Start over.

**No exceptions:**
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it
- Delete means delete

Implement fresh from tests. Period.

## Red-Green-Refactor

### RED - Write Failing Test

Write one minimal test for one behavior with a clear name.

**Requirements:**
- One behavior
- Clear name
- Real code (no mocks unless unavoidable)

### Verify RED - Watch It Fail

**MANDATORY. Never skip.**

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

**MANDATORY.**

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

Order matters because strict TDD proves tests can detect missing behavior before implementation exists. Tests-after usually validate what was built, not what was required.

| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing. |
| "Tests after achieve same goals" | Tests-after = "what does this do?" Tests-first = "what should this do?" |
| "Already manually tested" | Ad-hoc ≠ systematic. No record, can't re-run. |
| "Deleting X hours is wasteful" | Sunk cost fallacy. Keeping unverified code is technical debt. |
| "Keep as reference, write tests first" | You'll adapt it. That's testing after. Delete means delete. |
| "Need to explore first" | Fine. Throw away exploration, start with TDD. |
| "Test hard = design unclear" | Listen to test. Hard to test = hard to use. |
| "TDD will slow me down" | TDD faster than debugging. Pragmatic = test-first. |
| "Manual test faster" | Manual doesn't prove edge cases. You'll re-test every change. |
| "This is integration-only, so TDD never applies" | If behavior is unit-testable, strict TDD is required. Integration-only paths require explicit repo/user instruction to use Constrained Workflow. |
| "No test infrastructure means skip verification" | No. Lack of infra is not self-justification; require user instruction, then run Constrained Workflow with build + relevant checks. |
| "I can infer an infrastructure exception from context" | No. Test-infra constraints must be explicit in repo notes/instructions or confirmed by user. |
| "Refactor means test signatures/implementation shape" | Refactor checks focus on observable behavior (or public API contract), not internal structure. |

**Red Flags**
- Code before test
- Test after implementation
- Test passes immediately
- Can't explain why test failed
- Tests added "later"
- Rationalizing "just this once"
- "I already manually tested it"
- "Tests after achieve the same purpose"
- "It's about spirit not ritual"
- "Keep as reference" or "adapt existing code"
- "Already spent X hours, deleting is wasteful"
- "TDD is dogmatic, I'm being pragmatic"
- "This is different because..."

If strict TDD is required, these mean: delete code and restart with TDD.
If strict TDD is blocked by routing in `When to Use`, run Constrained Workflow instead of skipping verification.

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

Can't check all boxes? You skipped strict TDD. Start over.

### Constrained Workflow

Before marking work complete:
- [ ] Constraint source is explicit (repository instruction/notes or direct user instruction)
- [ ] Pre-change assumptions verified
- [ ] Code change completed
- [ ] Build + relevant unit tests + integration checks passed
- [ ] Post-change outcome verified against goal

Can't check all boxes? Constrained verification is incomplete.

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

When adding mocks or test utilities, avoid these common pitfalls:
- Testing mock behavior instead of real behavior
- Adding test-only methods to production classes
- Mocking without understanding dependencies
- For refactors, asserting implementation shape instead of behavior (reflection checks, static assertions on private types, or "old method no longer exists" checks)
- Writing tests that lock internal signatures/structure unless public API compatibility is the required behavior

## Final Rule

```
Production code → test exists and failed first
Otherwise → not TDD
```

No exceptions without your human partner's permission.
