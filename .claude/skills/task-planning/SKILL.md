---
name: task-planning
description: Structured task planning templates with analysis, implementation steps, and progress logging. Use when starting any significant coding task to ensure thorough planning and tracking.
---

# Task Planning

Use this template for any non-trivial task to ensure thorough planning and execution tracking.

---

## Task Plan Template

```markdown
# [Task Name] - Implementation Plan

**Task Reference**: [TODO.md Section or Issue #]
**Created**: YYYY-MM-DD
**Status**: Planning | In Progress | Blocked | Complete
**Last Updated**: YYYY-MM-DD

---

## 1. Task Overview

**Goal**:
[REQUIRED: What are we trying to achieve?]

**Context**:
[REQUIRED: Why does this matter?]

**Success Criteria**:
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

---

## 2. Analysis

### Problem Restatement
[REQUIRED: Problem in your own words]

### Approaches Considered

| Approach | Pros | Cons |
|----------|------|------|
| [Approach A] | [Benefits] | [Drawbacks] |
| [Approach B] | [Benefits] | [Drawbacks] |
| [Approach C] | [Benefits] | [Drawbacks] |

### Selected Approach
[REQUIRED: Selected approach with reasoning]

### Assumptions
- [Assumption 1]
- [Assumption 2]

### Edge Cases

| Edge Case | Handling |
|-----------|----------|
| [Case 1] | [How handled] |
| [Case 2] | [How handled] |
| [Case 3] | [How handled] |
| [Case 4] | [How handled] |
| [Case 5] | [How handled] |

### Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| [Risk 1] | High/Med/Low | [How to mitigate] |
| [Risk 2] | High/Med/Low | [How to mitigate] |

---

## 3. Implementation Plan

### Files Affected

| File | Action | Purpose |
|------|--------|---------|
| [path/file.py] | Create/Modify | [What changes] |

### Dependencies
- [Dependency 1]
- [Dependency 2]

### Implementation Steps
1. [ ] [Step 1]
2. [ ] [Step 2]
3. [ ] [Step 3]

### Phases (for large tasks)

#### Phase 1: [Name]
- [ ] [Task 1.1]
- [ ] [Task 1.2]

#### Phase 2: [Name]
- [ ] [Task 2.1]
- [ ] [Task 2.2]

---

## 4. Implementation Log

### [YYYY-MM-DD HH:MM] — Planning
- Goal understood: [summary]
- Approach chosen: [brief]
- Risks identified: [list]

### [YYYY-MM-DD HH:MM] — Implementation
- Step completed: [what]
- Deviation from plan: [yes/no, why]
- Unexpected discovery: [if any]

### [YYYY-MM-DD HH:MM] — Sub-Item Complete
- Sub-item: [what was finished]
- **Results obtained**: [achievements]
- **Lessons learned**: [insights]
- **Problems encountered**: [issues and resolutions]
- **Improvements identified**: [list]
- **Technical debt noted**: [if any]

### [YYYY-MM-DD HH:MM] — Complete
- Final approach: [summary]
- Tests passing: [yes/no]
- Documentation updated: [list]

---

## 5. Key Discoveries

**Technical Insights**:
- [Insight]: [Explanation]

**Architectural Decisions**:
- [Decision]: [Rationale]

**Patterns Identified**:
- [Pattern]: [When to use]

---

## 6. Future Improvements

### Enhancement Ideas (minimum 2)

| Idea | Rationale | Effort | Priority |
|------|-----------|--------|----------|
| [Idea 1] | [Why] | H/M/L | H/M/L |
| [Idea 2] | [Why] | H/M/L | H/M/L |

### Technical Debt

| Item | Why It Exists | Impact | Remediation |
|------|---------------|--------|-------------|
| [Item] | [Reason] | H/M/L | [Approach] |

### Spawned Tasks

| Task | Origin | Priority | Added to TODO |
|------|--------|----------|---------------|
| [Task] | [Step/Phase] | H/M/L | [ ] Yes |

---

## 7. Testing

### Test Plan
- [ ] Unit tests for [component]
- [ ] Integration tests for [flow]
- [ ] Manual testing for [scenario]

### Test Results

| Test | Result | Notes |
|------|--------|-------|
| [Test 1] | Pass/Fail | [Notes] |

---

## 8. Review

### Self-Review Checklist
- [ ] Code follows project conventions
- [ ] Tests written and passing
- [ ] Documentation updated
- [ ] No security concerns
- [ ] Performance considered

### Approval
- [ ] Self-review complete
- [ ] User approved
```

---

## Quick Planning Checklist

For simpler tasks, use this abbreviated checklist:

- [ ] **Goal**: What are we achieving?
- [ ] **Approach**: Which of 3 options is best?
- [ ] **Edge Cases**: At least 5 identified
- [ ] **Steps**: Implementation order defined
- [ ] **Tests**: What tests will verify success?
- [ ] **Risks**: What could go wrong?
