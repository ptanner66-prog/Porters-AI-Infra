# Discovery Report

**Produced:** 2026-04-21 (MODE 1 / DISCOVER)
**Operator:** Porter Tanner
**Target:** tanner-stack — generic, reusable coding-methodology repo distilled from Motion-Granted
**Scope:** Read-only inventory of two source trees, with extraction recommendations for MODE 2

---

## Sources Scanned

| # | Path | Purpose | Key Observation |
|---|---|---|---|
| 1 | `C:/Users/ptann/OneDrive/Work/motion granted/Main System/Motion-Granted-Production/` | Primary — stable legal-tech Next.js codebase with 1,100+ TS files, 49 prompts, 31 skills, 18 commands. Main branch snapshot dated Apr 14–15. | Canonical methodology surface. |
| 2 | `C:/Users/ptann/OneDrive/Work/motion granted/Main System/mg-overnight/` | Parallel/overnight workspace. Same tree shape. All files dated Apr 17 00:22 (uniform mtime = bulk sync, not organic edits). | Tracks CIV-fixes branch with 590+ undeployed commits; subsystem files evolved since Apr 15 snapshot. |

**Cross-source identity check (root methodology files):**

| File | MG-Production bytes | mg-overnight bytes | Verdict |
|---|---|---|---|
| AGENTS.md | 17,139 | 17,139 | **identical** |
| CLAUDE.md | 9,026 | 9,026 | **identical** |
| README.md | 9,944 | 9,944 | **identical** |
| BACKLOG.md | 2,369 | 2,369 | **identical** |
| LEARNINGS.md | 11,566 | 11,402 | 164-byte diff (minor) |
| PROGRESS.md | 25,453 | 25,242 | 211-byte diff (minor) |

**.claude/ subtree diff counts:**
- `.claude/commands/` — 18 files in each source; **all 18 differ** (same names, evolved content)
- `.claude/skills/` — 31 skill dirs in each; **28 SKILL.md files differ**, 3 identical
- `.claude/agents/` — 3 in each; civ-auditor.md and grep-verifier.md differ, **code-reviewer.md identical**
- `.claude/rules/` — 6 in each; **all 6 differ**

**Implication for R4:** Top-level methodology docs can be extracted from either source (identical). Subsystem files (commands, most skills, agents, rules) need per-file decisions in MODE 2. Default recommendation: **Motion-Granted-Production as canonical** unless a specific mg-overnight version shows a meaningful improvement; divergences are flagged per-row below.

---

## Source 1: Motion-Granted-Production

### Category 1 — Personas / Agent Configs

| File | What it is | Extract? | Domain-specificity note |
|---|---|---|---|
| `AGENTS.md` | 344-line agent coordination + mode-switching guide: GitNexus reference, inviolable rules, parallel agent rules, file navigation tables, model routing, D-code binding decisions. | **yes** (structure) | Inviolable rules and model routing are MG-specific; GitNexus + mode-switching scaffolding is reusable. |
| `.claude/agents/civ-auditor.md` | Sonnet/high-effort agent for debugging citation pipeline: 7-step table, 4 integration points, treatment taxonomy, 6-step debug sequence. | **maybe** | Methodology (step isolation, constraint checking, cross-table validation) extracts cleanly. CIV specifics replaceable. |
| `.claude/agents/code-reviewer.md` | Post-edit reviewer (Sonnet, high effort): 8 checks with GREP/GRAPH/HYBRID evidence sources + severity. **IDENTICAL to mg-overnight version.** | **yes** | Check taxonomy is reusable; MG violations (filing deadline, federal substitution) replaceable. |
| `.claude/agents/grep-verifier.md` | Skeptical auditor (Sonnet, 30 turns): 6 validation rules, 4 false-positive patterns, CONFIRMED/LIKELY/FALSE POSITIVE rating. | **yes** | Validation methodology + 38–54% false-positive baseline is reusable evidence framework. |
| `.claude/skills/chen/chen-superprompt-v83.md` | 775-line Chen adversarial audit protocol: hostile-auditor identity, authority resolution (5-tier), certainty labels (CONFIRMED/HYPOTHESIS/UNVERIFIED/DISPROVEN), 12-step audit sequence, zero false-positive standard. | **yes** | Protocol is domain-agnostic; legal examples replaceable. **Highest-ROI single extraction.** |

