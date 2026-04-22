<!--
Generated during: v0.3.0 sub-agent conversion (expanded spec)
Purpose: Categorize every skill in skills/ into A (sub-agent-owned), B (main-session), C (alias), or D (delete). Baseline from operator spec; content validated against each SKILL.md description.
Date: 2026-04-21
Operator: Porter Tanner
-->

# Skill Categorization Audit — v0.3.0

Every directory under `skills/` has been categorized into one of four categories (A / B / C / D) plus a `Scaffold` bucket for the `_template` directory which is not a real skill.

**Rule enforced:** every skill must be either (a) preloaded in a sub-agent's `skills:` frontmatter OR (b) have auto-invocation language (`Use PROACTIVELY` / `MUST BE USED` / `Use when ...`) in its description — no orphans.

## Summary counts

- **A — Sub-agent-owned:** 8 skills
- **B — Main-session (auto-invoke):** 16 skills
- **C — Alias (delete directory, note on canonical):** 3 skills
- **D — Delete (dead / MG-specific):** 1 skill
- **Scaffold (keep as-is):** 1 (`_template`)
- **Total directories in skills/:** 29

## Category A — Sub-agent-owned (preload via `skills:` frontmatter)

| Skill | Owner sub-agent | Reasoning |
|---|---|---|
| `chen` | chen | Canonical Chen persona activator; chen sub-agent needs this as the baseline protocol reference |
| `audit-deep-subsystem` | chen | Chen MODE 1 playbook (extracted from superprompt this session) |
| `audit-finding-expansion` | chen | Chen MODE 2 playbook (extracted from superprompt this session) |
| `audit-spec-to-code-delta` | chen | Chen MODE 3 playbook (extracted from superprompt this session) |
| `audit-pre-launch-failure` | chen | Chen MODE 4 playbook (extracted from superprompt this session) |
| `pr` | code-reviewer | PR preflight + submission; paired with code-reviewer per expanded spec |
| `grep-verify` | grep-verifier | Claim verification protocol; paired with grep-verifier per expanded spec |
| `swarm` | architect | Parallel-agent orchestration; architect owns swarm authority per expanded spec |

## Category B — Main-session (auto-invoke via Skill tool)

Invoked by the main agent via Claude Code's Skill-tool description matching. Each needs sharp auto-invocation language in its description (see Step 9 — description-sharpening pass).

| Skill | Current description snippet | Sharpen in Step 9? |
|---|---|---|
| `100` | "Enforces 100% certainty standard on all findings and fixes..." | YES — add "Use PROACTIVELY when..." opener |
| `adverse` | "Adversarial completeness check. Scores..." | YES |
| `code` | "Confirms with 100% certainty that code is actually connected..." | YES |
| `decide` | "Surfaces pending decisions that need operator input..." | YES |
| `diagnose` | "Structured diagnostic workflow for bugs and errors..." | YES |
| `gitnexus-cli` | "Use when the user needs to run GitNexus CLI commands..." | NO — already compliant |
| `gitnexus-debugging` | "Use when the user is debugging a bug..." | NO — already compliant |
| `gitnexus-exploring` | "Use when the user asks how code works..." | NO — already compliant |
| `gitnexus-guide` | "Use when the user asks about GitNexus itself..." | NO — already compliant |
| `lossy` | "Hunts for design-level lossy transformations in a pipeline..." | YES |
| `session-end` | "End-of-session handoff document generator..." | YES |
| `session-start` | "Session startup briefing..." | YES |
| `shutup` | "Silences Claude for the rest of the session..." | YES |
| `soft` | "Enforces hard-evidence-only debugging..." | YES |
| `st` | "Full stress test protocol..." | YES |
| `zero` | "Enforces Zero False Positive / Zero False Negative standard..." | YES |

## Category C — Alias (delete directory, note on canonical)

Each of these is a duplicate of its canonical skill. Plan: add a note to the top of the canonical skill's body ("Also invokable as: `<alias>`"), then delete the alias directory.

| Alias | Canonical | Action |
|---|---|---|
| `gv` | `grep-verify` | Note on grep-verify; delete `skills/gv/` |
| `start` | `session-start` | Note on session-start; delete `skills/start/` |
| `end` | `session-end` | Note on session-end; delete `skills/end/` |

## Category D — Delete

| Skill | Reasoning |
|---|---|
| `sql` | MG-specific: hard-codes Motion-Granted Supabase project ID (`odhgidurshcznbslviqo`) and MG table schemas. Not salvageable as generic. Memory confirms prior decision to skip during MODE 2 extraction; delete now rather than ship a broken skill. |

## Scaffold (keep as-is)

| Item | Reasoning |
|---|---|
| `_template` | Scaffolding directory for authoring new skills — not a skill itself; no invocation path needed. Copied by operators via `cp -r skills/_template skills/<new>`. |

## Note on the prior `max` entry

The operator's baseline categorization listed `max` as a main-session skill, but `skills/max/` does not exist. `max.md` is a **command** at `.claude/commands/max.md`, not a skill. No action needed in this audit; flagged for operator awareness.

## Execution plan produced by this audit

1. Step 3–6 patch: add `skills:` entries to each sub-agent's frontmatter per Category A mappings.
2. Step 9: sharpen the 12 Category B descriptions marked `YES`.
3. Step 10: apply Category C alias notes + delete alias directories; delete `skills/sql/`.
4. Step 12: move this file to `docs/build-notes/skill-audit-2026-04-21.md` per expanded spec.
