---
name: code-review
description: Self-review checklist and PR review standards for code quality. Use when reviewing your own code before commit, reviewing others' PRs, or when asked to perform a code review.
---

# Code Review Standards

Every code change benefits from review. Reviews catch bugs, improve design, and share knowledge.

---

## Review Requirements

| Change Type | Self-Review | Peer Review |
|-------------|-------------|-------------|
| Bug fix | Required | Recommended (critical bugs: required) |
| New feature | Required | Required |
| Refactoring | Required | Required |
| Configuration | Required | Optional (security: required) |
| Documentation | Required | Optional |
| Hotfix | Required | Post-merge review |

---

## Self-Review Checklist

Before requesting peer review:

### Functionality
- [ ] Code does what the task requires
- [ ] Edge cases are handled
- [ ] Error conditions are handled gracefully
- [ ] No debugging code left in (print, console.log, etc.)

### Code Quality
- [ ] Follows project conventions
- [ ] No code duplication (DRY)
- [ ] Functions are focused (single responsibility)
- [ ] Names are clear and descriptive
- [ ] Complex logic has comments explaining "why"

### Testing
- [ ] Tests exist for new functionality
- [ ] Tests pass locally
- [ ] Edge cases have test coverage
- [ ] No skipped tests without documented reason

### Security
- [ ] No hardcoded secrets or credentials
- [ ] User input is validated
- [ ] SQL/command injection prevented
- [ ] Sensitive data not logged

### Performance
- [ ] No obvious performance issues
- [ ] Database queries are efficient
- [ ] No unnecessary loops
- [ ] Resources are properly released

---

## Conducting Reviews

### What to Look For

| Category | Questions |
|----------|-----------|
| **Correctness** | Does this do what it's supposed to? Are there bugs? |
| **Design** | Is this the right approach? Does it fit the architecture? |
| **Readability** | Can I understand this code? Will others? |
| **Maintainability** | Will this be easy to modify in 6 months? |
| **Testing** | Are tests sufficient? Do they test the right things? |
| **Security** | Are there vulnerabilities? Is data protected? |
| **Performance** | Will this scale? Are there bottlenecks? |

### Review Depth by Risk

| Risk Level | Depth | Time |
|------------|-------|------|
| Low (docs, config) | Quick scan | 5-10 min |
| Medium (features) | Thorough | 15-30 min |
| High (security, data) | Deep | 30-60 min |
| Critical (auth, payments) | Multiple reviewers | As needed |

---

## Providing Feedback

### Use Clear Categories

```
[MUST] - Blocking issue, must fix before merge
[SHOULD] - Important improvement, strongly recommended
[COULD] - Nice to have, optional improvement
[QUESTION] - Seeking clarification
[NITPICK] - Minor style issue, author's discretion
```

### Be Specific and Constructive

```
Bad: "This is wrong"

Good: "[MUST] This will throw NullPointerException when user is None.
      Consider adding a null check: `if user is not None:`"
```

```
Bad: "Make this better"

Good: "[SHOULD] This loop iterates 3 times over the same list.
      Consider combining into a single pass for O(n) instead of O(3n)."
```

### Acknowledge Good Work
- Point out clever solutions
- Recognize good test coverage
- Note improvements to existing code

---

## Review Checklists by Category

### Functionality
- [ ] Code accomplishes stated objective
- [ ] Logic is correct
- [ ] Edge cases handled
- [ ] No regressions

### Security
- [ ] Authentication/authorization correct
- [ ] Input validation present
- [ ] No injection vulnerabilities
- [ ] Secrets not exposed
- [ ] HTTPS/encryption used

### Performance
- [ ] No N+1 queries
- [ ] Appropriate data structures
- [ ] No unnecessary computation
- [ ] Resources closed/released

### Code Quality
- [ ] Follows style guide
- [ ] Clear naming
- [ ] Appropriate abstraction
- [ ] No dead code
- [ ] Type hints present

### Testing
- [ ] Unit tests for new code
- [ ] Integration tests where needed
- [ ] Test names describe behavior
- [ ] Tests are deterministic
- [ ] Edge cases covered

### Documentation
- [ ] Public APIs documented
- [ ] Complex logic explained
- [ ] README updated if needed
- [ ] Breaking changes documented

---

## Responding to Review

### As Author

1. Read all feedback before responding
2. Address every comment
3. Don't take it personally
4. Ask for clarification if needed
5. Thank reviewers

### Making Changes

- Fix all [MUST] items
- Seriously consider [SHOULD] items
- Use judgment on [COULD] and [NITPICK]
- Re-request review after significant changes

---

## Special Review Types

### Security-Sensitive Changes
- [ ] Reviewed by security-knowledgeable person
- [ ] Threat model considered
- [ ] Attack vectors evaluated
- [ ] Follows OWASP guidelines

### Database Changes
- [ ] Migration tested both directions
- [ ] Performance impact evaluated
- [ ] Backward compatibility verified
- [ ] Rollback plan documented

### API Changes
- [ ] Breaking changes identified
- [ ] Versioning considered
- [ ] Documentation updated
- [ ] Client impact assessed

---

## Healthy Review Indicators

| Metric | Target |
|--------|--------|
| Time to first review | < 24 hours |
| Review iterations | 1-3 rounds |
| Comments per review | 3-10 |
| PR size | < 400 lines |

### Warning Signs

- Reviews taking > 48 hours
- Same issues repeatedly found
- Large PRs (> 1000 lines)
- Reviews with 0 comments
- Frequent "LGTM" without substance