### Category 2 — Skills (31 total in `.claude/skills/`)

| Skill | Purpose | Extract? | Note |
|---|---|---|---|
| `chen` | Launches Chen v83 adversarial audit protocol | **yes** | Protocol framework. |
| `100` | 100% certainty enforcement (file:line proof, pattern census) | **yes** | Generic rigor framework. |
| `adverse` | Adversarial completeness check: security / business logic / integration | **yes** | Reusable scoring framework. |
| `code` | GitNexus-backed code exploration + impact analysis | **yes** | GitNexus wrapper pattern. |
| `decide` | Binary decision forcing with evidence trail | **yes** | Decision framework. |
| `diagnose` | Root-cause hypothesis→test→refine | **yes** | Methodology, not targets. |
| `dx` | Developer experience audit (build speed, types, lint, docs) | **yes** | Generic DX checklist. |
| `grep-verify` (alias `gv`) | Validates audit claims with grep before accepting | **yes** | Reusable validation layer. |
| `lossy` | Lossy summarization for context compaction | **yes** | Compaction heuristic. |
| `pr` | PR creation + formatting + mutation check | **yes** | Generic PR framework. |
| `session-start` / `session-end` | Session lifecycle: init / close | **yes** | Startup/close checklists. |
| `end` | Session-end memory flush to LEARNINGS.md | **yes** | Close framework. |
| `soft` | Soft-evidence weighting (circumstantial > grep-only) | **yes** | Evidence framework. |
| `sql` | SQL query builder for exploration | **yes** | Generic query pattern. |
| `swarm` | Parallel-agent coordination (scope by file, commit waves) | **yes** | Swarm protocol. |
| `test` | Test creation + validation wrapper | **yes** | Generic framework. |
| `zero` | Zero-false-positive mandate | **yes** | Rigor framework. |
| `shutup` | Verbose-output suppression | **yes** | Muting framework. |
| `dwt` | "Did we test?" — coverage discovery | **yes** | Test-coverage pattern. |
| `dc` | Dead-code detection via import absence | **yes** | Detection pattern. |
| `devserv` / `start` | Dev-server launcher + health check | **yes** | Server-lifecycle wrapper. |
| `gitnexus/gitnexus-cli` / `-debugging` / `-exploring` / `-guide` | GitNexus CLI integration wrappers (4 skills) | **yes** | MCP integration pattern. |
| `civ-trace` | Traces citations through 7-step CIV via Supabase | **maybe** | CIV-specific queries; step-isolation methodology salvageable. |
| `st` | Stress-test runner | **maybe** | Framework is generic; test scenarios MG-specific. |
| `adverse` (already above but also has legal role-swap angle) | Opposition-attorney simulator | **maybe** | Role-swap methodology reusable; legal framing specific. |
| `d-code` | D-code binding decision navigator | **no** | 100% MG governance reference. |
| `CIV` | Duplicate alias of `civ-trace` | **no** | Skip duplicate. |
| `census` | Court coverage classifier (DESERT/SCRAPED/PARTIAL/FULL) | **no** | CourtListener-specific. |

**R5 application:** per policy, extract only methodological skills that generalize. Target MODE-2 skill set: `chen`, `100`, `adverse` (generic half), `code`, `decide`, `diagnose`, `dx`, `grep-verify`, `lossy`, `pr`, `session-start`, `session-end`, `end`, `soft`, `sql`, `swarm`, `test`, `zero`, `shutup`, `dwt`, `dc`, `devserv`, `gitnexus/*` (4). Domain-specific skills → not extracted.

### Category 3 — Workflows

