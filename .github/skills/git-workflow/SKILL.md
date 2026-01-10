---
name: git-workflow
description: Git branching strategy, conventional commits, PR guidelines, and merge strategies. Use when making commits, creating branches, or preparing pull requests to ensure clean and consistent version control.
---

# Git Workflow

Git history should be clean, meaningful, and useful for understanding project evolution.

---

## Branch Strategy

### Branch Types

| Branch | Purpose | Naming | Lifetime |
|--------|---------|--------|----------|
| `main` | Production-ready code | `main` | Permanent |
| `develop` | Integration branch | `develop` | Permanent |
| `feature/*` | New features | `feature/short-description` | Until merged |
| `bugfix/*` | Bug fixes | `bugfix/issue-or-description` | Until merged |
| `hotfix/*` | Production fixes | `hotfix/critical-issue` | Until merged |
| `release/*` | Release preparation | `release/vX.Y.Z` | Until released |
| `refactor/*` | Code restructuring | `refactor/what-refactored` | Until merged |

### Branch Naming

```
Good:
feature/user-authentication
bugfix/login-validation-error
hotfix/security-patch-cve-2024
refactor/database-layer
release/v2.1.0

Bad:
feature/my-feature
fix
test-branch
john-working
```

### Branch Rules

| Rule | Main | Develop | Feature |
|------|------|---------|---------|
| Direct commits | No | No | Yes |
| Force push | Never | No | Before review only |
| Requires review | Yes | Yes | Optional |
| CI must pass | Yes | Yes | Yes |

---

## Commit Standards

### Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

| Part | Required | Description |
|------|----------|-------------|
| type | Yes | Category of change |
| scope | No | Component affected |
| subject | Yes | Brief description (imperative mood) |
| body | No | Detailed explanation |
| footer | No | References, breaking changes |

### Commit Types

| Type | Description | Example |
|------|-------------|---------|
| `feat` | New feature | `feat(auth): add OAuth2 login` |
| `fix` | Bug fix | `fix(api): handle null response` |
| `docs` | Documentation only | `docs: update API reference` |
| `style` | Formatting, no logic change | `style: fix indentation` |
| `refactor` | Code change, no feature/fix | `refactor: extract validation` |
| `test` | Adding/updating tests | `test: add login edge cases` |
| `chore` | Maintenance tasks | `chore: update dependencies` |
| `perf` | Performance improvement | `perf: optimize query` |
| `ci` | CI/CD changes | `ci: add coverage report` |
| `revert` | Revert previous commit | `revert: feat(auth): add OAuth2` |

### Commit Examples

```
feat(auth): implement two-factor authentication

Add TOTP-based 2FA for enhanced account security.
- Generate and store encrypted secrets
- Verify codes during login
- Add backup codes for recovery

Closes #123
```

```
fix(api): prevent race condition in session handler

Multiple concurrent requests could corrupt session data
due to unsynchronized access. Added mutex lock around
session read/write operations.

Fixes #456
```

```
refactor(db): migrate from raw SQL to ORM

BREAKING CHANGE: Database access layer API changed.
See migration guide in docs/migrations/v3.md
```

### Commit Best Practices

**DO:**
- Write in imperative mood ("Add feature" not "Added feature")
- Keep subject under 50 characters
- Wrap body at 72 characters
- Explain "what" and "why", not "how"
- Reference issues/tasks
- Make atomic commits (one logical change)

**DON'T:**
- Commit generated files (unless intentional)
- Mix unrelated changes
- Write vague messages ("fix", "update", "WIP")
- Include sensitive data
- Leave merge conflict markers

---

## Workflow Patterns

### Feature Development

```bash
# 1. Start from up-to-date main
git checkout main
git pull origin main

# 2. Create feature branch
git checkout -b feature/user-profile

# 3. Work (multiple commits OK)
git add .
git commit -m "feat(profile): add avatar upload"

# 4. Keep updated (if long-lived)
git fetch origin
git rebase origin/main

# 5. Push for review
git push origin feature/user-profile

# 6. After approval, merge via PR

# 7. Delete branch after merge
git branch -d feature/user-profile
```

### Bug Fix

```bash
git checkout main && git pull origin main
git checkout -b bugfix/login-error

git add .
git commit -m "fix(auth): handle expired tokens gracefully

Tokens expired mid-session caused 500 error.
Now redirects to login with message.

Fixes #789"

git push origin bugfix/login-error
```

### Hotfix

```bash
# 1. Branch from main
git checkout main && git pull
git checkout -b hotfix/security-patch

# 2. Minimal fix only
git commit -m "fix(security): patch XSS vulnerability"

# 3. Expedited review and merge

# 4. Tag release
git tag -a v1.2.1 -m "Security patch"
git push origin v1.2.1
```

---

## Merge Strategies

| Strategy | When to Use | Result |
|----------|-------------|--------|
| Merge commit | Preserving feature history | Merge commit + all feature commits |
| Squash merge | Clean history, many small commits | Single commit |
| Rebase | Linear history, clean commits | Feature commits on top of main |

**Recommended**: Squash merge for clean history, one commit per feature.

---

## Pull Request Guidelines

### PR Template

```markdown
## Summary
[Brief description of changes]

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update
- [ ] Refactoring

## Changes Made
- [Change 1]
- [Change 2]

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] Manual testing performed

## Checklist
- [ ] Code follows project style
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No new warnings

## Related
- Task: [Reference]
- Plan: [Link]
```

### PR Size Guidelines

| Size | Lines | Recommendation |
|------|-------|----------------|
| Small | < 100 | Ideal |
| Medium | 100-400 | Acceptable |
| Large | 400-1000 | Consider splitting |
| XL | 1000+ | Split required |

---

## Recovery Procedures

### Undo Last Commit (Not Pushed)

```bash
git reset --soft HEAD~1  # Keep changes
git reset --hard HEAD~1  # Discard changes
```

### Undo Pushed Commit

```bash
git revert HEAD  # Safe: creates reverting commit
git push
```

### Recover Deleted Branch

```bash
git reflog
git checkout -b recovered-branch <commit-hash>
```

---

## Git Configuration

```bash
# User identification
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Recommended settings
git config --global init.defaultBranch main
git config --global diff.algorithm histogram
git config --global rebase.autoStash true
git config --global fetch.prune true
git config --global merge.conflictStyle diff3
```
