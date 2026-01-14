# Development Workflow

Universal development workflow. Project-specific commands are in [PROJECT.md](../PROJECT.md).

---

## Overview

Every task follows this cycle:

```
REMEMBER → PLAN → EXECUTE → VERIFY → DOCUMENT → COMPLETE
```

Each phase has mandatory checkpoints that must be satisfied before proceeding.

---

## Plan Lifecycle Management

**Plans MUST follow this lifecycle:**

```
1. CREATE    → Create plan (in docs/plans/ or /root/.claude/plans/)
                     ↓
2. SYNC      → If created in /root/.claude/plans/, IMMEDIATELY copy to docs/plans/
                     ↓
3. TRACK     → Update docs/plans/ version during execution
                     ↓
4. ARCHIVE   → Move to docs/archive/plans/ after completion
```

### Plan Location Rules

| Stage | Location | Action |
|-------|----------|--------|
| Creation | `docs/plans/` (preferred) or `/root/.claude/plans/` | Create using template |
| Active | `docs/plans/` | **Required** - must exist here during execution |
| Complete | `docs/archive/plans/` | Move after all steps done |

### If Plan Created in /root/.claude/plans/

**IMMEDIATELY after creation:**
1. Copy to `docs/plans/YYYY-MM-DD_task-name.md`
2. Both locations must stay synchronized during execution
3. Update `docs/plans/` version as the source of truth

### During Execution

- Update `docs/plans/` version with progress
- Mark completed steps with `[x]`
- Add implementation log entries with timestamps
- Document discoveries and deviations

### After Completion

**Follow the Task Completion Documentation Sequence (Phase 5.1):**

```
1. EXTRACT    → Improvements from plan → BACKLOG.md / TODO.md
2. ARCHIVE    → Move plan to docs/archive/plans/
3. TRANSITION → Move task from TODO.md → DONE.md
4. COMMIT     → Git commit all documentation changes
5. MEMORY     → Update knowledge graph
```

See **Phase 5: COMPLETE** for detailed instructions on each step.

### Directory Structure

```
docs/
├── plans/                    # Active plans
│   └── 2025-01-15_add-auth.md
└── archive/
    └── plans/                # Completed plans
        └── 2025-01-10_setup-db.md
```

---

## Task Decomposition for Large Tasks

**When a task is large, complex, or voluminous, it MUST be divided into phases.**

### Criteria for Phase Division

A task requires phase division when ANY of these apply:
- Affects 5+ files
- Requires 3+ distinct implementation steps
- Involves multiple subsystems
- Estimated complexity exceeds 100 lines of changes
- Contains multiple independent sub-features

### Phase Execution Protocol

1. **Divide into logical phases** — Each phase should be a complete, testable unit
2. **Execute one phase at a time** — Complete each phase fully before proceeding
3. **Pause and report after each phase**:
   - Report what was accomplished
   - Document results in the plan file
   - List any deviations or discoveries
   - **Wait for developer confirmation to proceed**
4. **Apply completion rules to each phase** — Each phase follows the same rigor as a full task

### Phase Report Template

After completing each phase, provide:

```markdown
### Phase [N] Complete: [Phase Name]

**Accomplished**:
- [What was done]

**Files Modified**:
- [file] - [change description]

**Tests Status**: [Passing/Failing/Pending]

**Discoveries/Deviations**:
- [Any unexpected findings or plan changes]

**Ready for Phase [N+1]**: [Brief description of next phase]

⏸️ **Awaiting confirmation to proceed...**
```

### Developer Control

- Developer may approve, modify, or halt at any phase checkpoint
- If halted, document current state and remaining phases in plan file
- Phase boundaries are natural save points for complex work

---

## Phase 0: REMEMBER

### 0.1 Memory Retrieval (MANDATORY)

**Before starting ANY task, retrieve relevant memory:**

```
search_nodes("project:[project-name]")  → Project context
search_nodes("user:preference")         → User preferences
search_nodes("global:pattern")          → Reusable patterns
```

### 0.2 Context Synthesis

After retrieval, synthesize:

```markdown
**Memory Context:**
- Project: [name] - [key facts]
- Relevant decisions: [list]
- User preferences: [list]
- Applicable patterns: [list]
```

