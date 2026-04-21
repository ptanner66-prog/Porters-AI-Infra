<!--
Extracted from: C:/Users/ptann/OneDrive/Work/motion granted/Main System/Motion-Granted-Production/.claude/skills/gv/SKILL.md
Original purpose: Grep-verify shortcut/alias.
Genericized: 2026-04-21
Operator: Porter Tanner
-->

---
name: gv
description: >
  Grep-verify shortcut. Fact-checks claims about the codebase with grep evidence.
  Alias for /grep-verify.
disable-model-invocation: true
context: fork
argument-hint: "[claim-to-verify]"
allowed-tools: Bash(grep *) Bash(rg *) Bash(find *) Bash(cat *) Bash(head *) Bash(tail *) Bash(wc *) Read Grep Glob
---

# Grep-Verify — Codebase Claim Verification

Verify this claim against actual source code using grep evidence.

**Claim**: $ARGUMENTS

Historical false positive rate in AI-assisted audits: 38–54%. Trust grep, not summaries.

## Protocol

1. Decompose the claim into testable assertions.
2. Grep for evidence of each assertion.
3. Check for dead-code traps (project-specific known-dead paths — auto-FALSE POSITIVE for live-system impact).
4. Rate each: CONFIRMED (grep evidence) / LIKELY (indirect) / INDETERMINATE (can't prove) / FALSE POSITIVE (contradicted).
5. Show exact grep commands and file:line output.
6. Never upgrade INDETERMINATE to CONFIRMED.

Full protocol in [grep-verify/SKILL.md](../grep-verify/SKILL.md).