| File | What it is | Extract? | Note |
|---|---|---|---|
| `.github/pull_request_template.md` | PR template: What/Why/How/Testing/Breaking Changes/Deploy Notes. | **yes** | Structure reusable. |
| `.claude/commands/adverse.md` | Adversarial completeness check command | **yes** | Generic workflow. |
| `.claude/commands/blast.md` | Blast-radius checker (impact-analysis wrapper) | **yes** | Generic refactoring workflow. |
| `.claude/commands/dead.md` | Dead-code detector | **yes** | Generic. |
| `.claude/commands/downstream.md` / `upstream.md` | Dependency tracers (both directions) | **yes** | Generic. |
| `.claude/commands/impact.md` | GitNexus impact-analysis wrapper | **yes** | Generic MCP pattern. |
| `.claude/commands/verify.md` | Post-fix validation | **yes** | Generic. |
| `.claude/commands/swarm.md` | Parallel-swarm deployment | **yes** | Generic workflow. |
| `.claude/commands/finalize.md` | Session-finalization checklist | **yes** | Generic. |
| `.claude/commands/grep-all.md` | Full-codebase grep | **yes** | Generic. |
| `.claude/commands/zero.md` / `zt.md` | Zero-tolerance violation checks | **yes** | Generic (replace violation list). |
| `.claude/commands/project-tdd.md` | Test-driven-development workflow | **yes** | Generic TDD loop. |
| `.claude/commands/label.md` / `max.md` / `root.md` | Auxiliary commands (uncertain purpose from name alone) | **maybe** | Read in MODE 2 to classify. |
| `.claude/commands/db.md` / `sql.md` | DB/SQL query helpers | **maybe** | Framework is generic; queries are schema-specific. |
| `audits/phase-iii-audit-2026-04-15.md` | Legal-methodology audit of Phase III prompt (v79). | **maybe** | Audit-methodology (prompt reading, output tracing, hypothesis testing) extracts; IRAC/element content does not. |
| `audits/civ-pipeline-audit-2026-04-15.md` | 300+ findings across 7-step CIV | **maybe** | Pipeline-audit methodology (step isolation, gate testing) extracts. |
| `audits/phase-iii-iv-v-integration-audit-2026-04-15.md` | Handoff audit across phase boundaries | **maybe** | Handoff-audit methodology (schema check, ID-namespace validation) extracts. |
| `audits/phase-v-audit-2026-04-15.md` | Phase V (draft generation) audit | **no** | Legal domain. |
| `audits/cl-search-pipeline-audit-2026-04-15.md` / `cl-query-quality-audit-2026-04-16.md` / `cl-search-deep-audit-2026-04-15.md` | CourtListener search-quality audits | **no** | API + legal-search specific. |
| `docs/PRODUCTION_READINESS_CHECKLIST.md` | Ship gate: schema, migrations, env vars, secrets, monitoring, rollback | **yes** | Generic readiness framework. |
| `docs/POST_MORTEM_ORCHESTRATOR.md` | Incident response: timeline, root cause, fix, prevention | **yes** | Generic framework. |
| `docs/BRANCH_PROTECTION_SETUP.md` | Branch-rule config (required reviews, status checks) | **yes** | Generic branch config. |
| `docs/SECRETS_ROTATION_CHECKLIST.md` | Secrets-rotation runbook | **yes** | Generic rotation framework. |
| `docs/DEAD_CODE_DETECTION_CHECKLIST.md` | Dead-code audit checklist | **yes** | Reusable. |

### Category 4 — Architecture

