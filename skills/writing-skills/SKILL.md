---
name: writing-skills
description: Use when creating new skills, editing existing skills, or reviewing skill changes before deployment
---

# Writing Skills

## Overview

Use evidence-first validation when writing or editing skill instructions: capture baseline behavior, make targeted skill changes, and verify improved outcomes.

**Core principle:** If you cannot show a clear before/after behavior change, you do not know whether the skill works.

**Required background:** Understand practical validation and evidence collection for behavior change.

## When to Use

- When working on an agent skill repository.
- When creating a skill instruction.
- When editing a skill instruction.

Suggest user to create a new skill:
- for reusable workflows, recurring failure patterns, or durable decision rules
- not for one-off fixes, repository-only conventions, or rules better enforced by automation

## Validation Law

```
NO SKILL DEPLOYMENT WITHOUT VALIDATION EVIDENCE
```

## Evidence-Based Verification Workflow

**Step 1: Define Goal**
- State the real user goal and expected outcome in concrete terms.
- Anchor it to a realistic user-experience scenario.

**Step 2: Capture Baseline Behavior**
- Predict or replay what an agent does today with current skill coverage.
- Verify whether behavior meets the goal and record the gap.
- Do not edit the skill until the gap is explicit.

**Step 3: Write/Update Skill**
- Make the minimal skill changes that directly target the observed gap.

**Step 4: Predict Post-Change Behavior**
- State what an agent should do after the skill change.

**Step 5: Verify Gap Is Bridged**
- Re-run/replay the same scenario and confirm the outcome now meets the goal.

## Skill Creation Checklist

Finish validation for the current skill before starting another.

**Design + Metadata:**
- [ ] Name uses only letters, numbers, hyphens (no parentheses/special chars)
- [ ] YAML frontmatter with only name and description (max 1024 chars)
- [ ] Description starts with "Use when..." and includes specific triggers/symptoms
- [ ] Description written in third person
- [ ] Keywords throughout for search (errors, symptoms, tools)
- [ ] Clear overview with core principle
- [ ] Code inline OR link to separate file
- [ ] One excellent example (not multi-language)

**Evidence + Validation:**
- [ ] Goal is explicit for the scenario under test
- [ ] Baseline behavior captured and compared against goal
- [ ] Behavior gap is explicit before edits
- [ ] Skill changes directly target that gap
- [ ] Post-change behavior verified on the same scenario
- [ ] Gap is bridged and user outcome is improved

**Quality Checks:**
- [ ] Decision guidance included only if decision is non-obvious
- [ ] Quick reference table
- [ ] Common mistakes section
- [ ] No narrative storytelling
- [ ] Supporting files only for tools or heavy reference

**Deployment:**
- [ ] Commit skill changes if working in a skill repository

## Validation Mapping for Skills

| Validation Concept | Skill Creation |
|--------------------|----------------|
| **Baseline** | Observe behavior before changes (agent run or transcript replay) |
| **Change** | Edit skill document (`SKILL.md`) |
| **Post-change check** | Re-run scenario and verify behavior improved |
| **Refactor** | Close loopholes while keeping validated behavior |
| **Evidence** | Record concrete failures/rationalizations and post-change outcomes |

Use baseline/post-change validation for every skill change.

## Skill Types

### Technique
Concrete method with steps to follow (condition-based-waiting, root-cause-tracing)

### Pattern
Way of thinking about problems (flatten-with-flags, test-invariants)

### Reference
API docs, syntax guides, tool documentation (office docs)

## Directory Structure


```
skills/
  skill-name/
    SKILL.md              # Main reference (required)
    supporting-file.*     # Only if needed
```

**Flat namespace** - all skills in one searchable namespace

**Separate files for:**
1. **Heavy reference** (100+ lines) - API docs, comprehensive syntax
2. **Reusable tools** - Scripts, utilities, templates

**Keep inline:**
- Principles and concepts
- Code patterns (< 50 lines)
- Everything else

## SKILL.md Structure

**Frontmatter (YAML):**
- Only two fields supported: `name` and `description`
- Max 1024 characters total
- `name`: Use letters, numbers, and hyphens only (no parentheses, special chars)
- `description`: Third-person, describes ONLY when to use (NOT what it does)
  - Start with "Use when..." to focus on triggering conditions
  - Include specific symptoms, situations, and contexts
  - **NEVER summarize the skill's process or workflow** (see CSO section for why)
  - Keep under 500 characters if possible

```markdown
---
name: Skill-Name-With-Hyphens
description: Use when [specific triggering conditions and symptoms]
---

# Skill Name

## Overview
What is this? Core principle in 1-2 sentences.

## When to Use
[Decision guidance IF decision non-obvious]

Bullet list with SYMPTOMS and use cases
When NOT to use

## Core Pattern (for techniques/patterns)
Before/after code comparison

## Quick Reference
Table or bullets for scanning common operations

## Implementation
Inline code for simple patterns
Link to file for heavy reference or reusable tools

## Common Mistakes
What goes wrong + fixes

## Real-World Impact (optional)
Concrete results
```


## Codex Search Optimization (CSO)

**Critical for discovery:** Future Codex needs to FIND your skill

### 1. Rich Description Field

**Purpose:** Codex reads description to decide which skills to load for a given task. Make it answer: "Should I read this skill right now?"

**Format:** Start with "Use when..." to focus on triggering conditions

**Description = When to Use, NOT What the Skill Does**

