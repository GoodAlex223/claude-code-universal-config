# Manual Testing Scenarios Policy

Standards for creating and maintaining manual testing scenarios after code changes.

---

## Overview

**Manual testing scenarios are a MANDATORY phase of task execution, performed AFTER code implementation.**

This phase ensures that:
1. Code changes can be validated through reproducible steps
2. Regression tests capture fixed bugs for future reference
3. Automated tests are created to simulate manual scenarios

---

## When to Create Manual Testing Scenarios

Manual testing scenarios MUST be created when ANY of these apply:

| Trigger | Action Required |
|---------|-----------------|
| Bug fix | Add script to reproduce original bug + verify fix |
| New feature | Add scenarios covering feature's user flows |
| Behavior change | Update/delete contradicting scenarios + add new ones |
| UI/UX change | Add scenarios for user interaction paths |
| Code deletion | Remove scenarios that test deleted functionality |

**Exception**: Pure refactoring with no behavior change may skip this phase (document why in plan).

---

## Timing: AFTER Implementation, NOT During Planning

```
PLAN → EXECUTE → VERIFY → **MANUAL TESTING PHASE** → DOCUMENT → COMPLETE
```

### Why After Implementation?

1. **During planning**: You don't know exact behavior yet
2. **During implementation**: Focus on code, not scenarios
3. **After implementation**: You understand actual behavior and edge cases discovered

**DO NOT define test scenarios during planning.** Wait until code is working.

---

## Manual Testing Phase Workflow

### Step 1: Review Existing Scenarios

**Before creating new scenarios, read `docs/MANUAL_TESTING.md`:**

```bash
# Always review existing scenarios first
cat docs/MANUAL_TESTING.md
```

| Question | Action |
|----------|--------|
| Do any scenarios test removed/changed code? | Delete or modify them |
| Do any scenarios contradict new behavior? | Update expected results |
| Are there similar scenarios to extend? | Add variants instead of duplicating |

### Step 2: Delete/Modify Contradicting Scenarios

**Mark scenarios as fixed or update them:**

```markdown
### Script [N]: [Description]

**Added**: YYYY-MM-DD
**Corrected**: YYYY-MM-DD  ← Update this
**Status**: [x] Fixed      ← Mark as fixed if bug resolved

**Fix Reference**: [commit hash or PR link]
```

### Step 3: Create New Scenarios

**Add scenarios that verify the changes work correctly:**

```markdown
### Script [N]: [Brief Description]

**Added**: YYYY-MM-DD
**Corrected**: YYYY-MM-DD (or "N/A" if new feature)
**Status**: [ ] Open / [x] Fixed

**Steps**:
1. [Specific action]
2. [Specific action]
...

**Expected**: [What should happen]
**Actual**: [What happens - describe bug if exists]
**Fix Reference**: [Link to fix commit/PR]

**Rationale**: [Why this scenario matters - what it validates]
```

### Step 4: Create Automated Tests for Scenarios

**MANDATORY: Convert manual scenarios to automated tests where feasible.**

For each manual scenario, consider:

| Manual Scenario Type | Automated Test Approach |
|---------------------|-------------------------|
| Data transformation | Unit test with same inputs/outputs |
| State transitions | Integration test simulating flow |
| Error handling | Test with error injection |
| User input validation | Parameterized tests with edge cases |
| Multi-step flows | End-to-end test or integration test |

**Naming convention for scenario-based tests:**

```python
def test_scenario_N_description():
    """
    Automated version of Manual Script N.

    Tests that [behavior] works correctly after [change].
    Reference: docs/MANUAL_TESTING.md Script N
    """
```

---

## Scenario Format Standards

### Bug Reproduction Scripts

For bugs being fixed:

```markdown
### Script [N]: [Bug Summary]

**Added**: YYYY-MM-DD (when bug discovered)
**Corrected**: YYYY-MM-DD (when fix applied)
**Status**: [x] Fixed

**Steps**:
1. [Exact reproduction steps]
2. [Continue with specifics]
...

**Expected**: [Correct behavior]
**Actual**: [Bug behavior that occurred]
**Fix Reference**: [commit hash or task reference]
```

### Feature Validation Scenarios

For new features:

```markdown
### Script [N]: [Feature - Specific Flow]

**Added**: YYYY-MM-DD
**Status**: [ ] Pending Verification

**Preconditions**:
- [Required state before testing]

**Steps**:
1. [Action]
2. [Action]
...

**Expected**: [Correct behavior]
**Edge Cases Tested**:
- [Edge case 1]
- [Edge case 2]
```

### Regression Prevention Scenarios

For preventing bug recurrence:

```markdown
### Script [N]: [Regression - Original Bug Summary]

**Added**: YYYY-MM-DD
**Original Bug Date**: YYYY-MM-DD
**Status**: [x] Regression Test

**Steps**:
1. [Steps that triggered original bug]
...

**Expected**: [Now works correctly]
**Was**: [Original buggy behavior]
**Prevention**: [What fix prevents recurrence]
```