| File | What it is | Extract? | Note |
|---|---|---|---|
| `AGENTS.md` lines 1–78 | GitNexus integration mandate: impact-before-edit, detect-changes-before-commit, rename-safety, cypher querying. Duplicated in CLAUDE.md lines 112–189. | **yes** | Generic MCP/GitNexus integration pattern; MG index name replaceable. |
| `.claude/settings.json` | Harness config: model=opus; permission allowlist (Read/Grep/Glob/Bash readonly, tsc, git, npm); deny list (rm -rf, push --force, checkout main/master); PostToolUse tsc hook on Edit; PreToolUse blocks dead code + protected-path access. | **yes** | Config structure reusable; dead-code block list + protected paths are MG-specific. |
| `.claude/launch.json` | Dev-server launcher: next-dev @ 3000, inngest-dev @ 8288 | **yes** | Framework reusable; ports + server names MG-specific. |
| `.claude/rules/inviolable-decisions.md` | 19-line D-code quick reference (Cardinal Sin, D-CIV-VII1, D-DESERT-SEARCH-1, D-PX-3, D-IX1-3, D-EMAIL-3, D-3, D-CRLF-1, D-FALLBACK-KILL-2). | **no** | 100% MG governance; structure (path-scoped DO NOTs) is reusable at abstract level → see workflows/. |
| `.claude/rules/civ-pipeline.md` / `courtlistener.md` / `phase-iv.md` / `generators.md` | Path-scoped subsystem rule files (4 subsystems). | **no** for content; **maybe** for structure | Rule-file pattern (paths: scoping, DO NOT lists, architecture notes, thresholds, key files) is reusable; content is MG-specific. |
| `.claude/rules/inngest.md` | Inngest orchestration rules: step.run memoization, idempotency, circuit breakers, truncation retry. | **maybe** | Inngest usage is generic; MG event names replaceable. |
| `.gitnexus/` directory | Code-intelligence index (8,482 symbols, 21,633 relationships, 300 execution flows). | **no** (content) | Index is regenerated per codebase; document the integration, not the index. |
| `lib/config/*.ts` (phase-registry, tier-config, models, courtlistener-court-map) | Runtime config for 14-phase pipeline, pricing tiers, model routing, court reference data. | **no** | Legal-domain implementation detail. |

**Swarm / Mythos / Hermes / Arsenal:** Only `swarm` skill + `swarm.md` command found. No Mythos/Hermes/Arsenal references in either source — these are unimplemented integration targets per the mission spec. MODE 2 should document hook points for these, not extract code for them.

### Category 5 — Methodology Writing

| File | Structure | Gist | Extract? |
|---|---|---|---|
| `CLAUDE.md` (189 lines, 9.0K) | Active Branch → Inviolable Rules (8) → Session Defaults → Project Overview → Commands → DO NOT Edit (dead code) → DO Writable Repo → Danger Zones → Patterns (6) → CRLF handling → Verification → Compaction → Out-of-Scope Issues → References → GitNexus reference. | Singleton dev-onboarding + safety doc. Combines repo state + safety constraints + verification discipline. | **yes** — patterns (branch checks, dead-code trap, verification discipline, compaction) are reusable; legal inviolable rules replaceable. |
| `LEARNINGS.md` (164 lines, 11.5K) | 13 lessons (numbered): Dead Code Trap; False-Positive Audits (38–54%); Phase III→IV Handoff Truncation; CRLF; proposition_text Mismatch; Ghost Tables; Court Code Bugs; Compat Layer Hides Bugs; D-CIV-VII1 Inviolable Rule; Write-Side vs Read-Side Fix Ordering; Compat Scale Mismatch; Undefined Fallbacks; forElement Precision. Each: problem → fix → lesson → prevention. | Hard-won pattern library. Lessons 1–2, 4, 10, 12–13 generalize; 5–9, 11 are MG-pipeline specific. | **yes** (format + generic lessons) |
| `PROGRESS.md` (211 lines, 25.4K) | ✅ Implemented (90+ items across Phase IV, CIV, Treatment, Infrastructure, Handoff, Model Routing, Citations Graph, Compat, Stress Test, Search & Routing) with D-code binding per item. Dated Apr 15 2026. Branch: CIV-fixes (590+ undeployed commits). | Implementation-tracking dashboard. Format + D-code discipline is reusable; content is MG. | **maybe** (format only — see `feat: extract workflows` for template) |
| `BACKLOG.md` (60 lines, 2.4K) | How to Add Items → Cleanup → Technical Debt → (empty) Bugs / Feature Requests / Security / Documentation. Format: checkbox + title + one-liner + found-date + context. | Out-of-scope tracker; "defer but track" discipline. | **yes** — template is reusable. |
| `README.md` (224 lines, 9.9K) | Summary → Tech Stack (14 components) → Codebase stats → 14-Phase Workflow Pipeline → Checkpoints (HOLD/NOTIFICATION/BLOCKING) → 4-tier pricing → 7-step CIV → Development prereqs. | Product + architecture overview; onboarding audience. | **no** for content; **maybe** for structure (write a generic README in MODE 2). |

