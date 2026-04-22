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

## Step 5 sub-agent preload verification (2026-04-22)

All four sub-agent `skills:` frontmatter lists were checked against skill references in their bodies:

- **architect** → preloads `swarm`. Body references the preloaded `swarm` skill in its Swarm Orchestration section. ✓ complete.
- **chen** → preloads `chen`, `audit-deep-subsystem`, `audit-finding-expansion`, `audit-spec-to-code-delta`, `audit-pre-launch-failure`. Body's Focus Modes section references each preloaded mode-specific skill by name. ✓ complete.
- **code-reviewer** → preloads `pr`. Body references `/pr` in the single-mode description. ✓ complete.
- **grep-verifier** → preloads `grep-verify`. Body has no other skill references. ✓ complete.

**No preload corrections needed.** All skill names resolve 1:1 to existing `skills/<name>/SKILL.md` directories.

## Step 9 dry-run routing simulation (2026-04-22)

Traced 17 natural-language task signals against the current (post-Step 2 and Step 3) sub-agent and skill descriptions.

| # | Signal | Expected (per operator spec) | Actual routing w/ current descriptions | OK? | Notes |
|---|---|---|---|---|---|
| 1 | "Audit the phase-executor subsystem for failure modes" | chen (audit-deep-subsystem) | chen — matches "audit"; no diff so code-reviewer carve-out blocks; no textual claim so grep-verifier blocks. Chen's Focus Modes table picks audit-deep-subsystem from "audit a subsystem for failure modes" signal. | ✓ | Clean route. |
| 2 | "Review this PR diff" | code-reviewer | code-reviewer — "review this PR", "review this diff" are exact trigger phrases. chen's "DO NOT USE for reviewing proposed changes, diffs, or PRs" carve-out blocks chen. | ✓ | Clean route. Fix 2a confirmed. |
| 3 | "Verify foo.ts no longer imports bar" | grep-verifier | grep-verifier — exact trigger phrase ("does foo.ts still import bar"). chen's MODE 2 carve-out ("first-pass textual verification — that is grep-verifier") blocks chen. | ✓ | Clean route. Fix 2b confirmed. |
| 4 | "What am I missing on this plan" | adverse skill | adverse — exact trigger phrase. No sub-agent matches ("plan" is not code). | ✓ | Clean route. |
| 5 | "Help me decide between option A and B" | decide skill | **decide skill description explicitly says "Not for deciding between options the user just presented — this surfaces decisions the project has already stalled on."** So decide does NOT fire on this signal. Actual routing: main session handles conversationally (no specific skill). | ✗ | **Mismatch between operator expected routing and actual decide-skill purpose.** The decide skill is for surfacing pending project decisions, not choosing between user-presented options. Flagged in halt report. Description is correct; operator's expectation was mistaken. |
| 6 | "Why is this test failing" | diagnose skill | diagnose — "Use PROACTIVELY when user reports ... test failure" exact match. If GitNexus is available, gitnexus-debugging also fires (scoped to "AND GitNexus tooling is available"); both are compatible — diagnose runs the 4-phase workflow, gitnexus-debugging provides structural-query support within it. | ✓ | Clean route; conditional gitnexus-debugging is complementary, not competing. |
| 7 | "Refactor these 15 files to use the new API — do it in parallel" | architect (swarm orchestration) | architect — "parallel", "multi-file refactor" are exact trigger phrases. swarm skill's "DO NOT auto-fire from main-session 'parallel'/'bulk task' signals; those route to architect first" carve-out blocks direct swarm fire. Architect then invokes preloaded swarm skill. | ✓ | Clean route. |
| 8 | "Extract the methodology from this repo into a reusable template" | architect (extraction) | architect — "extract", "methodology extraction" are exact trigger phrases. | ✓ | Clean route. |
| 9 | "I'm starting a new session" | session-start skill | session-start — "starts a chat after a gap" trigger phrase matches. | ✓ | Clean route. |
| 10 | "Commit and finish what you started" | 100 or soft skill (operator asked to clarify) | **Neither 100 nor soft fires on this signal.** 100 is about 100%-certainty rigor; soft is about banning soft conclusions. Neither has "commit" / "finish" triggers. Actual routing: if framed as "open a PR and finish", the pr skill (preloaded in code-reviewer) handles the PR-submission workflow. If framed as "just commit current work", main session commits directly. | ✗ | **Operator-expected routing doesn't match actual skill purposes.** 100 and soft are rigor-discipline skills, not commit/finish handlers. No skill in the current inventory specifically handles "commit and finish" as a unified signal. Flagged in halt report. |
| 11 | "Check this code I'm working on" (WIP review) | code skill, NOT code-reviewer | code skill — "check this code I'm working on" is the exact trigger phrase (added in Fix 2c pairing). code-reviewer's "DO NOT USE for every WIP edit during active development — the code skill handles quick main-session review of work-in-progress" carve-out blocks code-reviewer. | ✓ | Clean route. Fix 2c confirmed. |
| 12 | "Pre-launch check on the payment flow" | chen (audit-pre-launch-failure) | chen — "pre-launch check" is an exact chen trigger phrase. chen's Focus Modes table picks audit-pre-launch-failure for "pre-launch check" and "before release" signals. | ✓ | Clean route. |
| 13 | "Stress test this function" | st skill, NOT chen | st — "stress test this" is the exact trigger phrase. chen has no "stress test" trigger; its MODE 4 is "pre-launch failure audit", not stress testing. st's description differentiates from chen MODE 4 explicitly. | ✓ | Clean route. |
| 14 | "Run the audit-fix-build workflow" | chen for audit phase, architect for build phase | Multi-phase workflow invocation. Main session reads [workflows/audit-fix-build.md](workflows/audit-fix-build.md) and orchestrates. Phase 1 AUDIT auto-delegates to chen. Phase 3 BUILD uses main session unless the build involves extraction/scaffolding (architect). pr skill (code-reviewer preload) handles the PR-submission tail. | ✓ | Workflow orchestration, not single-sub-agent trace. Sub-agents fire per phase via natural auto-delegation from workflow step descriptions. |
| 15 | "Summarize this long document" | lossy skill | **lossy does NOT fire.** lossy is "Hunts design-level lossy transformations in pipelines — silent field drops, truncations, type narrowing, scale mismatches, JSON round-trips that lose keys" — i.e. debugging silent data loss in CODE, not document summarization. Actual routing: main session summarizes conversationally (no specific skill). | ✗ | **Operator-expected routing doesn't match actual lossy-skill purpose.** lossy is a debugging/audit skill for pipelines, not a document compressor. The name "lossy" is evocative of compression but the skill is about tracing data loss bugs. Flagged in halt report. Description is correct; operator's expectation was mistaken. |
| 16 | "Review this" (diff present vs not) | trace both cases | Case A (diff/PR/changeset visible): code-reviewer — "presents an explicit diff / changeset / commit for review" matches. Case B (no diff, existing code in scope): chen — "audits of existing code in the repo" matches. Case C (mid-edit WIP): code skill — "check this code I'm working on" matches. Descriptions' mutual DO-NOT-USE carve-outs route cleanly per context. Residual ambiguity for bare "review this" with no context: operator may still need to halt and ask. | ✓ | Descriptions disambiguate as well as signal allows. Residual ambiguity in context-free cases is inherent to the signal, not a description bug. |
| 17 | "Verify this" (textual vs semantic) | trace both cases | Case A (textual: file-contents / phrase-presence check): grep-verifier — "verify the change landed" matches; chen's carve-out blocks chen. Case B (semantic drill-down on prior chen finding): chen (MODE 2) — "drilling into an existing chen finding, not one-shot pattern checks" matches; grep-verifier's carve-out blocks grep-verifier. Bare "verify this" without context is ambiguous — depends on whether a prior chen finding exists in session. | ✓ | Descriptions disambiguate as well as signal allows. Clarification gate (§3.5.5) handles irreducible ambiguity. |