---

## Project File Location

### Standard Location

```
docs/MANUAL_TESTING.md
```

### Required Sections in MANUAL_TESTING.md

```markdown
# Manual Testing - Bug Reproduction Scripts

**Created**: YYYY-MM-DD
**Updated**: YYYY-MM-DD
**Purpose**: Track scripts that verify correct behavior with dates added and corrected

---

## Script Format

[Template for new scripts]

---

## Bug Reproduction Scripts

[Active bug reproduction scripts]

---

## Test Scenarios

[Feature validation scenarios]

---

## Test Environment Setup

[Environment-specific instructions]
```

---

## Integration with Workflow

### In Plan Document

Add to Section 3 (Implementation Log):

```markdown
### [YYYY-MM-DD HH:MM] — PHASE: Manual Testing Scenarios

**Scenarios Reviewed**: [count in MANUAL_TESTING.md]
**Scenarios Modified**: [list with reasons]
**Scenarios Deleted**: [list with reasons]
**Scenarios Added**: [list with descriptions]
**Automated Tests Created**: [list test files/functions]
```

### In Task Completion Checklist

- [ ] `docs/MANUAL_TESTING.md` reviewed
- [ ] Contradicting scenarios updated/deleted
- [ ] New scenarios added for changes
- [ ] Automated tests created for scenarios
- [ ] Scenarios follow format standards

---

## Quality Criteria

### Good Scenario Characteristics

| Criterion | Description |
|-----------|-------------|
| **Reproducible** | Anyone can follow steps and get same result |
| **Specific** | Exact actions, not vague descriptions |
| **Minimal** | Fewest steps to demonstrate behavior |
| **Dated** | Always has Added/Corrected dates |
| **Referenced** | Links to fix commit or task |

### Bad Scenario Examples

| Problem | Example | Fix |
|---------|---------|-----|
| Vague steps | "Start a session" | "Send `/start` command" |
| Missing dates | No Added/Corrected | Always include dates |
| No expected result | Just lists steps | Add Expected/Actual |
| Duplicate | Same as Script 3 | Delete or merge |

---

## Memory Integration

After updating manual testing scenarios:

```
# Create/update memory for significant scenarios
create_entities([{
  "name": "project:[name]:test-scenario:[brief-id]",
  "entityType": "TestScenario",
  "observations": [
    "[YYYY-MM-DD] Added scenario for [description]",
    "Tests: [what it validates]",
    "Automated in: tests/[file].py::[test_name]"
  ]
}])
```

---

## Examples

### Example 1: Bug Fix Scenario

Code change: Fixed race condition in rating handler

```markdown
### Script 15: Race Condition - Rapid Rating Causes Wrong Post Rating

**Added**: 2026-01-14
**Corrected**: 2026-01-14
**Status**: [x] Fixed

**Steps**:
1. Start from text with any tag
2. Press Start
3. When post appears, rapidly click rating button 3+ times

**Expected**: Only first rating accepted, subsequent clicks ignored
**Actual (before fix)**: Multiple posts shown, rating applied to wrong post
**Fix Reference**: Task 2.2.1 - Added rate limiting per user

**Automated Test**: tests/integration/test_rating_handler.py::test_rapid_rating_only_accepts_first
```

### Example 2: Feature Validation Scenario

Code change: Added session rating before data save

```markdown
### Script 16: Rating Prompt Before Data Save

**Added**: 2026-01-14
**Status**: [x] Fixed

**Steps**:
1. Start session with any tag
2. Like/skip a few posts
3. Send /stop
4. **OBSERVE**: Rating prompt should appear IMMEDIATELY

**Expected**: Rating prompt appears before DB save completes
**Verification**: Check DB - posts_viewed=0 until rating sent
**Fix Reference**: Task 2.3.1 - Deferred save pattern

**Automated Test**: tests/integration/test_session_flow.py::test_rating_prompt_before_save
```

---

## Checklist for Manual Testing Phase

After completing code implementation:

- [ ] Read entire `docs/MANUAL_TESTING.md`
- [ ] Identify scenarios affected by changes
- [ ] Delete scenarios for removed functionality
- [ ] Update scenarios with changed expected behavior
- [ ] Add new scenarios for:
  - [ ] Bug fixes (reproduction + verification)
  - [ ] New features (user flows)
  - [ ] Changed behavior (before/after)
- [ ] Create automated tests for each new scenario
- [ ] Update scenario dates and fix references
- [ ] Verify scenario format follows standards
- [ ] Update memory if significant scenarios added

---

*See [testing.md](testing.md) for general testing policy.*
*See [../WORKFLOW.md](../WORKFLOW.md) for full development workflow.*