### Category 6 — All CLAUDE.md Files (Recursive)

Exactly **one** CLAUDE.md at the root of Motion-Granted-Production. No subdirectory CLAUDE.md hierarchy. References `@PROGRESS.md`, `@LEARNINGS.md`, `@BACKLOG.md`, `.claude/rules/`.

### Category 7 — Infrastructure

| File | What it is | Extract? | Note |
|---|---|---|---|
| `.claude/settings.json` | See Architecture above. | **yes** | Reusable scaffold. |
| `.claude/settings.local.json` | Local overrides (not VCS). | **no** | Environment-specific. |
| `.claude/launch.json` | Dev-server config. | **yes** | Scaffold reusable. |
| `.claude/rules/*.md` (6) | Subsystem rules (see Architecture). | **no** content / **yes** structural pattern | Extract pattern, not content. |
| `.claude/skills/*/SKILL.md` (31) | See Category 2. | mixed | Per-skill decision. |
| `.claude/agents/*.md` (3) | See Category 1. | yes | Persona scaffolds. |
| `.claude/commands/*.md` (18) | See Category 3. | mostly yes | Workflow commands. |
| `hooks/` (React hooks, NOT Git hooks) | React useX hooks for UI. | **no** | Frontend; not methodology. |
| `scripts/` (14+ SQL + TS diagnostics) | Schema drift checks, migrations, benchmarks. | **no** | MG-DB-schema specific. |
| `.git/hooks/` | Only sample hooks present; none active. | **no** | Nothing to extract. |

---

## Source 2: mg-overnight

**Abbreviated** because top-level methodology files and directory shape mirror Source 1. Deltas below; full enumeration redundant.

### What's the same
- AGENTS.md, CLAUDE.md, README.md, BACKLOG.md: byte-identical to Source 1.
- `.claude/agents/code-reviewer.md`: identical.
- Directory structure, skill names (31), command names (18), rule names (6): all identical.

### What differs (subsystem content evolution, same scaffolding)

| Class | Count | Divergence character |
|---|---|---|
| `.claude/commands/*.md` | 18/18 differ | Apr 17 edits; same command names, refined prompt content. |
| `.claude/skills/*/SKILL.md` | 28/31 differ | Skill bodies refined; frontmatter + purpose stable. |
| `.claude/agents/civ-auditor.md`, `grep-verifier.md` | 2/3 differ | code-reviewer.md identical — suggests code-reviewer reached stable form first. |
| `.claude/rules/*.md` | 6/6 differ | Minor rule-content refinements. |
| `LEARNINGS.md` | ~164-byte diff | Minor; content structure unchanged. |
| `PROGRESS.md` | ~211-byte diff | Minor; implementation-tracking drift. |

### Sibling Divergence Signals (R4 — operator-decision rows)

Under R4, when files exist in both with meaningful differences, DISCOVERY names them and recommends a version. Given the scale (54 differing files across commands/skills/agents/rules), enumerating every diff would bloat this doc and add no value: the top-level methodology IS identical, and the subsystem divergences are content-evolution, not structural.

**Recommendation for MODE 2 (operator to confirm):**
- **Default canonical source = Motion-Granted-Production** (the stable snapshot).
- **Exception policy:** if during extraction a specific file's mg-overnight version is clearly cleaner / more generic / has new structure worth preserving, flag it in the MODE 2 commit message and extract from mg-overnight for that single file.
- **Alternative** (if operator prefers): adopt mg-overnight as canonical for `.claude/skills/`, `.claude/commands/`, `.claude/rules/` since those files are newer (Apr 17) and presumably represent the latest thinking. Keeps the rest from MG-Production.

