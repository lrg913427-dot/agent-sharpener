---
name: to-issues
description: >
  Break plans, specs, or PRDs into independently-grabbable issues using
  tracer-bullet vertical slices. Classifies as HITL (needs human) or AFK
  (agent-executable). Creates GitHub issues with dependency ordering.
  Use when user wants to convert a plan into issues, create implementation
  tickets, break down work, or says "break this into issues" / "make issues".
---

# To Issues

Break a plan into independently-grabbable issues using vertical slices (tracer bullets).

## Process

### 1. Gather context

Work from conversation context. If user passes an issue reference (number, URL, path), fetch it via `gh issue view`.

### 2. Explore codebase (optional)

If not already explored, understand current state. Issue titles should use project's established terminology.

### 3. Draft vertical slices

Each issue is a **thin vertical slice** cutting through ALL layers end-to-end, NOT a horizontal slice of one layer.

**Rules:**
- Each slice delivers a narrow but COMPLETE path (schema → API → UI → tests)
- A completed slice is demoable/verifiable on its own
- Prefer **many thin slices** over few thick ones
- Each slice classified as **HITL** or **AFK**

**HITL vs AFK:**
- **HITL** — requires human interaction (design review, architectural decision, manual testing, external access)
- **AFK** — can be implemented and merged without human interaction
- Prefer AFK over HITL where possible

### 4. Present breakdown

Show as numbered list. For each slice:

```
N. [Title]
   Type: HITL / AFK
   Blocked by: [other slice numbers] or None
   Covers: [user stories if available]
```

Ask user:
- Granularity right? (too coarse / too fine)
- Dependencies correct?
- Merge or split any?
- HITL/AFK classifications correct?

Iterate until approved.

### 5. Publish issues

For each approved slice, create GitHub issue via `gh issue create`:

```markdown
## Parent
[reference to parent issue if applicable]

## What to build
[concise description of vertical slice — end-to-end behavior, not layer-by-layer]

## Acceptance criteria
- [ ] [criterion 1]
- [ ] [criterion 2]
- [ ] [criterion 3]

## Blocked by
[blocking issue reference] or "None — can start immediately"

## Type
HITL / AFK
```

Apply `needs-triage` label. Publish in dependency order (blockers first) so you can reference real issue numbers in "Blocked by".

Do NOT close or modify any parent issue.

## Hermes-specific adaptations

- Use `gh issue create` and `gh issue view` for GitHub integration
- AFK issues can be directly fed to kanban workers or cron jobs
- Use `delegate_task` to parallelize issue creation for large breakdowns
- Works with any GitHub repo — no setup skill required
