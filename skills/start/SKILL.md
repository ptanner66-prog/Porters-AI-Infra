<!--
Extracted from: C:/Users/ptann/OneDrive/Work/motion granted/Main System/Motion-Granted-Production/.claude/skills/start/SKILL.md
Original purpose: Session-start shortcut/alias.
Genericized: 2026-04-21
Operator: Porter Tanner
-->

---
name: start
description: >
  Session start shortcut. Displays git state, branch safety check, dev-server status.
  Alias for /session-start.
disable-model-invocation: true
allowed-tools: Bash(git *) Bash(curl *) Read
---

# Session Start Briefing

## Pre-Loaded State

**Branch:**
!`git branch --show-current`

**Server status** (edit URL for project):
!`curl -s -o /dev/null -w "%{http_code}" --connect-timeout 2 http://localhost:3000 2>/dev/null || echo "DOWN"`

**Recent commits:**
!`git log --oneline -5`

**Working tree:**
!`git status --short`

**Changed files:**
!`git diff --stat`

## Task

Present a concise briefing from the pre-loaded state above.

Branch safety: feature branch = ✓. main / master / LIVE / Production = 🚨 DANGER.

Flag: uncommitted danger-zone files, dead-code file edits, suspected line-ending issues.

Surface top 3 inviolable rules from the project's rule files.

Keep it tight. Full protocol in [session-start/SKILL.md](../session-start/SKILL.md).
