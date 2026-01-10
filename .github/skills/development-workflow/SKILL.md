---
name: development-workflow
description: Plan-Execute-Verify-Document cycle for structured task completion with phase management. Use when implementing features, fixing bugs, or any multi-step development task that needs organized execution.
---

# Development Workflow

Every task follows this cycle:

```
PLAN → EXECUTE → VERIFY → DOCUMENT → COMPLETE
```

Each phase has mandatory checkpoints that must be satisfied before proceeding.

---

## Phase 1: PLAN

### Create Plan Document

Before starting any significant task, create a plan with:

```markdown
# [Task Name] - Implementation Plan

**Status**: Planning | In Progress | Complete
**Created**: YYYY-MM-DD

## 1. Task Overview
**Goal**: [Clear objective]
**Context**: [Why this matters]
**Success Criteria**:
- [ ] [Criterion 1]
- [ ] [Criterion 2]

## 2. Analysis
**Problem Restatement**: [In your own words]
**Approaches Considered**: [At least 3]
**Selected Approach**: [With reasoning]
**Edge Cases**: [At least 5]
**Risks**: [Identified risks]

## 3. Implementation Steps
1. [ ] [Step 1]
2. [ ] [Step 2]
3. [ ] [Step 3]
```

### Analysis Requirements

Before implementation:
- [ ] Problem restated in own words
- [ ] 3+ alternative approaches considered
- [ ] 5+ edge cases identified
- [ ] Existing similar code searched
- [ ] Trade-offs documented

---

## Phase 2: EXECUTE

### Test-Driven Development (TDD)

Mandatory sequence:
1. **Write tests first** — Cover expected behavior
2. **Run tests** — Verify they fail
3. **Implement code** — Make tests pass
4. **Refactor** — Clean up while tests stay green
5. **Add edge case tests** — Cover discovered cases

### Implementation Checkpoints

**Every 50 lines of code:**
- [ ] Still aligned with plan?
- [ ] Deviations documented?
- [ ] Partial tests runnable?

**Before any file save:**
- [ ] Addresses actual requirement?
- [ ] No obvious bugs?
- [ ] Types correct?

### Stuck Protocol

If stuck after 3 attempts:
1. **STOP** — Do not continue blindly
2. **Document** — What was tried, what failed
3. **Ask** — Request user clarification
4. **Wait** — Do not proceed without guidance

---

## Phase 3: VERIFY

### Automated Verification

Before any commit:
- [ ] Pre-commit hooks pass
- [ ] Test suite passes
- [ ] Type checking passes
- [ ] Linting passes

### Manual Testing

Create testing checklist:

```markdown
# Manual Testing Checklist

**Feature**: [Name]
**Date**: YYYY-MM-DD

## Functional Tests
- [ ] **Test 1**: [Description]
  - Steps: [Instructions]
  - Expected: [Result]

## Edge Cases
- [ ] **Edge 1**: [Description]

## Regression Tests
- [ ] [Existing feature still works]

## Sign-off
- [ ] All tests passed
- [ ] Ready to push
```

### User Approval

**Mandatory before push:**
1. Notify user that changes are ready
2. Provide testing checklist
3. Wait for explicit approval

---

## Phase 4: DOCUMENT

### Required Updates

| Item | Action |
|------|--------|
| TODO.md | Remove completed task |
| DONE.md | Add task with details |
| Plan document | Mark complete, add discoveries |
| PROJECT_CONTEXT.md | Add architectural decisions |

### Key Discoveries

Document in plan:
- Technical insights
- Architectural decisions made
- Patterns identified
- Anti-patterns avoided

### Future Improvements (Mandatory)

**Minimum 2 items:**
- Enhancement ideas
- Technical debt identified
- Performance optimizations
- Spawned tasks for TODO.md

---

## Phase 5: COMPLETE

### Final Checklist

- [ ] All tests passing
- [ ] Pre-commit hooks passing
- [ ] User approved manual testing
- [ ] TODO.md updated
- [ ] DONE.md updated
- [ ] Plan document completed
- [ ] Code pushed

---

## Task Decomposition

### When to Divide into Phases

A task requires phase division when:
- Affects 5+ files
- Requires 3+ distinct steps
- Involves multiple subsystems
- Exceeds 100 lines of changes

### Phase Execution Protocol

1. **Divide** into logical phases (each testable)
2. **Execute** one phase at a time
3. **Pause and report** after each phase:
   ```markdown
   ### Phase [N] Complete: [Name]

   **Accomplished**: [What was done]
   **Files Modified**: [List]
   **Tests Status**: [Passing/Failing]
   **Discoveries**: [Any findings]

   **Ready for Phase [N+1]**: [Brief description]

   Awaiting confirmation to proceed...
   ```
4. **Wait** for developer confirmation

---

## Abort Conditions

**Stop and consult user when:**
- Task scope exceeds estimate by 2x
- Requirement conflicts with architecture
- 3+ test failures after 3 fix attempts
- Uncertainty about security implications
- Found potential data loss scenario
- Ambiguity that could lead to wrong implementation
