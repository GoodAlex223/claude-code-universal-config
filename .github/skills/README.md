# Agent Skills

Portable AI agent skills for VS Code Copilot, GitHub Copilot CLI, and other AI agents.

These skills follow the [Agent Skills open standard](https://agentskills.io).

---

## Available Skills

### Core Workflow

| Skill | Description |
|-------|-------------|
| [critical-thinking](critical-thinking/) | Deep analysis with EXPLORE-CHALLENGE-SYNTHESIZE phases |
| [development-workflow](development-workflow/) | Plan-Execute-Verify-Document cycle for structured development |
| [task-planning](task-planning/) | Structured task planning with analysis and implementation steps |
| [git-workflow](git-workflow/) | Branch strategy, conventional commits, and PR guidelines |

### Quality Standards

| Skill | Description |
|-------|-------------|
| [code-review](code-review/) | Self-review checklists and PR review standards |
| [testing-standards](testing-standards/) | TDD workflow, coverage requirements, and quality gates |
| [security-standards](security-standards/) | Security validation, secrets management, OWASP guidelines |

### Language Standards

| Skill | Description |
|-------|-------------|
| [python-standards](python-standards/) | Python coding conventions and best practices |
| [typescript-standards](typescript-standards/) | TypeScript/JavaScript coding standards |

### Templates

| Skill | Description |
|-------|-------------|
| [documentation-templates](documentation-templates/) | ADR, PR, bug report, and other templates |

---

## Usage

Skills are automatically loaded by compatible AI agents (VS Code Copilot, GitHub Copilot CLI) when relevant to your request.

### Manual Loading

You can reference skills explicitly in your prompts:
- "Use the critical-thinking skill to analyze this problem"
- "Follow the git-workflow skill for this commit"

### Installation

These skills are already installed in this repository. To use in other projects:

1. Copy the `.github/skills/` directory to your project
2. Customize skills as needed for your project
3. Skills will be automatically discovered by compatible AI agents

---

## Skill Format

Each skill contains a `SKILL.md` file with:

```yaml
---
name: skill-identifier
description: Brief description of what the skill does
---

# Skill Instructions

Detailed instructions for the AI agent...
```

---

## Related Documentation

- [CLAUDE.md](../../CLAUDE.md) - Universal Claude Code configuration
- [PROJECT.md](../../PROJECT.md) - Project-specific settings
- [.claude/WORKFLOW.md](../../.claude/WORKFLOW.md) - Development workflow details

---

*Skills based on [claude-code-universal-config](https://github.com/alexm/claude-code-universal-config)*
