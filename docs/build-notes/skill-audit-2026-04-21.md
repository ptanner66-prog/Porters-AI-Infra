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

---

## Actions taken (post-execution record)

**Commits by step:**

| Step | Commit | Summary |
|---|---|---|
| Step 3–6 patch | `ed18f07` | Added `skills:` preloads to all 4 sub-agents; added Swarm Orchestration section (architect) and Focus Modes section (chen); added `memory: project` to each. |
| Step 9 | `1ec7705` | Sharpened 12 Category B skill descriptions with "Use PROACTIVELY" / "Use when" auto-invocation language. |
| Step 10 | `d8630c6` | Added alias notes to grep-verify, session-start, session-end bodies. Deleted `skills/gv/`, `skills/start/`, `skills/end/`, `skills/sql/`. |

**Final `skills/` inventory: 25 directories**

- Category A (sub-agent-owned, 8): `chen`, `audit-deep-subsystem`, `audit-finding-expansion`, `audit-spec-to-code-delta`, `audit-pre-launch-failure`, `pr`, `grep-verify`, `swarm`
- Category B (main-session auto-invoke, 16): `100`, `adverse`, `code`, `decide`, `diagnose`, `gitnexus-cli`, `gitnexus-debugging`, `gitnexus-exploring`, `gitnexus-guide`, `lossy`, `session-end`, `session-start`, `shutup`, `soft`, `st`, `zero`
- Scaffold (1): `_template`

**Sub-agent skills: frontmatter bindings (verified):**

- architect → `swarm`
- chen → `chen`, `audit-deep-subsystem`, `audit-finding-expansion`, `audit-spec-to-code-delta`, `audit-pre-launch-failure`
- code-reviewer → `pr`
- grep-verifier → `grep-verify`

Every preloaded skill name resolves to a real `skills/<name>/SKILL.md` directory — no orphans, no placeholders.

**Coverage guarantee:** every directory in the final `skills/` inventory is either (a) preloaded in a sub-agent's `skills:` frontmatter OR (b) has `Use PROACTIVELY` / `MUST BE USED` / `Use when` in its `description` field. `_template` is exempt as a scaffold.

---

## v0.3.1 Step 4 orphan re-check (2026-04-22)

Re-verified after the v0.3.1 description-sharpening pass (commits `f011743`, `594423f`):

- OWNED (8): `chen`, `audit-deep-subsystem`, `audit-finding-expansion`, `audit-spec-to-code-delta`, `audit-pre-launch-failure` (→ chen); `pr` (→ code-reviewer); `grep-verify` (→ grep-verifier); `swarm` (→ architect).
- AUTO (16): `100`, `adverse`, `code`, `decide`, `diagnose`, `gitnexus-cli`, `gitnexus-debugging`, `gitnexus-exploring`, `gitnexus-guide`, `lossy`, `session-end`, `session-start`, `shutup`, `soft`, `st`, `zero` — all have "Use PROACTIVELY" / "Use when" openers with explicit trigger phrases. `gitnexus-debugging` and `gitnexus-exploring` now tool-availability-gated; `swarm` now marked architect-invoked-only.
- SCAFFOLD (1): `_template`.
- **Orphans: 0.** Total: 25 directories, all accounted for.

---

## v0.3.1 Category B additions (2026-04-22)

Three new main-session skills added to close dry-run trace gaps #5, #10, #15 from [adversarial-cleanup-2026-04-22.md](adversarial-cleanup-2026-04-22.md):

| Skill | Status | Commit | Purpose |
|---|---|---|---|
| `triage` | Added v0.3.1 | `b99c908` | Structured option comparison when user asks "help me decide between A and B" (Trace #5 fix). |
| `finish` | Added v0.3.1 | `1669424` | Commit-and-close-out discipline when user signals "commit and finish", "ship it", etc. (Trace #10 fix). |
| `summarize` | Added v0.3.1 | `a4c4dff` | Document compression when user says "summarize", "tldr", "compress this" (Trace #15 fix). |

**Updated totals:**
- Category A (sub-agent-owned, preloaded): **8** (unchanged)
- Category B (main-session auto-invoke): **19** (was 16 + 3)
- Scaffold: **1** (unchanged)
- **Total `skills/` directories: 28** (was 25 + 3)

Each new skill's description includes explicit `DO NOT USE for X — that is the Y skill` carve-outs to prevent auto-routing overlap with the sibling skill it was confused with in the original trace:

- `triage` carves out from `decide` (pending project decisions vs. user-presented option triage).
- `finish` carves out from `pr` (main-session commit vs. code-reviewer-sub-agent PR submission).
- `summarize` carves out from `lossy` (document compression vs. pipeline data-loss debugging) and from `shutup` (summary with preservation vs. silencing).

**Coverage guarantee (post-v0.3.1):** every directory in `skills/` remains either (a) preloaded in a sub-agent's `skills:` frontmatter OR (b) has `Use PROACTIVELY` / `MUST BE USED` / `Use when` in its description. Zero orphans.
