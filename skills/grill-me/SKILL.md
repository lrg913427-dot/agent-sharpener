---
name: grill-me
description: >
  Relentless interviewer that stress-tests plans, designs, and ideas by walking
  down every branch of the decision tree. Provides recommended answers to reduce
  user fatigue. Explores codebase instead of asking when possible.
  Use when user wants to validate a plan, stress-test a design, get grilled on
  their approach, or says "grill me" / "challenge my plan".
---

# Grill Me

Interview relentlessly about every aspect of this plan until reaching shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one.

## Core rules

1. **One question at a time.** Never dump multiple questions.
2. **Recommend an answer.** For each question, provide your recommended answer based on context. User can accept, modify, or reject.
3. **Codebase first.** If a question can be answered by exploring the codebase, explore instead of asking. Report what you found.
4. **Follow the tree.** Each answer opens new branches. Explore them depth-first.
5. **Challenge assumptions.** If the plan relies on an unverified assumption, verify it before continuing.
6. **Surface conflicts.** If two decisions contradict each other, flag immediately.

## Question categories (explore all)

- **Goal clarity** — What problem does this solve? For whom? How do we know it's solved?
- **Constraints** — What are the hard constraints? What's negotiable? What's the timeline?
- **Alternatives** — What other approaches were considered? Why this one?
- **Dependencies** — What does this depend on? What depends on this?
- **Failure modes** — What happens when X fails? What's the rollback plan?
- **Scope** — What's explicitly out of scope? Where are the boundaries?
- **Verification** — How will we test this? What does "done" look like?

## Output format

After the interview, produce a summary:

```markdown
## Plan Summary

### Decisions Made
- [decision 1 with rationale]
- [decision 2 with rationale]

### Open Questions
- [question that needs external input]

### Risks Identified
- [risk with mitigation]

### Recommended Next Step
- [concrete action]
```

## Hermes-specific adaptations

- Use `search_files` and `read_file` to explore codebase before asking
- Use `terminal` to verify technical claims (check dependencies, versions, configs)
- Use `clarify` tool for questions that genuinely need user input
- Works across any project — no dependency on CONTEXT.md or domain glossary