**Summary:** 14 of 17 traces route cleanly. 3 flagged (#5, #10, #15) are cases where the operator's expected routing does not match the actual skill's purpose — the skill descriptions correctly reflect what the skill does, but the operator's mental model for `decide` / `100` / `soft` / `lossy` was off. These are not description bugs; rewriting the descriptions to match the operator's expectations would misrepresent the skills' actual behavior.

Recommendation: operator reviews the three flagged traces and decides whether to (a) accept current skill purpose (flagged cases fall through to main-session conversational handling), (b) add a new skill to handle the signal (e.g. a `summarize` skill for document compression), or (c) broaden a skill's purpose (and rewrite both description and body).

## Step 10 cross-cutting consistency audit (2026-04-22)

All checks pass — no fixes applied.

| Check | Result |
|---|---|
| Four sub-agent `color` fields unique (blue/orange/green/cyan) | ✓ architect=blue, chen=orange, code-reviewer=green, grep-verifier=cyan |
| All four sub-agent `memory: project` | ✓ all four |
| Every sub-agent's `tools` list includes Read + Glob + Grep | ✓ all four |
| Chen's tools list excludes Write and Edit (audit-only) | ✓ `Read, Grep, Glob, Bash` |
| Architect's tools list includes Write and Edit | ✓ `Read, Write, Edit, Glob, Grep, Bash` |
| No sub-agent frontmatter contains `effort:` or `maxTurns:` | ✓ none found (only name/description/tools/model/color/memory/skills) |
| Every sub-agent body contains halt / stop criteria | ✓ architect (§ Authority and Halt Rules), chen (halt on irreducible mode ambiguity + audit-only refusal), code-reviewer (Stop review on P0, BLOCKED verdict), grep-verifier (four-tier verdicts, does-not-self-promote) |
| Chen body retains "Expected invocation posture: max effort, long runs (design for up to ~80 turns)" | ✓ line 29 |
| Architect body retains equivalent "~60 turns" posture guidance | ✓ line 23 |
| code-reviewer and grep-verifier have their own posture guidance | ✓ both at "~30 turns" (line 23 in each) |
| Provenance headers on all four extracted Chen audit-mode skills | ✓ all four include `Extracted from: prompts/superprompts/chen-audit-protocol.md § AUDIT MODES — MODE N` |

## Step 12 final verification (2026-04-22)

All v0.3.1 fixes applied and verified:

| Check | Result |
|---|---|
| `.claude/agents/` has 4 files with valid YAML frontmatter | ✓ architect, chen, code-reviewer, grep-verifier |
| Every sub-agent `skills:` entry resolves to real `skills/<name>/SKILL.md` | ✓ swarm, chen, 4×audit-*, pr, grep-verify all present |
| Every `skills/` directory is OWNED (preloaded) or AUTO (main-session with auto-invoke language) or SCAFFOLD (_template) | ✓ 8 / 16 / 1 = 25 total, zero orphans |
| No two skill descriptions have substantial overlap that would cause mis-routing | ✓ three v0.3.0 flags resolved in Step 2; four v0.3.1 flags (swarm / gitnexus-debugging / gitnexus-exploring / code-vs-code-reviewer boundary) resolved in Step 3 |
| `personas/*.md` all have REFERENCE DOC header + synced descriptions | ✓ all four (Step 11 sync) |
| CLAUDE.md and README.md reflect current `.claude/agents/` + `skills/` inventory | ✓ Steps 6 + 7 cleanup |
| All 17 dry-run traces route correctly (or flagged with reasoning) | ✓ 14 clean + 3 flagged as operator mental-model mismatches (not description bugs) |
| No sub-agent frontmatter contains `effort: max` or `maxTurns` | ✓ Step 10 — body-level posture guidance only |
| All four extracted Chen audit-mode skills have provenance headers | ✓ all four |
| `.claude/settings.json` is language-agnostic | ✓ Step 8 stripped npx tsc hint and hardcoded npm / port defaults from launch.json |
| Zero slash references to skill names in active docs | ✓ CLAUDE.md, README.md, AGENTS.md, workflows/\*.md, docs/methodology.md, docs/architecture.md, LEARNINGS.md, BACKLOG.md, .claude/agents/\*.md all clean. VERIFICATION.md preserved as v0.2 historical snapshot with top-of-file disclaimer. docs/build-notes/ preserved unedited. Remaining `/command` references are real commands in `.claude/commands/` not in the operator's skill-name ban list. |

## Summary

The three specific overlaps flagged in the v0.3.0 halt report are resolved with mutually-exclusive DO-NOT-USE carve-outs in each sub-agent description. Four additional overlaps identified during Step 1 audit (swarm skill, gitnexus-debugging, gitnexus-exploring, code-vs-code-reviewer boundary) are also resolved. All main-session skill descriptions carry explicit trigger phrases. Zero skill orphans. Zero frontmatter drift. Zero slash-skill-name references in active user-facing docs.

**Flagged for operator attention (not description bugs):**
- Dry-run traces #5, #10, #15 expose mismatches between the operator's expected routing and the actual skill purposes for `decide` (surfaces project-pending decisions, not user-option triage), `100` / `soft` (rigor-mode skills, not commit/finish handlers), and `lossy` (pipeline data-loss debugger, not document summarizer).
- VERIFICATION.md still holds v0.2-era slash references; preserved as a historical record with a top-of-file disclaimer pointing to current convention.
- Operator listed `/swarm` and `/max` in the slash-ban grep list, but both remain real commands in `.claude/commands/`. Documentation prose no longer uses slash-prefix for them; the command files themselves are unchanged.

Tag-ready at HEAD as v0.3.1.

---

## Post-script: v0.3.1 Trace-gap closures (2026-04-22)

After the Step 12 halt flagged traces #5, #10, #15 as operator-expectation / skill-purpose mismatches, the operator requested the gaps be closed by adding three new skills rather than accepting fall-through to main-session conversational handling. Three new Category B skills landed:

| Commit | Skill | Closes | Description opener |
|---|---|---|---|
| `b99c908` | [triage](../../skills/triage/SKILL.md) | Trace #5 | `Use PROACTIVELY when user says "help me decide between", "which should I pick", "A or B", "what's better", or presents 2+ explicit options for comparison.` |
| `1669424` | [finish](../../skills/finish/SKILL.md) | Trace #10 | `Use PROACTIVELY when user says "commit and finish", "wrap this up", "close this out", "finish what you started", "ship it", or signals end-of-task closure on in-progress work.` |
| `a4c4dff` | [summarize](../../skills/summarize/SKILL.md) | Trace #15 | `MUST BE USED when user says "summarize this document", "compress this", "give me the short version", "tldr this", or pastes long content and asks for a condensed version.` |

### Re-trace confirmation

| # | Signal | Expected | Actual routing (v0.3.1 post-fix) | OK? |
|---|---|---|---|---|
| 5 | "Help me decide between option A and B" | triage | triage — phrase contains both "help me decide between" and "A or B" explicit trigger phrases. decide description explicitly carves out ("NOT for deciding between options the user just presented"). | ✓ |
| 10 | "Commit and finish what you started" | finish | finish — phrase contains both "commit and finish" and "finish what you started" explicit trigger phrases. pr description scoped to "open a PR / submit this"; 100/soft/zero are rigor modes with orthogonal triggers. | ✓ |
| 15 | "Summarize this long document" | summarize | summarize — phrase contains "summarize this document" explicit trigger phrase. lossy description scoped to "data mysteriously degraded / values dropped between stages"; shutup scoped to silencing signals. Both have explicit carve-outs in summarize's description. | ✓ |

### New-overlap-risk check

Each new skill description carries explicit `DO NOT USE for X — that is the Y skill` carve-outs paired with the sibling it was originally confused with:

| New skill | Carve-out pairs | Sibling trigger (confirmed non-overlap) |
|---|---|---|
| `triage` | `decide` | decide fires on "what's blocking me" / "where are we stuck" — distinct from option-comparison signals |
| `finish` | `pr` | pr fires on "open a PR" / "submit this" — distinct from commit-and-close-out signals |
| `finish` | `100` / `soft` / `zero` | rigor modes fire on "no margin for error" / "no guessing" / "no false positives" — orthogonal to commit/finish |
| `summarize` | `lossy` | lossy fires on pipeline-data-loss debugging signals — distinct from document-compression |
| `summarize` | `shutup` | shutup fires on "stop talking" / silencing signals — distinct from compression-with-preservation |

No new overlaps introduced. All three re-traces route cleanly.

### Final state

- `skills/` inventory: **28** directories (Category A: 8; Category B: 19; Scaffold: 1). Zero orphans.
- Sub-agent descriptions and skill descriptions are all SHARP with explicit triggers and carve-outs.
- All original v0.3.0 + v0.3.1 overlap flags are resolved.
- CLAUDE.md §4 and README.md architecture diagram both updated with the three new skills.
- Skill audit doc (`skill-audit-2026-04-21.md`) carries the v0.3.1 addition record.

Tag-ready at HEAD as v0.3.1.