### 0.3 Memory Validation

**Check retrieved memory for staleness:**

| Check | Action if Found |
|-------|-----------------|
| Entity references deleted file/code | `delete_entities()` or `delete_observations()` |
| Observation marked `DEPRECATED:` | Consider deletion or update |
| Decision no longer applies | Add `DEPRECATED:` prefix or delete |
| Preference contradicts current request | Clarify with user, then update |

**Update stale information immediately:**
```
# Remove outdated observation
delete_observations({
  "entityName": "project:my-app:decision:old-choice",
  "observations": ["Outdated fact to remove"]
})

# Or delete entire entity if obsolete
delete_entities(["project:my-app:file:deleted-file"])
```

### 0.4 Memory-Informed Planning

Use retrieved context to:
- Avoid repeating past mistakes (check decisions)
- Apply established patterns (check patterns)
- Honor user preferences (check preferences)
- Understand project architecture (check architecture entities)

See: [POLICIES/memory.md](POLICIES/memory.md) for full memory policy.

---

## Phase 1: PLAN

### 1.1 Create Plan Document

**Before starting ANY task from TODO.md:**

Create a plan document in `docs/plans/` with filename: `YYYY-MM-DD_task-name.md`

Required sections:

```markdown
# [Task Name] - Implementation Plan

**Task Reference**: TODO.md Section X.Y.Z
**Created**: YYYY-MM-DD
**Status**: Planning | In Progress | Complete
**Last Updated**: YYYY-MM-DD

---

## 1. Task Overview

**Goal**: [Clear statement of objective]
**Context**: [Why this matters]
**Success Criteria**: [How we know it's done]

---

## 2. Initial Plan

**Approach**: [High-level strategy]

**Steps**:
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Files Affected**: [List]
**Dependencies**: [Other tasks/systems]
**Risks**: [Potential issues]

---

## 3. Implementation Log

[Updated during execution - see Phase 2]

---

## 4. Key Discoveries

[Filled after completion - see Phase 4]

---

## 5. Future Improvements

[Filled after completion - MANDATORY]
```

### 1.2 Analysis Requirements

Before proceeding to implementation:

- [ ] Problem restated in own words
- [ ] 3+ alternative approaches considered
- [ ] 5+ edge cases identified
- [ ] Existing similar code searched
- [ ] Trade-offs documented
- [ ] Recommended approach justified

### 1.3 User Confirmation

For complex tasks, present analysis summary and get user confirmation before coding.

---

## Phase 2: EXECUTE

### 2.1 Test-Driven Development (TDD)

**Mandatory sequence:**

1. **Write tests first** — Cover expected behavior
2. **Run tests** — Verify they fail (test the tests)
3. **Implement code** — Make tests pass
4. **Refactor** — Clean up while tests stay green
5. **Add edge case tests** — Cover discovered edge cases

### 2.2 Implementation Logging

Update plan document during execution:

```markdown
### [YYYY-MM-DD HH:MM] - [Activity]

**What was done**: [Description]
**Decisions made**: [Key choices with rationale]
**Discoveries**: [New information]
**Challenges**: [Problems and solutions]
**Deviation from plan**: [If any, with reason]
```

### 2.3 Checkpoints

**Every 50 lines of code:**
- [ ] Still aligned with plan?
- [ ] Deviations documented?
- [ ] Partial tests runnable?

**Before any file save:**
- [ ] Addresses actual requirement?
- [ ] No obvious bugs?
- [ ] Types correct?

### 2.4 Memory Checkpoints

**When making significant decisions:**
- [ ] Document decision in memory (create Decision entity)
- [ ] Link to related entities (create relations)
- [ ] Record rationale in observations

**When discovering patterns:**
- [ ] Check if pattern exists in memory
- [ ] If new, create Pattern entity
- [ ] If exists, add observation

### 2.5 Stuck Protocol

If stuck after 3 attempts:

1. **STOP** — Do not continue blindly
2. **Document** — What was tried, what failed
3. **Ask** — Request user clarification
4. **Wait** — Do not proceed without guidance

---

## Phase 3: VERIFY

### 3.1 Automated Verification

Before any commit, run the project's verification commands (defined in PROJECT.md):
- Pre-commit hooks
- Test suite
- Type checking
- Linting

