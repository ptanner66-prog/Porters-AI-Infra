# AGENTS.md — Agent Coordination Guide

<!-- provenance: genericized from Motion-Granted-Production/AGENTS.md + CLAUDE.md GitNexus section -->

This file governs how Claude Code sessions in this repo operate: mode selection, parallel agent rules, model routing, and GitNexus integration mandates.

---

## Mode Selection

Sessions operate in one of three modes. Declare your mode at the start of a session.

| Mode | Trigger | Behavior |
|------|---------|----------|
| **IMPLEMENT** | Default — active coding task | High-effort editing, verification after every change, GitNexus impact check before every symbol edit |
| **AUDIT** | `/chen`, `/adverse`, explicit request | Read-only investigation. No edits. Produce findings with evidence labels. Await operator approval before implementing. |
| **DISCOVERY** | New codebase, onboarding | Inventory mode. Read, map, document. No edits. Produce a structured report. |

**Mode-switching rules:**
- Switching from AUDIT → IMPLEMENT requires explicit operator approval ("proceed", "implement", "go ahead").
- DISCOVERY produces a written report before any mode switch.
- Never self-promote to IMPLEMENT from AUDIT without approval.

---

## Parallel Agent Rules

When spawning subagents (via `/swarm` or manual multi-agent use):

1. **Decompose by file boundary** — each agent owns a non-overlapping set of files.
2. **Read-only subagents** — subagents investigate and report; the main thread synthesizes and edits.
3. **Max 5 subagents per swarm** — context budget constraint.
4. **Each agent must complete independently** — no agent waits on another's output.
5. **Main thread merges** — conflicts between subagent findings are resolved by the main thread, not by the agents themselves.

---

## Model Routing

| Task type | Recommended model |
|-----------|------------------|
| Architecture decisions, hard debugging | Opus (max effort) |
| Standard implementation, code review | Sonnet (high effort) |
| Quick lookups, throwaway questions | Haiku |

Override model routing only when the operator explicitly requests it. Never downgrade the model for a task that crosses a pipeline boundary or touches inviolable rules.

---

## GitNexus Integration (Inviolable)

The following rules are mandatory whenever GitNexus is connected. Violations are treated as errors.

### Before editing any symbol
Run impact analysis:
```
gitnexus_impact({target: "symbolName", direction: "upstream"})
```
Report the blast radius to the operator. If risk is HIGH or CRITICAL, STOP and get approval.

### Before committing
Run change detection:
```
gitnexus_detect_changes({scope: "staged"})
```
Verify that only expected symbols and execution flows were modified.

### Before renaming anything
Run rename preview:
```
gitnexus_rename({symbol_name: "old", new_name: "new", dry_run: true})
```
Review every affected site. Then run with `dry_run: false`.

### Never
- NEVER edit a symbol without running `gitnexus_impact` first.
- NEVER ignore HIGH or CRITICAL risk warnings.
- NEVER rename with find-and-replace — always use `gitnexus_rename`.
- NEVER commit without running `gitnexus_detect_changes`.

---

## File Navigation

| Question | Tool |
|---------|------|
| "Where is X defined?" | `gitnexus_query({query: "X"})` |
| "What calls X?" | `gitnexus_context({name: "X"})` → upstream |
| "What does X call?" | `gitnexus_context({name: "X"})` → downstream |
| "What breaks if I change X?" | `gitnexus_impact({target: "X", direction: "upstream"})` |
| "What did I change?" | `gitnexus_detect_changes({scope: "staged"})` |
| "Show me the execution flow" | `READ gitnexus://repo/YOUR_REPO_NAME/process/{name}` |

---

## Inviolable Session Rules

1. **Branch check first** — `git branch --show-current` before any edit.
2. **Grep before trust** — never accept an audit finding without grep evidence.
3. **Dead code check** — verify the live path before editing any file.
4. **Impact before edit** — GitNexus impact analysis before modifying any symbol.
5. **Operator owns production** — never push to production branches without explicit operator instruction.
6. **Scope discipline** — out-of-scope findings go to BACKLOG.md, not inline fixes.
