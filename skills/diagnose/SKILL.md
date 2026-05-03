---
name: diagnose
description: >
  Disciplined 6-phase diagnosis loop for hard bugs and performance regressions.
  Build feedback loop → reproduce → hypothesise → instrument → fix → post-mortem.
  Use when user reports a bug, says something is broken/failing/crashing, describes
  a performance regression, or says "diagnose this" / "debug this".
---

# Diagnose

Disciplined debugging for hard problems. Skip phases only when explicitly justified.

## Phase 1 — Build a feedback loop

**This IS the skill.** Everything else is mechanical. A fast, deterministic pass/fail signal = bug solved.

Spend disproportionate effort here. Be aggressive. Be creative. Refuse to give up.

### Loop strategies (try in order)

1. **Failing test** — unit, integration, or e2e at whatever seam reaches the bug
2. **Curl/HTTP script** against a running dev server
3. **CLI invocation** with fixture input, diff stdout against known-good snapshot
4. **Headless browser** (Playwright/Puppeteer) — drive UI, assert on DOM/console/network
5. **Replay captured trace** — save real request/payload to disk, replay in isolation
6. **Throwaway harness** — minimal subset of system, single function call
7. **Property/fuzz loop** — 1000 random inputs, look for failure mode
8. **Bisection harness** — `git bisect run` automation between known states
9. **Differential loop** — old vs new version, diff outputs
10. **HITL script** — last resort, structured script that drives human interaction

### Iterate on the loop itself

Treat the loop as a product:
- **Faster?** Cache setup, skip unrelated init, narrow scope
- **Sharper signal?** Assert on specific symptom, not "didn't crash"
- **More deterministic?** Pin time, seed RNG, isolate filesystem

A 30s flaky loop ≈ no loop. A 2s deterministic loop = debugging superpower.

### Non-deterministic bugs

Goal: **higher reproduction rate**, not clean repro. Loop 100×, parallelize, add stress, narrow timing windows. 50%-flake = debuggable; 1% = not. Keep raising rate.

### Cannot build a loop?

Stop. Say so explicitly. List what you tried. Ask user for:
- Access to reproducing environment
- Captured artifact (HAR, log dump, core dump, screen recording)
- Permission for temporary production instrumentation

**Do NOT proceed without a loop.**

## Phase 2 — Reproduce

Run the loop. Watch the bug appear.

Checklist:
- [ ] Loop produces the **user's** described failure — not a nearby different one
- [ ] Failure reproducible across runs (or high-enough rate for non-deterministic)
- [ ] Exact symptom captured (error message, wrong output, timing)

## Phase 3 — Hypothesise

Generate **3–5 ranked hypotheses** BEFORE testing any. Single-hypothesis = anchoring bias.

Each must be **falsifiable**: "If <X> is the cause, then <changing Y> will make bug disappear / <changing Z> will make it worse."

If you can't state the prediction → it's a vibe → discard or sharpen.

**Show ranked list to user before testing.** They often re-rank instantly ("we just deployed a change to #3"). Cheap checkpoint, big time saver. Proceed if user is AFK.

## Phase 4 — Instrument

Each probe maps to a specific prediction. **One variable at a time.**

Tool preference:
1. **Debugger/REPL** — one breakpoint beats ten logs
2. **Targeted logs** at boundaries that distinguish hypotheses
3. Never "log everything and grep"

**Tag every debug log** with unique prefix `[DEBUG-a4f2]`. Cleanup = single grep.

**Perf branch:** For performance regressions, establish baseline measurement first (timing harness, profiler, query plan), then bisect. Measure first, fix second.

## Phase 5 — Fix + regression test

Write regression test **before the fix** — but only at a **correct seam** (test exercises real bug pattern at the call site).

If no correct seam exists → **that itself is the finding.** Note it. Architecture is preventing the bug from being locked down.

If correct seam exists:
1. Turn minimised repro into failing test
2. Watch it fail
3. Apply fix
4. Watch it pass
5. Re-run Phase 1 loop against original scenario

## Phase 6 — Cleanup + post-mortem

Required before declaring done:
- [ ] Original repro no longer reproduces (re-run Phase 1 loop)
- [ ] Regression test passes (or absence of seam documented)
- [ ] All `[DEBUG-...]` instrumentation removed
- [ ] Throwaway prototypes deleted
- [ ] Correct hypothesis stated in commit/PR message

**Then ask: what would have prevented this bug?** If architectural (no test seam, tangled callers, hidden coupling) → hand off to architecture improvement with specifics.

## Hermes-specific adaptations

- Use `terminal` for running tests, scripts, and reproduction loops
- Use `delegate_task` for parallel hypothesis testing when independent
- Use `search_files` for instrumentation cleanup (grep DEBUG tags)
- For long-running loops, use `terminal(background=true, notify_on_complete=true)`
