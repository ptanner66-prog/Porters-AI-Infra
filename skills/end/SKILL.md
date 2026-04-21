<!--
Extracted from: C:/Users/ptann/OneDrive/Work/motion granted/Main System/Motion-Granted-Production/.claude/skills/end/SKILL.md
Original purpose: Session-end shortcut/alias.
Genericized: 2026-04-21
Operator: Porter Tanner
-->

---
name: end
description: >
  Session end shortcut. Generates handoff document capturing all changes, decisions,
  remaining work, and next steps. Alias for /session-end.
disable-model-invocation: true
allowed-tools: Bash(git *) Bash(grep *) Bash(wc *) Bash(cat *) Bash(find *) Read Grep Glob
---

# Session End — Handoff Document

Capture everything needed for the next session.

1. **Git state**: branch, last 10 commits, status, diff --name-only, diff --stat
2. **Changes made**: for each modified file — path, what changed, why, risk level (LOW/MEDIUM/HIGH)
3. **Decisions / work items touched**: ID, status (IMPLEMENTED/PARTIAL/DIAGNOSED), files affected
4. **Remaining work**: P0 (merge blockers), P1 (should fix), P2 (deferrable)
5. **Blockers**: humans waiting-on, external deps, architectural decisions, branch conflicts
6. **Next step**: single concrete action for the next session

Be honest about incomplete work. Flag any dead-code edits or inviolable-rule risks. Self-contained — a reader with no context should understand what happened.

Full protocol in [session-end/SKILL.md](../session-end/SKILL.md).
