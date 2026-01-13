# Memory Management Policy

Persistent memory system using @modelcontextprotocol/server-memory knowledge graph.

**Last Updated**: 2026-01-13

---

## 1. Overview

The memory system enables Claude to retain and recall:
- Code patterns and architectural decisions
- User preferences and working styles
- Project context and domain knowledge
- Workflow learnings and optimizations
- Key files and component relationships

**Storage**: Global JSONL file at `~/.claude/memory.jsonl` (or `%USERPROFILE%\.claude\memory.jsonl` on Windows)

**Approach**: Hybrid - single global file with strict project namespacing

---

## 2. Entity Naming Convention

### Namespace Structure

All entities MUST follow this naming pattern:

```
[scope]:[category]:[identifier]
```

### Scopes

| Scope | Purpose | Example |
|-------|---------|---------|
| `global` | Cross-project knowledge | `global:pattern:error-handling-retry` |
| `user` | User preferences/style | `user:preference:commit-style` |
| `project:[name]` | Project-specific | `project:my-app:architecture:overview` |

### Categories

| Category | Purpose | Example Entities |
|----------|---------|------------------|
| `pattern` | Reusable code patterns | `global:pattern:api-error-handling` |
| `decision` | Architecture decisions | `project:my-app:decision:use-postgres` |
| `preference` | User preferences | `user:preference:test-coverage-80` |
| `architecture` | System structure | `project:my-app:architecture:overview` |
| `file` | Important files | `project:my-app:file:src-auth-service` |
| `component` | Code components | `project:my-app:component:auth-middleware` |
| `tool` | Development tools | `global:tool:pytest` |
| `workflow` | Process knowledge | `global:workflow:pr-review-checklist` |
| `person` | Team members | `project:my-app:person:lead-developer` |
| `domain` | Business domain | `project:my-app:domain:payment-processing` |

### Entity Type Taxonomy

Use these standard `entityType` values:

| entityType | Description |
|------------|-------------|
| `Pattern` | Reusable code or design pattern |
| `Decision` | Architectural or technical decision |
| `Preference` | User configuration or style choice |
| `Architecture` | System design element |
| `File` | Source file or configuration |
| `Component` | Code module or service |
| `Tool` | Development tool or utility |
| `Workflow` | Process or procedure |
| `Person` | Team member or stakeholder |
| `Domain` | Business domain concept |
| `Project` | Project metadata |

---

## 3. Observation Guidelines

### Atomic Facts

Each observation should be:
- **Atomic**: One fact per observation
- **Self-contained**: Understandable without other observations
- **Timestamped** (when relevant): Include date for time-sensitive facts
- **Actionable**: Lead to concrete behavior

### Good vs Bad Observations

```
GOOD (atomic, actionable):
- "Uses pytest for all test files"
- "Prefers explicit type hints over Any"
- "Database migrations require review before merge"
- "[2026-01-13] Decided to use Redis for session storage"

BAD (compound, vague):
- "Uses pytest and follows TDD with 80% coverage and likes descriptive names"
- "Good developer"
- "Knows about databases"
```

### Observation Prefixes

Use prefixes for context:

| Prefix | Purpose | Example |
|--------|---------|---------|
| `[YYYY-MM-DD]` | Time-sensitive facts | `[2026-01-13] Migrated from MySQL to PostgreSQL` |
| `DEPRECATED:` | Outdated information | `DEPRECATED: Used Flask, now uses FastAPI` |
| `CRITICAL:` | Must-follow rules | `CRITICAL: Never commit .env files` |
| `PREFERENCE:` | User choice | `PREFERENCE: 2-space indentation` |

---

## 4. Relation Guidelines

### Relation Types (Active Voice)

| relationType | Meaning | Example |
|--------------|---------|---------|
| `uses` | Dependency/tool usage | Project -> uses -> Tool |
| `implements` | Pattern implementation | Component -> implements -> Pattern |
| `decided` | Decision maker | Person -> decided -> Decision |
| `contains` | Composition | Project -> contains -> Component |
| `depends_on` | Runtime dependency | Component -> depends_on -> Component |
| `created_by` | Authorship | File -> created_by -> Person |
| `documented_in` | Documentation link | Decision -> documented_in -> File |
| `supersedes` | Replacement | Decision -> supersedes -> Decision |
| `relates_to` | General association | Domain -> relates_to -> Component |
| `follows` | Standard adherence | Component -> follows -> Pattern |

### Relation Direction

Relations are ALWAYS source -> target:
- The `from` entity performs the action
- The `to` entity receives the action
- Use active voice for `relationType`

---

## 5. Memory Operations

### When to CREATE Entities

| Trigger | Entity Type | Example |
|---------|-------------|---------|
| New project discovered | Project | `project:my-app:project:metadata` |
| Architecture decision made | Decision | `project:my-app:decision:api-versioning` |
| Pattern identified | Pattern | `global:pattern:repository-pattern` |
| User expresses preference | Preference | `user:preference:commit-message-style` |
| Important file created | File | `project:my-app:file:src-core-config` |

### When to UPDATE (add_observations)

| Trigger | Action |
|---------|--------|
| Learning new fact about entity | Add observation |
| Entity behavior changes | Add timestamped observation |
| User clarifies preference | Add/update preference observation |
| Decision rationale discovered | Add reasoning observation |

### When to DELETE