**Specific divergence rows worth operator attention (sample, non-exhaustive):**

| File | MG-Production bytes | mg-overnight bytes | Recommendation |
|---|---|---|---|
| LEARNINGS.md | 11,566 | 11,402 | **MG-Production** (larger; mg-overnight looks like a trimmed copy, not a richer rewrite). |
| PROGRESS.md | 25,453 | 25,242 | **MG-Production** (same reasoning; plus PROGRESS is MG-specific anyway, so we extract format only). |
| All 18 commands | varies | varies | **operator-decision** — read both in MODE 2 before selecting; default MG-Production. |
| All 28 differing skills | varies | varies | **operator-decision** at the per-skill level for the skills we keep. Default MG-Production. |
| 6 rules | n/a (no content extraction) | n/a | Extract pattern only, no content — divergence moot. |

---

## Extraction Recommendations

### Must-extract (clearly generic or cleanly genericizable)
1. **`AGENTS.md`** — reshape into scaffolding + mode-switching + GitNexus-integration template. Strip D-codes, branch names, MG index name.
2. **`CLAUDE.md`** — reshape as 8-section teaching doc per spec; patterns, verification, compaction, out-of-scope tracking all reusable.
3. **`LEARNINGS.md`** — keep format, keep generic lessons (1 dead-code trap, 2 false-positive audits, 4 CRLF, 10 write/read ordering, 12 undefined fallbacks, 13 precision mismatch). Replace specific ones with placeholders.
4. **`BACKLOG.md`** — template verbatim; empty the example items.
5. **`.claude/agents/code-reviewer.md`** — rename, genericize check list (keep taxonomy, strip MG violations).
6. **`.claude/agents/grep-verifier.md`** — rename "grep-verifier" → keep; swap MG-specific false-positive examples for generic ones.
7. **Chen protocol** (`.claude/skills/chen/` — SKILL.md + chen-superprompt-v83.md, 775 lines). Chen → rename "Auditor" persona. Legal examples → generic. **Highest-ROI single extraction.**
8. **Skills (24 generic)**: `100`, `adverse`, `code`, `decide`, `diagnose`, `dx`, `grep-verify` (+ `gv` alias), `lossy`, `pr`, `session-start`, `session-end`, `end`, `soft`, `sql`, `swarm`, `test`, `zero`, `shutup`, `dwt`, `dc`, `devserv`/`start`, `gitnexus-cli`/`-debugging`/`-exploring`/`-guide`.
9. **Commands (14–16 generic)**: `adverse`, `blast`, `dead`, `downstream`, `upstream`, `impact`, `verify`, `swarm`, `finalize`, `grep-all`, `zero`/`zt`, `project-tdd`. Read `label`/`max`/`root`/`db`/`sql` in MODE 2 to classify.
10. **`.github/pull_request_template.md`** — generic scaffold, strip MG-specific checks.
11. **Docs**: `PRODUCTION_READINESS_CHECKLIST.md`, `POST_MORTEM_ORCHESTRATOR.md`, `BRANCH_PROTECTION_SETUP.md`, `SECRETS_ROTATION_CHECKLIST.md`, `DEAD_CODE_DETECTION_CHECKLIST.md`.
12. **`.claude/settings.json` + `launch.json`** — structural scaffolds, strip MG paths + block lists (replace with placeholder stubs).

### Should-extract with genericization (pattern valuable, content specific)
13. **`.claude/agents/civ-auditor.md`** → reshape as "domain-auditor" template demonstrating pipeline-tracing methodology; legal examples replaced with generic pipeline.
14. **`.claude/rules/` pattern** — not content. Write one example generic rule file demonstrating path-scoping + DO NOT lists + architecture notes + thresholds structure.
15. **Audit-report structure** from `audits/*-audit-2026-04-15.md` → a generic audit-report template under `workflows/audit-report-template.md`. Not the content.
16. **PROGRESS.md format** — strip all content, keep only the scaffolding and "How to add entries" discipline.
17. **`.claude/rules/inngest.md`** — maybe worth keeping as generic orchestration-rules template (idempotency, circuit-breaker, retry patterns). Flag in MODE 2.