**All must pass before proceeding.**

### 3.2 Manual Testing Scenarios Phase (MANDATORY)

**After code implementation, create/update manual testing scenarios.**

See: [POLICIES/manual-testing.md](POLICIES/manual-testing.md) for full policy.

#### Workflow

```
1. Review existing scenarios in docs/MANUAL_TESTING.md
2. Delete/modify scenarios that contradict changes
3. Add new scenarios that verify changes work correctly
4. Create automated tests that simulate the scenarios
```

#### When to Create Scenarios

| Code Change Type | Required Action |
|------------------|-----------------|
| Bug fix | Add reproduction script + verification scenario |
| New feature | Add user flow scenarios |
| Behavior change | Update existing + add new scenarios |
| Code deletion | Remove obsolete scenarios |

#### Scenario Format

```markdown
### Script [N]: [Brief Description]

**Added**: YYYY-MM-DD
**Corrected**: YYYY-MM-DD (or "N/A")
**Status**: [ ] Open / [x] Fixed

**Steps**:
1. [Specific action]
2. [Specific action]

**Expected**: [Correct behavior]
**Actual**: [Bug behavior if applicable]
**Fix Reference**: [commit/task reference]

**Automated Test**: tests/[path]::[test_name]
```

#### Checklist

- [ ] `docs/MANUAL_TESTING.md` reviewed
- [ ] Contradicting scenarios updated/deleted
- [ ] New scenarios added for all changes
- [ ] Automated tests created for scenarios
- [ ] Dates and references updated

**DO NOT skip this phase.** Manual testing scenarios prevent regression and document expected behavior.

### 3.3 Manual Testing Gate

**Create testing checklist** at `docs/manual_testing_checklist.md`:

```markdown
# Manual Testing Checklist

**Feature**: [Name]
**Date**: YYYY-MM-DD
**Plan Document**: [Link]
**Status**: Pending Testing

---

## Summary of Changes

**Files Modified**:
- [file] - [what changed]

**New Files**:
- [file] - [purpose]

---

## Testing Checklist

### Functional Tests
- [ ] **Test 1**: [Description]
  - Steps: [Instructions]
  - Expected: [Result]

### Edge Cases
- [ ] **Edge 1**: [Description]
  - Trigger: [How]
  - Expected: [Behavior]

### Regression Tests
- [ ] [Existing feature still works]

---

## Test Results

**Tested By**: [Name]
**Date**: YYYY-MM-DD

### Issues Found
| # | Description | Severity | Status |
|---|------------|----------|--------|
| 1 | [Issue]    | High/Med/Low | Open/Fixed |

---

## Sign-off

- [ ] All tests passed
- [ ] No regressions
- [ ] Ready to push

**Approved**: [ ] Yes / [ ] No
```

### 3.3 User Approval

**Mandatory before push:**

1. Notify user that changes are ready
2. Provide testing checklist link
3. **Wait for explicit approval**
4. If issues found → fix → return to 3.1
5. Only push after "approved" or equivalent confirmation

---

## Phase 4: DOCUMENT

### 4.1 Required Documentation Updates

Before marking complete:

| Item | Action |
|------|--------|
| TODO.md | Remove task |
| DONE.md | Add task with implementation details |
| Plan document | Mark complete, add discoveries |
| PROJECT_CONTEXT.md | Add architectural decisions |
| CLAUDE.md | Add new patterns/policies if applicable |

### 4.2 Plan Document Completion

**CRITICAL: The plan document must be updated CONTINUOUSLY throughout task execution, not just at completion.**

Add to plan document:

