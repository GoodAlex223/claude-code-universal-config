---
name: documentation-templates
description: ADR, PR, bug report, feature request, and incident report templates. Use when creating architecture decision records, pull requests, issues, or any structured documentation.
---

# Documentation Templates

Ready-to-use templates for common documentation needs.

---

## Architecture Decision Record (ADR)

Use for significant technical decisions.

```markdown
# ADR-[NUMBER]: [Title]

**Status**: Proposed | Accepted | Deprecated | Superseded
**Date**: YYYY-MM-DD
**Decision Makers**: [Names]

---

## Context

[What is the issue motivating this decision?]

### Constraints
- [Constraint 1]
- [Constraint 2]

### Requirements
- [Requirement 1]
- [Requirement 2]

---

## Decision Drivers

- **[Driver 1]**: [Explanation]
- **[Driver 2]**: [Explanation]

---

## Considered Options

### Option 1: [Name]
**Description**: [What this option entails]
**Pros**: [Benefits]
**Cons**: [Drawbacks]
**Effort**: Small/Medium/Large

### Option 2: [Name]
**Description**: [What this option entails]
**Pros**: [Benefits]
**Cons**: [Drawbacks]
**Effort**: Small/Medium/Large

---

## Decision

[The decision made]

### Rationale
[Why this option was chosen]

---

## Consequences

### Positive
- [What becomes easier]

### Negative
- [What becomes harder]

---

## Implementation

### Action Items
- [ ] [Action 1]
- [ ] [Action 2]

### Rollback Plan
1. [How to undo if needed]

---

## Follow-up

**Review Date**: YYYY-MM-DD
**Success Metrics**: [How we know it worked]
```

---

## Pull Request Template

```markdown
## Summary

[What does this PR accomplish?]

## Type of Change

- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update
- [ ] Refactoring

## Changes Made

- [Change 1]
- [Change 2]

## Related Issues

- Closes #[issue]
- Task: [TODO.md reference]
- Plan: [link to plan]

## Testing

### Automated Tests
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] All tests passing

### Manual Testing
1. [Step 1]
2. [Expected result]

## Checklist

### Code Quality
- [ ] Code follows project style
- [ ] Self-review completed
- [ ] No debugging code left in

### Testing
- [ ] Tests cover new functionality
- [ ] Edge cases covered

### Documentation
- [ ] Code documentation updated
- [ ] README updated if needed

### Security
- [ ] No secrets in code
- [ ] Input validation present

## Breaking Changes

[If breaking, describe migration path]

## Deployment Notes

- [ ] Database migration required
- [ ] Environment variables needed
- [ ] Rollback plan documented
```

---

## Bug Report Template

```markdown
## Bug Report

### Summary
[One-line description]

### Severity
- [ ] Critical - System unusable
- [ ] High - Major feature broken
- [ ] Medium - Workaround exists
- [ ] Low - Minor/cosmetic

### Environment

| Item | Value |
|------|-------|
| Version | [e.g., 1.2.3] |
| OS | [e.g., Ubuntu 22.04] |
| Browser | [if applicable] |

### Steps to Reproduce

1. [First step]
2. [Second step]
3. [See error]

### Expected Behavior
[What should happen]

### Actual Behavior
[What actually happens]

### Error Messages

```
[Paste errors/logs here]
```

### Screenshots
[Attach if applicable]

### Possible Cause
[Your hypothesis if any]

### Workaround
[How to work around it]

### Additional Context
- First noticed: [date]
- Frequency: [always/sometimes/rarely]
- Related: #[issue]
```

---

## Feature Request Template

```markdown
## Feature Request

### Summary
[One-line description]

### Problem Statement
[What problem does this solve?]

### Proposed Solution
[How should this work?]

### User Story

```
As a [type of user],
I want [goal],
So that [benefit].
```

### Acceptance Criteria

- [ ] [Criterion 1]
- [ ] [Criterion 2]

### Priority

- [ ] Critical - Blocking
- [ ] High - Significant improvement
- [ ] Medium - Nice to have
- [ ] Low - Minor

### Use Cases

#### Use Case 1
- **Actor**: [Who]
- **Scenario**: [What]
- **Outcome**: [Result]

### Alternatives Considered

| Alternative | Pros | Cons |
|-------------|------|------|
| [Alt 1] | [Benefits] | [Drawbacks] |

### Dependencies
- [ ] [Dependency 1]

### Estimated Effort
- [ ] Small (< 1 day)
- [ ] Medium (1-3 days)
- [ ] Large (1-2 weeks)
- [ ] XL (> 2 weeks)
```

---

## Incident Report Template

```markdown
## Incident Report

### Summary
[Brief description of what happened]

### Severity
- [ ] SEV1 - Critical outage
- [ ] SEV2 - Major degradation
- [ ] SEV3 - Minor impact
- [ ] SEV4 - No user impact

### Timeline

| Time (UTC) | Event |
|------------|-------|
| HH:MM | [First alert] |
| HH:MM | [Investigation started] |
| HH:MM | [Root cause identified] |
| HH:MM | [Fix deployed] |
| HH:MM | [Resolved] |

### Impact
- **Duration**: [X hours Y minutes]
- **Users affected**: [Number/percentage]
- **Services affected**: [List]

### Root Cause
[What caused this incident]

### Resolution
[How was it fixed]

### Detection
- **How detected**: [Alert/User report/etc.]
- **Time to detect**: [Duration]

### Response
- **Time to respond**: [Duration]
- **Time to resolve**: [Duration]

### Action Items

| Action | Owner | Due Date | Status |
|--------|-------|----------|--------|
| [Action 1] | [Name] | YYYY-MM-DD | [ ] |
| [Action 2] | [Name] | YYYY-MM-DD | [ ] |

### Lessons Learned

#### What went well
- [Item 1]

#### What could be improved
- [Item 1]

### Prevention
[How to prevent recurrence]
```

---

## Release Notes Template

```markdown
## Release [VERSION]

**Release Date**: YYYY-MM-DD

### Highlights
[Key features or changes in this release]

### New Features
- **[Feature]**: [Description]

### Improvements
- **[Area]**: [What was improved]

### Bug Fixes
- **[Bug]**: [What was fixed] (#issue)

### Breaking Changes
- **[Change]**: [What breaks and migration path]

### Deprecations
- **[Feature]**: [Will be removed in version X]

### Security
- **[Fix]**: [Security improvement]

### Dependencies
- Updated [package] from X to Y

### Known Issues
- [Issue that exists in this release]

### Upgrade Guide
1. [Step 1]
2. [Step 2]

### Contributors
Thanks to @contributor for their contributions!
```

---

## Quick Reference

| Template | Use When |
|----------|----------|
| ADR | Major technical decisions |
| PR | Submitting code changes |
| Bug Report | Reporting issues |
| Feature Request | Proposing new functionality |
| Incident Report | Post-incident documentation |
| Release Notes | Publishing new versions |