| Trigger | Action |
|---------|--------|
| Project removed/archived | Delete all `project:[name]:*` entities |
| Entity obsolete (>6 months stale) | Delete during cleanup |
| Duplicate entity discovered | Merge observations, delete duplicate |
| Information proven incorrect | Delete specific observations |

---

## 6. Session Protocols

### Session Start (MANDATORY)

**Before any task work, execute memory retrieval:**

1. Search for project context:
   ```
   search_nodes("project:[current-project-name]")
   ```

2. Load user preferences:
   ```
   search_nodes("user:preference")
   ```

3. Load relevant global patterns:
   ```
   search_nodes("global:pattern")
   ```

4. Synthesize retrieved context before proceeding

### During Session

| Event | Memory Action |
|-------|---------------|
| Significant decision made | Create Decision entity with observations |
| New pattern discovered | Create/update Pattern entity |
| User corrects behavior | Create/update Preference entity |
| Important code created | Consider File/Component entity |
| Problem solved creatively | Document in Pattern entity |

### Session End

| Event | Memory Action |
|-------|---------------|
| Task completed successfully | Update relevant entities with learnings |
| New insights gained | Add observations to existing entities |
| User expressed satisfaction/frustration | Update Preference entities |

---

## 7. Memory Hygiene

### Periodic Cleanup (Monthly)

1. **Review stale entities**: Entities not referenced in 3+ months
2. **Merge duplicates**: Consolidate similar entities
3. **Archive deprecated**: Mark with `DEPRECATED:` prefix
4. **Delete obsolete**: Remove clearly outdated information
5. **Validate relations**: Remove relations to deleted entities

### Cleanup Protocol

```
1. read_graph() - Full memory audit
2. Identify candidates:
   - Entities with only DEPRECATED observations
   - Orphan entities (no relations)
   - Project entities for deleted projects
3. Confirm with user before deletion
4. delete_entities() for confirmed removals
5. Document cleanup in workflow log
```

### Size Management

| Threshold | Action |
|-----------|--------|
| < 500 entities | Normal operation |
| 500-1000 entities | Review for consolidation |
| > 1000 entities | Aggressive cleanup required |

---

## 8. Example Entities

### Project Entity

```json
{
  "name": "project:claude-config:project:metadata",
  "entityType": "Project",
  "observations": [
    "Universal configuration system for Claude Code",
    "Uses Markdown for documentation",
    "No runtime code - configuration only",
    "[2026-01-13] Added memory integration"
  ]
}
```

### Decision Entity

```json
{
  "name": "project:my-app:decision:use-postgres",
  "entityType": "Decision",
  "observations": [
    "[2026-01-10] Chose PostgreSQL over MySQL",
    "Rationale: Better JSON support for flexible schemas",
    "Alternative considered: MongoDB - rejected for ACID requirements",
    "CRITICAL: All migrations require DBA review"
  ]
}
```

### User Preference Entity

```json
{
  "name": "user:preference:code-style",
  "entityType": "Preference",
  "observations": [
    "PREFERENCE: Explicit type hints always",
    "PREFERENCE: Descriptive variable names over short ones",
    "PREFERENCE: Comments explain 'why' not 'what'",
    "PREFERENCE: 80% minimum test coverage"
  ]
}
```

### Pattern Entity

```json
{
  "name": "global:pattern:retry-with-backoff",
  "entityType": "Pattern",
  "observations": [
    "Use exponential backoff for API retries",
    "Max 3 retries with jitter",
    "Example: tenacity library in Python",
    "Applied in: project:my-app, project:other-app"
  ]
}
```

### Example Relations

```json
[
  {
    "from": "project:my-app:component:api-client",
    "to": "global:pattern:retry-with-backoff",
    "relationType": "implements"
  },
  {
    "from": "project:my-app:project:metadata",
    "to": "global:tool:pytest",
    "relationType": "uses"
  },
  {
    "from": "project:my-app:decision:use-postgres",
    "to": "project:my-app:file:docs-adr-001-database",
    "relationType": "documented_in"
  }
]
```

---

## 9. Available Tools

| Tool | Purpose | Example Usage |
|------|---------|---------------|
| `create_entities` | Add knowledge nodes | Batch create project entities |
| `create_relations` | Link entities | Connect decision to documentation |
| `add_observations` | Add facts to entities | Record new learning |
| `delete_entities` | Remove nodes | Clean up obsolete project |
| `delete_observations` | Remove facts | Correct outdated info |
| `delete_relations` | Remove links | Fix incorrect connections |
| `read_graph` | Full knowledge audit | Monthly cleanup |
| `search_nodes` | Query by content | Find project context |
| `open_nodes` | Fetch specific entities | Load known entity |

---

## 10. Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| Memory not persisting | Check MEMORY_FILE_PATH is writable |
| Slow searches | Reduce entity count through cleanup |
| Duplicate entities | Use consistent naming convention |
| Cross-project confusion | Verify project namespacing |

### Validation Commands

```
# Read current graph size
read_graph() -> count entities and relations

# Search for specific project
search_nodes("project:[name]")

# Verify entity exists
open_nodes(["entity:name"])
```

---

*This policy ensures Claude Code maintains accurate, useful persistent memory.*
*See [../WORKFLOW.md](../WORKFLOW.md) for task integration.*
*See [../mcp-config.md](../mcp-config.md) for MCP setup.*