```markdown
## 4. Key Discoveries

**Technical Insights**:
- [Insight with explanation]

**Architectural Decisions**:
- [Decision: why made, alternatives considered]

**Patterns Identified**:
- [Pattern: where applies, benefits]

**Anti-Patterns Avoided**:
- [Anti-pattern: why problematic]

---

## 5. Future Improvements ⚠️ MANDATORY

### After Each Sub-Item (Record Immediately in Plan File)

| Field | Description |
|-------|-------------|
| Results obtained | What was concretely achieved |
| Lessons learned | Insights gained |
| Problems encountered | Issues and resolutions |
| Improvements identified | What could be better (minimum 1) |
| Technical debt noted | Shortcuts taken |
| Related code | Other areas needing similar changes |

### At Task Completion

**Enhancement Ideas** (minimum 2):
- [Idea 1]: [Rationale, estimated effort, priority]
- [Idea 2]: [Rationale, estimated effort, priority]

**Technical Debt Identified**:
- [Debt item]: [Why it exists, impact, remediation approach]

**Performance Optimizations**:
- [Optimization]: [Current state, potential improvement, effort]

**Code Quality Improvements**:
- [Improvement]: [Where applies, benefit]

**Related Tasks Spawned**:
- [Task]: [Relationship to this work, add to TODO.md if actionable]

**Questions for Future Investigation**:
- [Question]: [Why it matters, when to revisit]
```

### 4.3 Improvement Tracking Workflow

**CRITICAL: Improvements must propagate to TODO.md**

After documenting improvements in plan document:

1. **Evaluate each improvement** — Is it actionable? Worth doing?
2. **Add to TODO.md** — Create task entry for accepted improvements
3. **Tag with origin** — Reference the plan document that spawned it
4. **Prioritize** — Assign priority based on impact/effort

Example TODO.md entry:
```markdown
### [ID] - [Improvement from Task X]
**Origin**: docs/plans/YYYY-MM-DD_original-task.md
**Priority**: Medium
**Spawned from**: [Original task description]
**Description**: [What to improve]
```

### 4.4 Code Documentation

For non-obvious code:

```python
# Manual testing fix (YYYY-MM-DD): Handle edge case where...
# This check is needed because...
if edge_case:
    special_handling()
```

### 4.5 Memory Persistence

**Before marking complete, update persistent memory:**

#### Create New Entities

| Discovery | Memory Action |
|-----------|---------------|
| Architecture decision | Create Decision entity |
| New reusable pattern | Create Pattern entity |
| User preference learned | Create/update Preference entity |
| Important file created | Create File entity |

#### Update Existing Entities

| Change | Memory Action |
|--------|---------------|
| New fact about existing entity | `add_observations()` |
| Entity behavior changed | Add timestamped observation |
| Pattern applied successfully | Add "Applied in: [project]" observation |

#### Delete Outdated Information

| Trigger | Memory Action |
|---------|---------------|
| File/code was deleted | `delete_entities()` for File entities |
| Decision was reversed | Add `DEPRECATED:` prefix or delete |
| Observation proven wrong | `delete_observations()` |
| Relation no longer valid | `delete_relations()` |

**Memory update checklist:**
- [ ] New decisions documented in memory?
- [ ] New patterns captured in memory?
- [ ] User preferences updated?
- [ ] Relations created between entities?
- [ ] **Outdated observations removed?**
- [ ] **Obsolete entities deleted?**

---

## Phase 5: COMPLETE

**This phase executes AFTER user approval of manual testing.**

### 5.1 Task Completion Documentation Sequence

**Execute in this exact order:**

```
1. EXTRACT    → Improvements from plan → BACKLOG.md / TODO.md
2. ARCHIVE    → Move plan to docs/archive/plans/
3. TRANSITION → Move task from TODO.md → DONE.md
4. COMMIT     → Git commit all changes
5. MEMORY     → Update knowledge graph
```

#### Step 1: Extract Improvements to BACKLOG.md

**⚠️ REQUIREMENT: Plan MUST have minimum 2 documented improvements before this step.**

If fewer than 2 improvements exist → STOP → Add more improvements to the plan first.

1. **Verify minimum 2 improvements exist** in plan Section 5
2. **Review all documented improvements**
3. **Categorize each improvement**:
   - `ACTIONABLE` → Add to TODO.md with priority
   - `IDEA` → Add to BACKLOG.md for future consideration
   - `SKIP` → Document reason in plan, don't add anywhere
4. **Format for BACKLOG.md**:
   ```markdown
   ### [YYYY-MM-DD] From: [task-name]
   **Origin**: docs/archive/plans/YYYY-MM-DD_task-name.md

   - [ ] [Improvement 1] — [brief rationale]
   - [ ] [Improvement 2] — [brief rationale]
   ```

