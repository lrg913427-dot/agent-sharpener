# Agent Sharpener

Engineering skill pack for AI coding agents. Battle-tested patterns distilled from real-world software engineering practices.

## Skills

### 🔧 diagnose
Disciplined 6-phase debugging loop. Build feedback loop → reproduce → hypothesise → instrument → fix → post-mortem. **Build the right loop and the bug is 90% fixed.**

### 🎯 grill-me
Relentless interviewer that stress-tests plans and designs. Provides recommended answers, explores codebase instead of asking when possible.

### 🏷️ triage
Issue triage state machine. Categorize, prioritize, generate durable agent briefs. No file paths or line numbers that go stale.

### 📋 to-issues
Break plans into vertical slices. HITL vs AFK classification. Creates GitHub issues with dependency ordering.

### 🦴 caveman
Ultra-compressed mode. ~75% fewer tokens. Auto-reverts for safety.

## Installation

### Hermes Agent
```bash
cp -r skills/* ~/.hermes/skills/
```

### Claude Code
```bash
ln -sfn $(pwd)/skills/diagnose ~/.claude/skills/diagnose
ln -sfn $(pwd)/skills/grill-me ~/.claude/skills/grill-me
ln -sfn $(pwd)/skills/triage ~/.claude/skills/triage
ln -sfn $(pwd)/skills/to-issues ~/.claude/skills/to-issues
ln -sfn $(pwd)/skills/caveman ~/.claude/skills/caveman
```

## Philosophy

- **Vertical slices over horizontal layers**
- **Deep modules** (Ousterhout) — small interface, deep implementation
- **HITL vs AFK** — separate human work from agent work
- **Durable outputs** — no file paths or line numbers
- **Feedback loops first**

## License

MIT
