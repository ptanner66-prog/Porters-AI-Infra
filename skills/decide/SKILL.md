<!--
Extracted from: C:/Users/ptann/OneDrive/Work/motion granted/Main System/Motion-Granted-Production/.claude/skills/decide/SKILL.md
Original purpose: Surfaces pending decisions that need operator input.
Genericized: 2026-04-21
Operator: Porter Tanner
-->

---
name: decide
description: >
  Surfaces pending decisions that need operator input. Scans progress / status docs,
  CLAUDE.md, conversation context, git state, and codebase for unresolved choices, blocked
  work, and tradeoffs. Presents each decision with context and a recommended course of action.
disable-model-invocation: true
context: fork
allowed-tools: Bash(grep *) Bash(rg *) Bash(cat *) Bash(git *) Read Grep Glob
---

# DECIDE — Pending Decision Finder

Scan the project for decisions that need operator input.

## Sources

### 1. Progress / status documentation
```bash
grep -n "Blocked\|blocked\|BLOCKED\|pending\|PENDING\|decision\|TBD" PROGRESS.md 2>/dev/null
grep -n "Blocked\|pending\|decision\|TBD" STATUS.md 2>/dev/null
```

### 2. CLAUDE.md and rule files
```bash
grep -rn "pending\|PENDING\|decision\|TBD\|sign-off\|reconcil" CLAUDE.md .claude/rules/ 2>/dev/null
```

### 3. Git state
```bash
git log --oneline -20
git branch -a
git stash list
```
Look for: branches needing reconciliation, stashed work, divergent branches.

### 4. Code TODOs
```bash
grep -rn "TODO\|FIXME\|HACK\|DECIDE" --include="*.ts" --include="*.tsx" --include="*.py" src/ | head -30
```

### 5. Uncommitted changes
```bash
git diff --name-only
```
Check changed files for decision points: feature flags, commented alternatives, TODO markers.

## Output Format

```
╔══════════════════════════════════════════════════╗
  PENDING DECISIONS
╚══════════════════════════════════════════════════╝

DECISION 1: {title}
  SOURCE: {file:line or section}
  CONTEXT: {plain English}
  OPTIONS:
    A) {option} — {tradeoff}
    B) {option} — {tradeoff}
  MY RECOMMENDATION: {which and why}
  BLOCKED WORK: {what's waiting on this}
  URGENCY: HIGH / MEDIUM / LOW
```

Always include a recommendation. The operator wants your opinion, not just a list.
Sort by urgency: HIGH first.