### Skip (too domain-specific)
18. All 49 `prompts/PHASE_*.md` files (legal case analysis, evidence strategy, motion drafting).
19. `lib/config/*.ts` (phase registry, tier config, court map).
20. `supabase/` migrations (243 SQL files).
21. `scripts/chen-monitor-v10-*`, `audit-cl-*.ts`, `civ/` subdir, `cl-census/` subdir.
22. `components/`, `contexts/`, `app/`, `hooks/` (React), `emails/`, `public/`, `test/`, `tests/`, `benchmarks/`.
23. `.gitnexus/` (regenerated per codebase).
24. `.claude/rules/inviolable-decisions.md`, `civ-pipeline.md`, `courtlistener.md`, `phase-iv.md`, `generators.md` — content, not pattern.
25. `.claude/skills/d-code`, `CIV` (duplicate), `census`, `civ-trace` (MG-DB specific queries).
26. All MG `audits/` content (extract template structure only per item 15).

### Flag for operator decision
27. **mg-overnight vs MG-Production default source**: confirm "MG-Production canonical, exception-by-exception fallback to mg-overnight" policy (see R4 recommendation above).
28. **Skills of ambiguous reusability**: `st` (stress test), `adverse` (MG has a legal role-swap variant mixed with generic adversarial framework), `civ-trace` (methodology value only if genericized heavily).
29. **Commands needing a read in MODE 2** before classification: `label`, `max`, `root`, `db`, `sql`.
30. **`.claude/rules/inngest.md`**: keep as generic-orchestration template, or skip as domain-infra?
31. **`README.md`**: keep the product-overview structure as a template, or write a fresh generic README?
32. **Chen rename**: "Chen" is a personal/fictional name and should almost certainly be genericized. Proposed rename: "Auditor" (or operator-preferred). Confirm.
33. **LEARNINGS.md lessons to carry over**: default is generic lessons only (1, 2, 4, 10, 12, 13). Confirm list or adjust.

### Swarm / Mythos / Hermes / Arsenal hook points (unimplemented — document only)
Neither source contains any Arsenal / Mythos / Hermes integration code or references. The only "swarm" presence is the `swarm` skill + command (parallel-agent coordination at the harness level, not a separate orchestrator). MODE 2 must document in `docs/architecture.md`:
- Where future swarm-mode integration hooks in (likely at the persona-selection layer).
- Where a future Hermes agent would plug in (likely as a new persona + MCP surface).
- Where Mythos/Arsenal belong if adopted.

These are hook-point descriptions, not implementations.

---

## Summary Stats

| Metric | Value |
|---|---|
| Source trees scanned | 2 |
| Top-level methodology files scanned | 6 (AGENTS, CLAUDE, LEARNINGS, README, PROGRESS, BACKLOG) |
| `.claude/` subfiles scanned | ~60 (18 commands + 31 skills + 3 agents + 6 rules + settings + launch) |
| Files identical across sources | ~12 (including 4 top-level methodology + code-reviewer.md + all scaffolding structure) |
| Files differing in content (same name, evolved body) | 54 (18 commands + 28 skills + 2 agents + 6 rules) |
| **Files recommended for extraction (must + should)** | **~50** |
| Files recommended skip | Hundreds (entire `app/`, `lib/`, `supabase/`, `prompts/`, `components/`, etc.) |
| Operator decisions flagged | 7 (items 27–33) |

## Next Step

**Halt and await MODE 2 approval.** On approval, MODE 2 proceeds per the R1/R3/R5/R7 plan: scaffold → personas → skills → workflows → architecture hooks → root CLAUDE.md (+ Karpathy) → docs. Operator-decision items above must be resolved at MODE-2 start.