The description should ONLY describe triggering conditions. Do NOT summarize the skill's process or workflow in the description.

**Why this matters:** Testing revealed that when a description summarizes the skill's workflow, Codex may follow the description instead of reading the full skill content. A description saying "code review between tasks" caused Codex to do ONE review, even though the skill itself required TWO reviews (spec compliance then code quality).

When the description was changed to just "Use when executing implementation plans with independent tasks" (no workflow summary), Codex correctly read the full skill and followed the two-stage review process.

Descriptions that summarize workflow can become a shortcut. Keep process details in the skill body.

```yaml
# ❌ BAD: Summarizes workflow - Codex may follow this instead of reading skill
description: Use when executing plans and coordinating independent implementation tasks with explicit review checkpoints

# ❌ BAD: Too much process detail
description: Use for TDD - write test first, watch it fail, write minimal code, refactor

# ✅ GOOD: Just triggering conditions, no workflow summary
description: Use when executing implementation plans with independent tasks in the current session

# ✅ GOOD: Triggering conditions only
description: Use when implementing any feature or bugfix, before writing implementation code
```

**Content:**
- Use concrete triggers, symptoms, and situations that signal this skill applies
- Describe the *problem* (race conditions, inconsistent behavior) not *language-specific symptoms* (setTimeout, sleep)
- Keep triggers technology-agnostic unless the skill itself is technology-specific
- If skill is technology-specific, make that explicit in the trigger
- Write in third person (injected into system prompt)
- **NEVER summarize the skill's process or workflow**

### 2. Keyword Coverage

Use words Codex would search for:
- Error messages: "Hook timed out", "ENOTEMPTY", "race condition"
- Symptoms: "flaky", "hanging", "zombie", "pollution"
- Synonyms: "timeout/hang/freeze", "cleanup/teardown/afterEach"
- Tools: Actual commands, library names, file types

### 3. Descriptive Naming

**Use active voice, verb-first:**
- ✅ `creating-skills` not `skill-creation`
- ✅ `condition-based-waiting` not `async-test-helpers`

### 4. Token Efficiency (Critical)

**Problem:** getting-started and frequently-referenced skills load into EVERY conversation. Every token counts.

**Target word counts:**
- Getting-started skill: <500 words
- Frequently loaded skill: <750 words
- Other skills: <2000 words

**Techniques:**

**Move details to tool help:**
```bash
# ❌ BAD: Document all flags in SKILL.md
search-conversations supports --text, --both, --after DATE, --before DATE, --limit N

# ✅ GOOD: Reference --help
search-conversations supports multiple modes and filters. Run --help for details.
```

**Use cross-references:**
```markdown
# ❌ BAD: Repeat workflow details
When searching, run a focused search pass with a concise template...
[20 lines of repeated instructions]

# ✅ GOOD: Reference other skill
Use focused execution to reduce context sprawl. REQUIRED: Use [other-skill-name] for workflow.
```

**Compress examples:**
```markdown
# ❌ BAD: Verbose example (42 words)
your human partner: "How did we handle authentication errors in React Router before?"
You: I'll search past conversations for React Router authentication patterns.
[Run focused search: "React Router authentication error handling 401"]

# ✅ GOOD: Minimal example (20 words)
Partner: "How did we handle auth errors in React Router?"
You: Searching...
[Search → synthesis]
```

**Eliminate redundancy:**
- Don't repeat what's in cross-referenced skills
- Don't explain what's obvious from command
- Don't include multiple examples of same pattern

**Verification:**
```bash
wc -w skills/path/SKILL.md
# getting-started skill: aim for <500
# frequently loaded skill: aim for <750
# other skills: aim for <2000
```

### 5. Cross-Referencing Other Skills

**When writing documentation that references other skills:**

Keep skills self-contained by default. Reference another skill only when it is:
- A dependency
- An invocation target
- A workflow transition target

Use skill name only. Use requirement markers only when the reference is truly mandatory:
- ✅ Good: `**REQUIRED SUB-SKILL:** Use test-driven-development`
- ✅ Good: `**REQUIRED BACKGROUND:** You MUST understand systematic-debugging`
- ❌ Bad: `See skills/testing/test-driven-development` (unclear if required)
- ❌ Bad: `@skills/testing/test-driven-development/SKILL.md` (force-loads, burns context)

**Why no @ links:** `@` syntax force-loads files immediately, consuming 200k+ context before you need them.

## File Organization

### Self-Contained Skill
```
defense-in-depth/
  SKILL.md    # Everything inline
```
When: All content fits, no heavy reference needed

### Skill with Reusable Tool
```
condition-based-waiting/
  SKILL.md    # Overview + patterns
  example.ts  # Working helpers to adapt
```
When: Tool is reusable code, not just narrative

### Skill with Heavy Reference
```
pptx/
  SKILL.md       # Overview + workflows
  pptxgenjs.md   # 600 lines API reference
  ooxml.md       # 500 lines XML structure
  scripts/       # Executable tools
```
When: Reference material too large for inline

## Anti-Patterns

### ❌ Narrative Example
"In session 2025-10-03, we found empty projectDir caused..."
**Why bad:** Too specific, not reusable

### ❌ Multi-Language Dilution
example-js.js, example-py.py, example-go.go
**Why bad:** Mediocre quality, maintenance burden

### ❌ Generic Labels
helper1, helper2, step3, pattern4
**Why bad:** Labels should have semantic meaning