#### Step 2: Archive Completed Plan

1. **Verify completeness**:
   - [ ] All sections filled
   - [ ] Status set to "Complete"
   - [ ] "Key Discoveries" documented
   - [ ] "Future Improvements" has minimum 2 items
2. **Move to archive**:
   ```bash
   mv docs/plans/YYYY-MM-DD_task-name.md docs/archive/plans/
   ```
3. **Clean up**: Delete `/root/.claude/plans/` copy if exists

#### Step 3: Transition Task TODO.md → DONE.md

**In TODO.md**: Remove the task entry completely

**In DONE.md**: Add with format:
```markdown
### [YYYY-MM-DD] [Task Title]

**Plan**: [docs/archive/plans/YYYY-MM-DD_task-name.md](docs/archive/plans/YYYY-MM-DD_task-name.md)
**Summary**: [1-2 sentence description]
**Key Changes**:
- [Change 1]
- [Change 2]
**Spawned Tasks**: [count] items added to TODO.md/BACKLOG.md
```

#### Step 4: Commit Documentation Changes

**Single commit for all planning documentation:**

```bash
git add docs/planning/TODO.md docs/planning/DONE.md docs/planning/BACKLOG.md
git add docs/archive/plans/YYYY-MM-DD_task-name.md
git commit -m "docs: Archive completed plan and update planning docs

- Archive: YYYY-MM-DD_task-name.md
- Move task from TODO.md to DONE.md
- Add [N] improvements to BACKLOG.md

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>"
```

#### Step 5: Update Memory

**Persist learnings to knowledge graph:**

| Discovery Type | Memory Action |
|----------------|---------------|
| New decision | `create_entities()` → Decision entity |
| New pattern | `create_entities()` → Pattern entity |
| Task completion | `add_observations()` → Update task entity status |
| Obsolete info | `delete_observations()` or `delete_entities()` |

**Memory checklist:**
- [ ] Decisions recorded in memory?
- [ ] New patterns captured?
- [ ] Task status updated?
- [ ] Stale observations cleaned?
- [ ] Relations updated?

### 5.2 Final Verification Checklist

- [ ] All tests passing
- [ ] Pre-commit hooks passing
- [ ] User approved manual testing
- [ ] Improvements extracted to BACKLOG.md/TODO.md
- [ ] Plan archived to `docs/archive/plans/`
- [ ] Task moved from TODO.md to DONE.md
- [ ] Documentation commit created
- [ ] Memory updated with learnings
- [ ] Code pushed to repository

### 5.3 Git Workflow for Feature Commits

```bash
# Stage changes
git add [files]

# Commit with descriptive message
git commit -m "[type]: [description]

[Body explaining what and why]

Refs: #[issue] or TODO.md X.Y.Z

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>"

# Push after user approval
git push origin [branch]
```

Commit types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`

---

## Type Safety Requirements

### Static Checking

**All new code should have type hints** (language-specific syntax applies).

**Before commit:** Run type checker (mypy, tsc, etc.) on modified files.

**No type-ignore comments without documented justification.**

### Runtime Validation

For public APIs and external data, use validation libraries appropriate for your language:
- Python: pydantic, attrs
- TypeScript: zod, io-ts
- Other: language-appropriate equivalents

---

## Code Reuse Policy

### Before Writing New Code

1. **Search for existing implementations**
2. **Evaluate reusability** — Can existing code be used or generalized?
3. **Prefer composition** over duplication

### When to Write New

Only when:
- Thorough search found nothing
- Existing code too specialized to generalize
- Reusing would create bad coupling

---

## Emergency Procedures

### Rollback

If deployed changes cause issues:

```bash
# Revert to previous commit
git revert HEAD

# Or reset to known good state
git reset --hard [good-commit]
git push --force origin [branch]  # Use with caution
```

### Hotfix

For critical production issues:

1. Branch from main: `git checkout -b hotfix/[name]`
2. Minimal fix only
3. Expedited testing (critical paths only)
4. Direct merge to main
5. Document in incident report

---

*See [../CLAUDE.md](../CLAUDE.md) for thinking protocol and analysis requirements.*
*See [../PROJECT.md](../PROJECT.md) for project-specific commands and configuration.*
