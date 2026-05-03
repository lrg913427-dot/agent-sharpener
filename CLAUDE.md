# Agent Sharpener

Engineering skill pack for AI coding agents.

## 技术栈
- Markdown SKILL.md 文件
- 兼容 Hermes Agent + Claude Code

## 结构
```
skills/
  diagnose/SKILL.md     — 调试循环
  grill-me/SKILL.md     — 连环追问
  triage/SKILL.md       — issue 分诊
  to-issues/SKILL.md    — 垂直切片分解
  caveman/SKILL.md      — 极简模式
```

## 部署
- Hermes: cp skills/* ~/.hermes/skills/
- Claude Code: ln -sfn 到 ~/.claude/skills/
- ClawHub: 5/9 后 clawhub publish

## 仓库
https://github.com/lrg913427-dot/agent-sharpener
