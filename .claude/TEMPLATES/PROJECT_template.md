# PROJECT.md

Project-specific configuration. Universal rules are in [CLAUDE.md](CLAUDE.md).

**Last Updated**: YYYY-MM-DD

---

## Project Overview

<!-- 
Brief description of what this project does.
Keep it to 2-3 sentences.
-->

[Description of your project]

### Tech Stack

| Component | Technology |
|-----------|------------|
| Language | [e.g., Python 3.11] |
| Framework | [e.g., aiogram 3.x] |
| Database | [e.g., SQLite / PostgreSQL] |
| Testing | [e.g., pytest] |
| CI/CD | [e.g., GitHub Actions] |

---

## Project Structure

<!-- 
Key directories and files. Only include what's important for navigation.
Update this when structure changes significantly.
-->

| Component | Location | Purpose |
|-----------|----------|---------|
| Entry Point | `[path]` | Main application |
| Core Logic | `[path/]` | Business logic |
| Database | `[path/]` | Data layer |
| Tests | `tests/` | Test suites |
| Config | `[path]` | Configuration files |

---

## Commands

### Development

```bash
# Run application
[command]

# Run with auto-restart (if applicable)
[command]

# Run tests
[command]

# Run specific test file
[command]
```

### Code Quality

```bash
# Linting
[command]

# Type checking
[command]

# All pre-commit hooks
pre-commit run --all-files
```

---

## Critical Systems (Tier Classification)

<!--
Systems that require extra caution when modifying.
Claude MUST consult user before modifying Tier 1 systems.
-->

| Tier | Description | Examples | Modification Rules |
|------|-------------|----------|-------------------|
| 1 | Critical | [e.g., Auth, payments] | Requires explicit user approval |
| 2 | Important | [e.g., Core business logic] | Requires plan review |
| 3 | Standard | [e.g., Utilities, helpers] | Standard workflow |
| 4 | Low-risk | [e.g., Documentation, tests] | Proceed with normal care |

---

## Project-Specific Conventions

### Naming Conventions

<!-- 
Project-specific naming rules beyond standard language conventions.
-->

| Element | Convention | Example |
|---------|------------|---------|
| [Type] | [Rule] | [Example] |

### Code Patterns

<!--
Patterns specific to this project that Claude should follow.
-->

- [Pattern 1]: [Description]
- [Pattern 2]: [Description]

### Error Handling

<!--
How errors should be handled in this project.
-->

[Describe error handling approach]

---

## External Dependencies

### APIs

| Service | Purpose | Docs Location |
|---------|---------|---------------|
| [API name] | [What it's used for] | [Link or file path] |

### Configuration

| Variable | Purpose | Location |
|----------|---------|----------|
| [VAR_NAME] | [What it configures] | [.env / config file] |

---

## Domain-Specific Documentation

<!--
Links to project-specific documentation beyond the standard structure.
-->

| Document | Purpose |
|----------|---------|
| [docs/path/file.md](docs/path/file.md) | [Description] |

---

## Localization (if applicable)

<!--
Remove this section if project doesn't have localization.
-->

| Language | Location | Status |
|----------|----------|--------|
| [Lang] | `[path]` | [Complete/Partial] |

### Localization Rules

When adding/modifying features with user-facing text:
1. [Rule 1]
2. [Rule 2]

---

## Deployment

### Environments

| Environment | URL/Location | Branch |
|-------------|--------------|--------|
| Development | [local/dev URL] | develop |
| Staging | [staging URL] | staging |
| Production | [prod URL] | main |

### Pre-deployment Checklist

- [ ] All tests passing
- [ ] Version bumped (if applicable)
- [ ] Changelog updated
- [ ] [Project-specific checks]

---

## Known Limitations

<!--
Document known issues, technical debt, or limitations that affect development.
-->

1. [Limitation 1]: [Description and workaround]
2. [Limitation 2]: [Description and workaround]

---

## Contact / Ownership

| Role | Contact |
|------|---------|
| Maintainer | [Name/handle] |
| [Other role] | [Contact] |

---

*For universal Claude Code rules, see [CLAUDE.md](CLAUDE.md).*
*For documentation index, see [docs/README.md](docs/README.md).*
