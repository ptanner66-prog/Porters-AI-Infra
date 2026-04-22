<!--
Generated during: v0.3.1 adversarial cleanup pass
Purpose: Audit every sub-agent and skill description for auto-invocation clarity and overlap risk. Trace 17 routing simulations against current (v0.3.0) descriptions to find mis-routes. Drives Steps 2–12 fixes.
Date: 2026-04-22
Operator: Porter Tanner
-->

# v0.3.1 Adversarial Cleanup Audit

## Trigger-Clarity and Overlap Audit (Step 1)

Columns:
- **Trigger clarity**: SHARP (specific phrases + "Use PROACTIVELY"/"MUST BE USED" opener) · VAGUE (generic) · MISSING (no auto-invocation language)
- **Overlap risk**: NONE · WITH `<asset>` (mis-route candidates)
- **Action needed**: OK · SHARPEN (language tune-up) · DISAMBIGUATE (carve out from sibling) · REWRITE

### Sub-agents

| Asset | Type | Trigger clarity | Overlap risk | Action needed |
|---|---|---|---|---|
| `architect` | sub-agent | SHARP | NONE (swarm skill overlaps but is preloaded, not auto-invoked in parallel) | OK |
| `chen` | sub-agent | SHARP | WITH code-reviewer (via "review this code"); WITH grep-verifier (via "audit" vs "verify" ambiguity) | **DISAMBIGUATE** (Step 2a + 2b) |
| `code-reviewer` | sub-agent | SHARP | WITH chen (broad "review"); WITH code skill (WIP vs submitted diff) | **DISAMBIGUATE** (Step 2a + 2c) |
| `grep-verifier` | sub-agent | SHARP | WITH chen (audit-finding-expansion semantic drill-down) | **DISAMBIGUATE** (Step 2b) |

### Category A — Sub-agent-owned skills (preloaded; don't need aggressive auto-invocation)

| Asset | Type | Trigger clarity | Overlap risk | Action needed |
|---|---|---|---|---|
| `chen` | skill | VAGUE (no "Use when" opener — preloaded) | NONE (preloaded) | OK |
| `audit-deep-subsystem` | skill | SHARP ("Use when auditing...") | NONE (preloaded) | OK |
| `audit-finding-expansion` | skill | SHARP ("Use when...") | NONE (preloaded) | OK |
| `audit-spec-to-code-delta` | skill | SHARP ("Use when...") | NONE (preloaded) | OK |
| `audit-pre-launch-failure` | skill | SHARP ("Use before launch...") | NONE (preloaded) | OK |
| `pr` | skill | VAGUE (workflow description, no explicit trigger opener) | NONE (preloaded) | OK — preloaded context is passive |
| `grep-verify` | skill | VAGUE (protocol description) | NONE (preloaded) | OK — preloaded context is passive |
| `swarm` | skill | VAGUE — description could cause main-session auto-fire on "parallel" when architect should handle it | **WITH architect sub-agent** | **SHARPEN** — add explicit "invoke only from within architect sub-agent; do not auto-fire main-session" |

### Category B — Main-session auto-invoke skills

| Asset | Type | Trigger clarity | Overlap risk | Action needed |
|---|---|---|---|---|
| `100` | skill | SHARP ("Use PROACTIVELY when stakes are high...") | NONE (differentiator vs /zero, /soft present) | OK |
| `adverse` | skill | SHARP ("Use PROACTIVELY when 'what am I missing'...") | NONE (explicit disambiguation from code-reviewer and diagnose) | OK |
| `code` | skill | SHARP | WITH code-reviewer (if user says "check this code" during WIP) | **SHARPEN** — add explicit "WIP only; do not use for submitted diffs — that is code-reviewer" (Step 2c pairing) |
| `decide` | skill | SHARP ("Use PROACTIVELY when 'what's blocking me'...") | NONE | OK |
| `diagnose` | skill | SHARP ("Use PROACTIVELY when user reports an error...") | WITH gitnexus-debugging (if GitNexus is available) | OK — gitnexus-debugging will be scoped to "when GitNexus is available" (see below) |
| `gitnexus-cli` | skill | SHARP ("Use when the user needs to run GitNexus CLI commands...") | NONE (explicit tool reference) | OK |
| `gitnexus-debugging` | skill | SHARP but too general | **WITH diagnose** (both fire on "why is this failing") | **DISAMBIGUATE** — scope to "when GitNexus tooling is available" |
| `gitnexus-exploring` | skill | SHARP but too general | **WITH general code exploration** / architect DISCOVER | **DISAMBIGUATE** — scope to "when GitNexus tooling is available" |
| `gitnexus-guide` | skill | SHARP ("Use when the user asks about GitNexus itself...") | NONE (meta-question about the tool) | OK |
| `lossy` | skill | SHARP ("Use PROACTIVELY when 'data is mysteriously degraded'...") | NONE | OK |
| `session-end` | skill | SHARP ("Use PROACTIVELY at end of a working session...") | NONE (temporal trigger) | OK |
| `session-start` | skill | SHARP ("Use PROACTIVELY at the start of a new working session...") | NONE (temporal trigger) | OK |
| `shutup` | skill | SHARP ("Use when user explicitly says 'stop talking'...") | NONE | OK |
| `soft` | skill | SHARP ("Use PROACTIVELY when 'no guessing'...") | NONE (differentiator vs /100 present) | OK |
| `st` | skill | SHARP ("Use PROACTIVELY when 'stress test'...") | NONE (differentiator vs /adverse, chen MODE 4 present) | OK |
| `zero` | skill | SHARP ("Use PROACTIVELY when 'no false positives'...") | NONE (differentiator vs /100, /soft present) | OK |

### Scaffold

| Asset | Type | Note |
|---|---|---|
| `_template` | scaffold | Exempt — not an invokable skill; used by operators as `cp -r skills/_template skills/<new>`. |

## Summary of actions needed

**Sub-agent descriptions to fix (3 overlaps flagged by v0.3.0 halt):**
- Fix 2a — "review this" overlap: `chen` + `code-reviewer` descriptions must carve submitted-change vs. existing-code boundary.
- Fix 2b — "verify X" overlap: `grep-verifier` + `chen` (finding-expansion) descriptions must carve textual vs. semantic boundary.
- Fix 2c — `code-reviewer` aggressiveness: scope to prepared diffs / PRs / committed reviews. Pair with `code` skill = WIP handler.

**Skill descriptions to fix (3 identified):**
- `code` — add WIP-only boundary (pairs with Fix 2c).
- `gitnexus-debugging` — scope to "when GitNexus is available" to avoid competing with `diagnose`.
- `gitnexus-exploring` — scope to "when GitNexus is available" to avoid competing with general code exploration / architect DISCOVER.
- `swarm` — mark as architect-invoked-only; do not main-session auto-fire.

(Other descriptions are SHARP with differentiators already present; no action needed.)

## Dry-run trace placeholder

17 routing simulations to be populated in Step 9 after all fixes applied.
