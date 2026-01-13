---
name: memory-management
description: Auto-trigger on session start for memory retrieval. Query persistent knowledge graph for project context, user preferences, and relevant patterns before beginning work. Use when starting sessions, making decisions, or capturing learnings.
---

# Memory Management Skill

Persistent knowledge graph management for cross-session continuity.

---

## Session Start Protocol

**MANDATORY at every session start:**

### Step 1: Announce Memory Retrieval

Begin with: "Retrieving memory context..."

### Step 2: Query Project Context

```
search_nodes("project:[current-project-name]")
```

Extract:
- Project metadata and purpose
- Architecture decisions
- Known patterns and conventions
- Important files and components

### Step 3: Load User Preferences

```
search_nodes("user:preference")
```

Extract:
- Code style preferences
- Communication preferences
- Workflow preferences
- Tool preferences

### Step 4: Load Relevant Global Patterns

Based on task type, search:
- `global:pattern:*` for coding tasks
- `global:workflow:*` for process tasks
- `global:tool:*` for tooling tasks

### Step 5: Synthesize Context

Present summary:
```markdown
**Memory Context Loaded:**
- Project: [name] - [brief description]
- Key decisions: [list relevant decisions]
- Preferences: [list active preferences]
- Patterns available: [list relevant patterns]
```

---

## Memory Update Patterns

### After Decisions

When a significant decision is made:

```
create_entities([{
  "name": "project:[project]:decision:[short-name]",
  "entityType": "Decision",
  "observations": [
    "[YYYY-MM-DD] Decision: [what was decided]",
    "Rationale: [why this choice]",
    "Alternatives: [what was considered]",
    "Impact: [affected areas]"
  ]
}])
```

### After Pattern Discovery

When identifying a reusable pattern:

```
create_entities([{
  "name": "global:pattern:[pattern-name]",
  "entityType": "Pattern",
  "observations": [
    "Purpose: [what problem it solves]",
    "Implementation: [how to apply]",
    "Example: [where it's used]"
  ]
}])
```

### After User Feedback

When user expresses preference:

```
add_observations({
  "entityName": "user:preference:[category]",
  "observations": [
    "PREFERENCE: [the preference]"
  ]
})
```

If entity doesn't exist, create it first.

---

## Memory Update/Delete Patterns

### Update Existing Entity

When adding new information to an existing entity:

```
add_observations({
  "entityName": "project:my-app:decision:use-postgres",
  "observations": [
    "[2026-01-13] Added read replica for scaling"
  ]
})
```

### Mark Information as Deprecated

When a fact is outdated but may have historical value:

```
add_observations({
  "entityName": "project:my-app:architecture:overview",
  "observations": [
    "DEPRECATED: Previously used monolithic architecture"
  ]
})
```

### Delete Outdated Observations

When a specific fact is wrong or no longer relevant:

```
delete_observations({
  "entityName": "project:my-app:file:old-config",
  "observations": ["Uses legacy config format"]
})
```

### Delete Obsolete Entities

When an entire entity is no longer valid (file deleted, project archived):

```
delete_entities(["project:old-project:file:deleted-module"])
```

### Delete Invalid Relations

When a connection between entities no longer applies:

```
delete_relations([{
  "from": "project:my-app:component:auth",
  "to": "global:pattern:deprecated-pattern",
  "relationType": "implements"
}])
```

---

## Memory Query Patterns

### Find Project Architecture

```
search_nodes("project:[name]:architecture")
open_nodes(["project:[name]:architecture:overview"])
```

### Find Related Decisions

```
search_nodes("project:[name]:decision")
```

### Find Applicable Patterns

```
search_nodes("global:pattern:[domain]")
```

### Find All User Preferences

```
search_nodes("user:preference")
```

---

## Memory Maintenance

### During Session (Proactive)

**When retrieving memory at session start, check for staleness:**

| If you find... | Action |
|----------------|--------|
| Entity references deleted file | Delete the entity |
| Observation with `DEPRECATED:` prefix | Consider removing |
| Decision that was reversed | Update or delete |
| Fact that contradicts current code | Delete and correct |

**Update immediately** - don't wait until session end.

### When to Clean Up (Reactive)

- At explicit user request
- When encountering "entity not found" errors
- When memory seems outdated
- Monthly as routine hygiene

### Cleanup Steps

1. `read_graph()` - Audit current state
2. Identify stale entities (no recent observations)
3. Confirm with user before deletion
4. `delete_entities()` for confirmed items
5. `delete_observations()` for outdated facts

---

## Quick Reference

### Entity Naming

```
[scope]:[category]:[identifier]

Scopes: global, user, project:[name]
Categories: pattern, decision, preference, architecture, file, component, tool, workflow, person, domain
```

### Common Operations

| Task | Operation |
|------|-----------|
| Start session | `search_nodes("project:[name]")` |
| Record decision | `create_entities([...])` |
| Add fact | `add_observations({...})` |
| Find pattern | `search_nodes("global:pattern")` |
| **Update stale info** | `delete_observations({...})` then `add_observations({...})` |
| **Delete obsolete entity** | `delete_entities([...])` |
| **Remove wrong fact** | `delete_observations({...})` |
| Full audit | `read_graph()` |

### Memory Tools

| Tool | Purpose |
|------|---------|
| `create_entities` | Add new knowledge nodes |
| `create_relations` | Link entities together |
| `add_observations` | Add facts to existing entities |
| `delete_entities` | Remove obsolete nodes |
| `delete_observations` | Remove outdated facts |
| `delete_relations` | Remove invalid links |
| `read_graph` | Full knowledge audit |
| `search_nodes` | Query by content |
| `open_nodes` | Fetch specific entities |

---

*Use this skill at session start and when capturing significant learnings.*
*See [../../POLICIES/memory.md](../../POLICIES/memory.md) for full policy.*
