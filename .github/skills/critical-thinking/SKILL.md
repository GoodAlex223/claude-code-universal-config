---
name: critical-thinking
description: Deep analysis protocol with EXPLORE-CHALLENGE-SYNTHESIZE phases for non-trivial coding tasks. Use when facing complex problems, architecture decisions, or when you need to evaluate multiple approaches before implementing.
---

# Critical Thinking Protocol

Apply this protocol before implementing non-trivial solutions. Never accept requests at face value - always analyze first.

---

## Phase 1: EXPLORE

Before writing any code:

| Step | Action | Output |
|------|--------|--------|
| 1 | Restate the problem | Written summary in own words |
| 2 | Generate alternatives | Minimum 3 different approaches |
| 3 | Identify blind spots | "What am I missing?" list |
| 4 | Map edge cases | Minimum 5 edge cases |
| 5 | Search existing code | Similar implementations found |

---

## Phase 2: CHALLENGE

Stress-test your proposed solution:

| Question | Purpose |
|----------|---------|
| "What would break this?" | Find failure modes |
| "What assumptions am I making?" | Expose hidden dependencies |
| "How would this fail at 10x scale?" | Check scalability |
| "What if the user's assumption is wrong?" | Validate requirements |
| "Is there a simpler way I dismissed?" | Avoid over-engineering |

---

## Phase 3: SYNTHESIZE

Present findings before proceeding:

```markdown
## Analysis Summary

**Problem**: [Restated in own words]

**Approaches**:
1. [A] — Pros: ... / Cons: ...
2. [B] — Pros: ... / Cons: ...
3. [C] — Pros: ... / Cons: ...

**Assumptions**:
- [List each assumption explicitly]

**Edge Cases**:
| Case | Handling |
|------|----------|
| [1]  | [How handled] |

**Recommendation**: [Choice] because [reasoning]

**Risks**: [Key risks and mitigations]
```

---

## Areas Requiring Critical Analysis

### Architecture Decisions

Questions to ask:
- Is this the right pattern for this problem?
- Will this scale to 10x users/data?
- Does this introduce unnecessary coupling?
- Does this match existing codebase patterns?

Red flags: Global state, tight coupling, circular dependencies, god classes

### Algorithm Choices

Questions to ask:
- What's the time complexity? Space complexity?
- Are there edge cases that change behavior?
- Does a simpler O(n) solution exist?

Red flags: Nested loops on unbounded data, recursive calls without depth limits

### Type Safety

Questions to ask:
- Are types properly constrained?
- Can invalid states be represented?
- Are boundary conditions handled?

Red flags: `Any`/`unknown` without justification, optional without null checks

### Code Structure

Questions to ask:
- Does this follow SOLID principles?
- Is this DRY?
- Will this be maintainable in 6 months?

Red flags: Functions > 50 lines, classes > 7 methods, > 5 parameters

---

## Divergent Thinking Triggers

Before finalizing ANY solution, force these perspectives:

| Perspective | Question |
|-------------|----------|
| Pessimist | "How will this fail?" |
| Maintainer | "Will I understand this in 6 months?" |
| Reviewer | "What would a senior engineer critique?" |
| User | "Does this actually solve the problem?" |
| Ops | "How will this behave in production?" |
| Skeptic | "Is the user's premise correct?" |

---

## Common Cognitive Traps

| Trap | Problem | Fix |
|------|---------|-----|
| First Solution Bias | Implementing first idea | Mandate 3 alternatives |
| Confirmation Bias | Seeking supporting evidence | Actively seek counter-examples |
| Complexity Bias | Assuming complex is better | Always ask "Is there a simpler way?" |
| Authority Bias | Accepting assertions without question | Validate all assumptions |
| Sunk Cost Fallacy | Continuing bad approach | Evaluate objectively; pivot if needed |

---

## Analysis Depth by Task Type

| Task Type | Depth | Alternatives Required |
|-----------|-------|----------------------|
| Bug fix | Medium | 2 |
| New feature | High | 3+ |
| Refactoring | High | 3+ |
| Configuration | Low | 1 |
| Documentation | Low | 1 |
| Architecture change | Very High | 5+ |
| Security-related | Very High | 3+ |

---

## Questioning User Assertions

| User Says | Check |
|-----------|-------|
| "This is simple" | Is it actually simple? What's the scope? |
| "Just do X" | Is X the right approach? |
| "It worked before" | Has context changed? |
| "Everyone does it this way" | Is that actually best practice? |
| "We don't need tests" | What are the risks? Push back. |

---

## Constructive Challenge Pattern

```markdown
I want to make sure I understand correctly. You're asking for [X], which would [consequences].

Before I proceed, I'd like to explore:

1. **Alternative**: Have you considered [Y]? It would [benefits].
2. **Risk**: I notice [potential issue]. How should we handle that?
3. **Clarification**: When you say [term], do you mean [A] or [B]?

My recommendation is [approach] because [reasoning].

Would you like to proceed with your original approach, or explore the alternative?
```
