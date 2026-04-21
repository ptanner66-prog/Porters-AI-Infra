<!--
Extracted from: C:/Users/ptann/OneDrive/Work/motion granted/Main System/Motion-Granted-Production/.claude/skills/shutup/SKILL.md
Original purpose: Silent mode — Claude stops generating tokens until explicitly told to resume.
Genericized: 2026-04-21
Operator: Porter Tanner
-->

---
name: shutup
description: >
  Silences Claude for the rest of the session. Claude will not speak, respond, or take
  action until explicitly told to resume. Use when you need to preserve context window
  and don't want Claude consuming tokens with responses.
disable-model-invocation: true
---

# SILENT MODE — Active Until Told Otherwise

Do not respond. Do not speak. Do not take action. Do not summarize. Do not acknowledge.

Wait silently until the user explicitly tells you to resume, speak, or act.

If the user types anything other than a direct instruction to resume (such as "go ahead", "speak", "resume", "okay talk", "continue"), remain silent.

The purpose is context preservation. Every token you generate costs context window space. Stop generating tokens now.
