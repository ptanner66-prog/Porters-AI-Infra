# CLAUDE.md — tanner-stack Orientation

<!-- provenance: written fresh for tanner-stack as the self-teaching entry point -->
<!-- Audience: a future Claude Code session with NO prior context about this repo -->

You are reading the root orientation document of **tanner-stack** — a generic, reusable coding-methodology repository. This file is your entry point. Read it end-to-end before taking any action. Every section below is binding.

---

## 1. What This Stack Is

tanner-stack is a drop-in starter kit for AI-assisted software engineering. It is not an application. It ships four things and nothing else:

| Asset | Where | What it is |
|---|---|---|
| **Sub-agents** | `.claude/agents/` | Named sub-agents (Architect, Chen, Code Reviewer, Grep Verifier) with auto-delegation via `description`-field triggers, declared modes, and staged approval gates. Reference-doc copies at `personas/`. |
| **Skills** | `skills/` | Single-purpose capabilities that auto-invoke based on description matching. 8 are preloaded into sub-agents; 16 auto-invoke from the main session. |
| **Workflows** | `workflows/` | Reusable processes (audit → fix → build loop, PR review, commit conventions, audit-report template). |
| **Harness config** | `.claude/` | Settings, sub-agents, command shortcuts, rule scaffolds, launch config. |

**Use it when:** you are installing an AI agent stack for a client's engineering org, starting a new project, or standardizing AI coding discipline across a team.

**Do not use it as:** a drop-in application, a code-generation library, or something that "does work for you." It is a methodology, not a machine.

This stack auto-dispatches task signals to the right sub-agent, skill, and workflow via Claude Code's native delegation — see §3.5 for routing rules. The `.claude/commands/` directory also defines a handful of structured slash-command workflows as overrides for direct control.

---

## 2. Read the Methodology First

Before you do anything else in this repo, read [docs/methodology.md](docs/methodology.md) end-to-end. It explains the four disciplines this stack enforces (evidence labeling, live-path verification, pattern census, bidirectional tracing) and how the persona / skill / command / rule layers compose.

Then skim [docs/architecture.md](docs/architecture.md) to understand current integration surfaces and future hook points (Swarm, Hermes, Mythos, Arsenal).

Then read [docs/extending.md](docs/extending.md) before authoring any new persona, skill, command, or rule — it specifies the templates and quality bar.

Then read [AGENTS.md](AGENTS.md) for session-level governance: mode selection (IMPLEMENT / AUDIT / DISCOVERY), parallel-agent rules, model routing, and the GitNexus inviolable mandate. This is the governance layer that sits above the sub-agents — each sub-agent operates *within* one of the session modes defined there.

Finally, internalize §3.5 below (Auto-Dispatch Rules) — the signal→sub-agent mapping that documents how Claude Code's native delegation routes a user's task. Routing is driven by each sub-agent's `description` frontmatter field in `.claude/agents/`, not by instructions in this doc. The tables in §3.5 exist for transparency and to cover signals that the `description` fields don't directly encode.

If you skip this, you will rediscover pitfalls the stack already encodes.

---

## 3. Sub-Agents — How They Get Picked

The four named operating identities (Architect, Chen, Code Reviewer, Grep Verifier) are Claude Code sub-agents at `.claude/agents/*.md`. Claude Code delegates to them automatically based on each sub-agent's `description` frontmatter field. Explicit slash-style invocation (if present in the harness) also works.

| Sub-agent | File | Model | Tools | Preloaded Skills | Purpose |
|---|---|---|---|---|---|
| **Architect** | [.claude/agents/architect.md](.claude/agents/architect.md) | opus | Read, Write, Edit, Glob, Grep, Bash | `swarm` | System design, repo scaffolding, methodology extraction, staged audit/extract/verify workflows, **and swarm orchestration authority**. |
| **Chen** | [.claude/agents/chen.md](.claude/agents/chen.md) | opus | Read, Grep, Glob, Bash (read-only) | `chen`, 4 audit modes | Adversarial systems audit via **four focus modes**: DEEP SUBSYSTEM, FINDING EXPANSION, SPEC-TO-CODE DELTA, PRE-LAUNCH FAILURE. Defers to [prompts/superprompts/chen-audit-protocol.md](prompts/superprompts/chen-audit-protocol.md). |
| **Code Reviewer** | [.claude/agents/code-reviewer.md](.claude/agents/code-reviewer.md) | sonnet | Read, Grep, Glob, Bash (read-only) | `pr` | Post-diff review. Combines structural evidence (GitNexus) with text evidence (grep). Three-layer discipline: correctness, design, style. |
| **Grep Verifier** | [.claude/agents/grep-verifier.md](.claude/agents/grep-verifier.md) | sonnet | Read, Grep, Glob, Bash (read-only) | `grep-verify` | Skeptical claim validator. Rates every claim CONFIRMED / LIKELY / INDETERMINATE / FALSE POSITIVE with grep evidence. |

**How delegation works:** Claude Code reads each sub-agent's `description` field at session start and routes natural-language signals to the best match. Sub-agents run in isolated contexts and return findings to the main session. The main session decides what to do with them.

> **Swarm orchestration is architect-owned.** Any parallel execution decision — when to swarm, when not to, how to decompose the work — routes through the architect sub-agent. The `swarm` skill is preloaded only in `.claude/agents/architect.md`. Other sub-agents request a swarm *via* architect; they do not invoke it directly.

> **Chen has four focus modes.** Chen declares its audit mode at the start of every invocation based on task signals (see the mode-selection table in [.claude/agents/chen.md](.claude/agents/chen.md)). Chen never mixes modes within a single audit run — focus is the discipline.

> **Sub-agents don't inherit skills from the parent session.** Each sub-agent preloads its own skill set via the `skills:` frontmatter field. A skill the main session has access to is not automatically available to a sub-agent unless listed in that sub-agent's frontmatter.

> **Main-session skills auto-invoke.** The ~16 skills in `skills/` not preloaded in a sub-agent are invoked by the main agent via Claude Code's Skill-tool description matching — no manual slash command needed. Each such skill's `description` field names the task signals that should fire it.

**Persona reference docs** live at `personas/*.md` and mirror the sub-agent content for historical continuity and extension guidance — the sub-agent files in `.claude/agents/` are authoritative.

Architect and Chen are multi-mode: each declares its mode at the start of every response and halts between modes for operator approval. Never mix modes in a single response.

---

## 3.5 Auto-Dispatch Rules

Routing sub-agents and skills from natural-language task signals is handled primarily by Claude Code's native delegation: each sub-agent's `description` frontmatter field (in `.claude/agents/*.md`) and each main-session skill's `description` field (in `skills/*/SKILL.md`) describe when they fire, and Claude Code picks the best match. **Structured slash-command workflows in `.claude/commands/` always override auto-dispatch** when you want direct control. The tables below document the intended routing for transparency — the `description` fields are authoritative; these tables are advisory.

### 3.5.1 Task-Signal Routing

Consult this table before asking clarifying questions.

| User signal | Default dispatch | Notes |
|---|---|---|
| "audit" / "review this" / "is this correct" / "find issues" | **[chen sub-agent](.claude/agents/chen.md).** Pick audit submode: DEEP SUBSYSTEM (broad), FINDING EXPANSION (known defect), SPEC-TO-CODE DELTA (spec comparison), PRE-LAUNCH FAILURE (ship gate). | All four submodes are audit-only. Findings report, no edits. |
| "fix X" / "resolve Y" / "clean up Z" | **[workflows/audit-fix-build.md](workflows/audit-fix-build.md) Phase 2 (FIX).** If a prior audit report exists in context, resume at Phase 2 with that finding. Otherwise dispatch to Chen first, then Phase 2. | Chen has no FIX mode. The implementing session does the fix per the workflow. |
| "build X" / "add X" / "create a feature" | **[workflows/audit-fix-build.md](workflows/audit-fix-build.md) Phase 3 (BUILD)** via main session; conclude by auto-invoking the `pr` skill (preloaded in the code-reviewer sub-agent). | For net-new features without prior code, Phase 1 AUDIT may be skipped. |
| "refactor" / "restructure" / "reorganize" | **Full audit-fix-build loop** starting with Chen MODE 1 (DEEP SUBSYSTEM) or MODE 2 (FINDING EXPANSION) to map structural scope before any edits. Use `/blast` on all modified symbols. | Refactoring is the highest-risk change class — always audit first. |
| "PR review" / "review this pull request" / "review this diff" | **[code-reviewer sub-agent](.claude/agents/code-reviewer.md)** + **[workflows/pr-review.md](workflows/pr-review.md)**. | Code Reviewer is post-diff, not pre-implementation. |
| "verify X" / "confirm Y" / "check that Z" | **[grep-verifier sub-agent](.claude/agents/grep-verifier.md)** for text-level claims (does file X contain Y?). **Chen MODE 2 (FINDING EXPANSION)** for semantic claims (does subsystem X correctly handle Y?). | Choose by claim type. |
| "explain this codebase" / "onboard me" / "how does X work" | **[architect sub-agent](.claude/agents/architect.md) DISCOVER mode.** If GitNexus is present (see §3.5.2), the `gitnexus-exploring` skill auto-invokes. | DISCOVER is read-only; no edits. |
| "bulk task" / "do X across all files" / "parallel" / "swarm" | **[architect sub-agent](.claude/agents/architect.md)** — architect decides swarm vs. serial and invokes its preloaded `swarm` skill when warranted. See §3.5.4. | |

### 3.5.2 Tool-Presence Conditionals

tanner-stack's core (sub-agents, skills, workflows) is always available. External tools may or may not be installed — detect before using.

| Tool | Detection | Use for | If absent |
|---|---|---|---|
| **GitNexus** | `.gitnexus/` directory at the project root, or `gitnexus_*` MCP tools visible in the session | Structural queries: who calls X, what depends on Y, blast-radius before edits, safe multi-file renames | Note in LEARNINGS.md that GitNexus is not installed. Fall back to `grep -rn 'importPattern'` for call-graph; manual impact analysis. [code-reviewer sub-agent](.claude/agents/code-reviewer.md) runs in degraded mode (grep-only) per its "GitNexus availability contract" section. |
| **Arsenal** (hook scripts) | `hooks/` directory following Arsenal conventions (pre-commit checks, test runners, verification gates) | Automated invocation of workflows on commit/push events | Invoke workflows manually. Run [workflows/pr-review.md](workflows/pr-review.md) layers by hand rather than via hook. |
| **Claude Code built-ins** | Always present | File operations, Bash, WebFetch, Grep, Glob, Read, Write, Edit | n/a |
| **Mythos / Hermes** | Referenced only by the *project-level* `CLAUDE.md` (not by tanner-stack itself) | If a project's CLAUDE.md documents them, follow that guide | Treat as future-integration hooks — see [docs/architecture.md § Future Hook Points](docs/architecture.md). Do not invent invocation for them. |

### 3.5.3 Default Workflow Selection

Once the task is classified per §3.5.1, the corresponding workflow is automatic:

| Task class | Workflow |
|---|---|
| Bug, correctness task, refactor | [workflows/audit-fix-build.md](workflows/audit-fix-build.md) |
| PR review | [workflows/pr-review.md](workflows/pr-review.md) |
| Creating a commit | [workflows/commit-conventions.md](workflows/commit-conventions.md) |
| Audit output formatting | [workflows/audit-report-template.md](workflows/audit-report-template.md) |
| Unknown / ambiguous | **Halt and ask — do not guess.** See §3.5.5. |

### 3.5.4 Swarm Triggering

**Swarm orchestration is architect-owned.** Natural-language signals that imply parallel work ("bulk task", "do X across all files", "parallel", "swarm", "refactor these N files consistently") route to the [architect sub-agent](.claude/agents/architect.md), which decides swarm vs. serial and — if swarm is warranted — invokes its preloaded `swarm` skill with a decomposed task list.

Trigger a swarm when the task is genuinely parallelizable. Skip when the task requires holistic reasoning.

**Trigger a swarm if any of these hold:**
- Task operates across 10+ independent files.
- No cross-file dependencies between the subtasks.
- Audit work where findings are independent per subsystem.
- Bulk refactor following a consistent, mechanical pattern.

**Do NOT swarm when:**
- Single-file work.
- Sequential dependencies (subtask B consumes subtask A's output).
- Architectural changes requiring holistic reasoning about the whole system.
- Fewer than ~5 files — swarm overhead exceeds benefit.

**Invocation:** architect decomposes the task and invokes the preloaded `swarm` skill ([skills/swarm/SKILL.md](skills/swarm/SKILL.md)) — decompose by file boundary, deploy up to 5 subagents, synthesize results, no two agents edit the same file. Synthesis step is mandatory.

### 3.5.5 Clarification Gate

**Binding rule.** Halt and ask ONE clarifying question before dispatching when any of these is true:

- The task signal does not clearly match any row in §3.5.1.
- Two conflicting signals are present (e.g., "audit and fix this" — that's a two-phase workflow, not a single dispatch; confirm whether to run AUDIT first and pause, or run the full loop).
- The target scope is undefined (which file? which subsystem? which PR?).
- The desired output shape is ambiguous (findings report vs. implemented fix vs. both).

Ask exactly one question. **Do not chain guesses. Do not propose a menu of possible tasks for the user to pick from.** Once the operator answers, dispatch and proceed. If they answer ambiguously, halt again with a tighter question.

This is not a stuck state. It is the default posture for any non-trivial task. The stack is built on the premise that **clarification cost < misroute cost**.

### 3.5.6 Live Markdown Cross-Reference Graph

**Do not trust any hardcoded file list, skill list, persona list, or workflow list — anywhere in this document or anywhere else.** Those lists drift silently as the repo evolves. The markdown files themselves are the source of truth. Build a live cross-reference graph on session start and use it for all routing and discovery decisions.

**Walk these locations on session start:**

- `CLAUDE.md` (this file), `README.md`, `AGENTS.md`, `LEARNINGS.md`, `BACKLOG.md`
- `.claude/agents/*.md` (authoritative sub-agents)
- `personas/*.md` (reference-doc copies)
- `skills/*/SKILL.md`
- `workflows/*.md`
- `docs/**/*.md`
- `.claude/rules/*.md`
- `.claude/commands/*.md`

For each markdown file, build a **node** with:

| Attribute | Value |
|---|---|
| `path` | File path relative to repo root |
| `type` | `sub-agent`, `persona`, `skill`, `workflow`, `doc`, `rule`, `command`, or `other` — inferred from the directory |
| `frontmatter` | Parsed YAML frontmatter if present; `null` otherwise |
| `summary` | One-line summary — from the `description:` frontmatter field if present, else the first prose paragraph after the top-level heading |

For each internal reference in that file, build an **edge**:

- Markdown links: `[label](path/to/other.md)` or `[label](path/to/other.md#anchor)` — anchor targets are edge metadata.
- File path mentions: bare references like `skills/chen/SKILL.md` appearing in prose or code fences.
- Frontmatter references: any field value that names another `.md` file.

**Use the graph for:**

1. **"What exists" queries.** `graph.filter(n => n.type === 'skill')` returns all skills. `graph.filter(n => n.type === 'sub-agent')` returns all sub-agents. `graph.filter(n => n.type === 'persona')` returns the reference-doc mirror. Etc. The categorized list in §4 and the routing table in §3.5.1 are hints that may be stale; the graph is authoritative.
2. **Auto-dispatch routing.** When §3.5.1 points at a sub-agent or skill by name, resolve the reference against the graph to confirm the target still exists and look up its one-line summary. If the `.claude/agents/<name>.md` node is present, its `description` field is the canonical delegation trigger; the §3.5.1 row is advisory.
3. **Drift detection.** Any edge that points to a file not present in the graph is a **broken link**. Report all broken links to the operator at session start as a drift signal. Do not silently continue past a broken link that affects the current task.

**Do not persist the graph.** Build it live in the agent's context on every session start. It is derived state, not committed state. No `.mdgraph.json` artifact, no pre-built index, no cache file in `.claude/`.

**Rebuild triggers:**

- Session start (mandatory).
- After a `git pull`, `git checkout`, or branch switch.
- When a referenced file is unexpectedly missing mid-session.
- When any `.md` file in the walked locations is edited during the session.

**Edge cases:**

- **Alias skills** — v0.3.0 deleted the separate alias directories (`gv`, `start`, `end`); aliases are now notes on the canonical skill bodies. Treat the canonical as the only node; alias mentions in prose are documentation, not separate dispatch targets.
- **Malformed markdown** — directory or file exists but frontmatter is unparseable, or the file has no top-level heading — include the node with `frontmatter: null` and `summary: "(unparseable — flagged)"`. Flag to the operator and continue.
- **Non-markdown references** — files like `.claude/settings.json`, `.claude/launch.json`, or shell scripts may be linked from markdown. Optionally add them as leaf nodes with `type: other` so link-breakage detection still covers them; `frontmatter` and `summary` are n/a.
- **Scope boundary** — the `.git/` directory is out of scope. `docs/build-notes/` is in scope but every node under it carries `type: other` and a note that the subtree is extraction history, not methodology. `node_modules/` and generated build output (if any) are out of scope.

The live graph makes routing decisions in §3.5.1 through §3.5.5 resilient to repo evolution. Anything you would otherwise have to remember from this document becomes a lookup against the graph instead. If §3.5.1 and the graph disagree, the graph wins.

---

## 4. Skills — Check Before Writing Code

**Always check `skills/` before writing a new script, query, prompt, or procedure.** There is a good chance the discipline you need already exists as a named skill.

Skills auto-invoke based on description matching. You do not need to memorize names — describe the task and the right skill fires. Each skill's `description` frontmatter field names its task signals.

Index by category (current as of v0.3.1):

- **Rigor modes** (stay active for the session): `100`, `zero`, `soft`, `shutup`
- **Adversarial audit / stress-test**: `adverse`, `code`, `lossy`, `st`
- **Active-bug investigation**: `diagnose`
- **Decision support**: `triage` (structured option comparison when user presents 2+ choices), `decide` (surfaces pending project decisions)
- **Task completion**: `finish` (commit-and-close-out on in-progress work; distinct from `pr` for PR submission)
- **Document compression**: `summarize` (length-targeted compression with signal preservation; distinct from `lossy` which debugs pipeline data loss)
- **Session lifecycle**: `session-start` (alias `start`), `session-end` (alias `end`), `pr` (preloaded in code-reviewer sub-agent)
- **GitNexus integration** (auto-invoke only when GitNexus tooling is available): `gitnexus-cli`, `gitnexus-debugging`, `gitnexus-exploring`, `gitnexus-guide`
- **Sub-agent-preloaded** (not main-session auto-invoke): `chen`, `audit-deep-subsystem`, `audit-finding-expansion`, `audit-spec-to-code-delta`, `audit-pre-launch-failure` (chen sub-agent); `grep-verify` (grep-verifier sub-agent); `swarm` (architect sub-agent — architect-invoked only)

Full catalog under `skills/`. New skills go through [docs/extending.md](docs/extending.md) and the [skills/_template/SKILL.md](skills/_template/SKILL.md) scaffold. The structured slash-command workflows in `.claude/commands/*.md` (e.g. `blast`, `verify`, `downstream`, `zt`) remain available as overrides.

---

## 5. Audit → Fix → Build Loop

This is the default workflow for any non-trivial change. Full description in [workflows/audit-fix-build.md](workflows/audit-fix-build.md). Summary:

1. **Audit** — the chen sub-agent auto-delegates from "audit this subsystem" or equivalent signals, OR the code-reviewer sub-agent handles submitted-diff review. Produces a findings report with evidence labels and severity. **Do not fix anything in this phase.**
2. **Fix** — only after audit findings are reviewed and approved. Fix one finding at a time. The `grep-verify` skill (preloaded in the grep-verifier sub-agent) verifies key claims. Run the project typecheck after every edit.
3. **Build** — integrate the fixes. Run the full test suite. The `adverse` skill auto-invokes from "what am I missing" for a completeness check. The `pr` skill (preloaded in code-reviewer sub-agent) handles PR submission with a final safety pass.

**Rules:**
- Never skip the audit phase. "I already know the problem" is how false-positive-rate climbs.
- Never batch multiple findings into a single fix commit unless they are a grouped change set (same file, same function, same contract).
- Never merge a PR whose audit pass was skipped or waived.

PR discipline: [workflows/pr-review.md](workflows/pr-review.md). Commit messages: [workflows/commit-conventions.md](workflows/commit-conventions.md). Audit-report format: [workflows/audit-report-template.md](workflows/audit-report-template.md).

---

## 6. Extending the Stack

When you need something the stack doesn't already provide, **add it through the extending workflow, not ad-hoc**:

- **New sub-agent** → copy a `.claude/agents/` file as a starting point, then mirror a reference-doc copy in `personas/`. Follow [docs/extending.md § Adding a Persona](docs/extending.md).
- **New skill** → `cp -r skills/_template skills/<name>`, fill in the slots, follow [docs/extending.md § Adding a Skill](docs/extending.md).
- **New command** → add a short `.md` under `.claude/commands/`; if it overlaps with a skill, the command is a thin wrapper that delegates.
- **New rule** → add a `.md` under `.claude/rules/`, following the pattern in [example-rule.md](.claude/rules/example-rule.md) (path-scoped DO NOT list, architecture notes, thresholds, key files).
- **New workflow** → add a `.md` under `workflows/`, modeled on the existing three.

Every new file under `.claude/agents/`, `personas/`, `skills/`, `workflows/`, `prompts/`, and `docs/` must carry a provenance header (see [docs/extending.md § Provenance Headers](docs/extending.md)).

---

## 7. Swarm Mode — Parallel-Agent Activation

**What it is:** a coordination discipline for deploying multiple agents in parallel on independent subtasks, then synthesizing their outputs.

**How to activate it:** describe a parallel task naturally ("refactor these 40 files consistently", "audit these 12 independent subsystems in parallel"). The architect sub-agent decides swarm vs. serial and, when warranted, invokes its preloaded `swarm` skill. Full protocol: [skills/swarm/SKILL.md](skills/swarm/SKILL.md). A structured command wrapper exists at [.claude/commands/swarm.md](.claude/commands/swarm.md) for direct override.

**Rules:**
- Decompose by file boundary — no two agents edit the same file.
- Max 5 subagents per swarm (context budget).
- Read-only subagents: they investigate and report; the main thread synthesizes and edits.
- Synthesis step is mandatory.

**Future integration hooks** (not yet implemented, documented for planning — see [docs/architecture.md § Future Hook Points](docs/architecture.md)):

- **Hermes** — notification / routing agent. Reserved subsection in architecture.md.
- **Mythos** — long-term memory layer. Reserved subsection in architecture.md.
- **Arsenal** — tool registry. Reserved subsection in architecture.md.

Do not invent architecture for Hermes / Mythos / Arsenal. If a task requires them, halt and ask the operator.

---

## 8. Common LLM Coding Pitfalls

These are the four failure modes LLM-assisted coding systems repeatedly hit. tanner-stack's personas, skills, and rigor modes exist largely to counter them. Internalize these before touching any code.

### 8.1. Think Before Coding
State your assumptions explicitly. If uncertain, ask. Do not proceed with hidden confusion or unexamined tradeoffs. Silent assumption → downstream bug you'll spend 10× longer debugging.

> Enforced by: `/100`, `/soft`, `/zero`, every persona's mode-declaration rule.

### 8.2. Simplicity First
Write minimal code that solves the stated problem. No speculative features. No abstractions for single-use code. If your solution exceeds the obvious length, simplify ruthlessly — most "comprehensive" code is padding.

> Enforced by: Chen's "no rewrites when precise fixes are available" directive, Code Reviewer's scope-drift check.

### 8.3. Surgical Changes
When modifying existing code, touch only what you must. Match surrounding style. Do not "improve" unrelated areas. Remove only the dead code your changes actually created — do not refactor as a side effect.

> Enforced by: `/adverse` integration-wiring check, Code Reviewer's blast-radius analysis.

### 8.4. Goal-Driven Execution
Transform the operator's request into verifiable success criteria before implementing. Pair each step of your plan with a concrete check (grep, SQL query, test). A task is not complete because code compiled — it's complete when the intended behavior is provably restored.

> Enforced by: `/diagnose` Phase 3 (FIX PROPOSAL), `/verify` three-leg loop, Chen's EXECUTION PROOF RULE.

_Synthesized from: [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) — Andrej Karpathy's distilled observations on LLM coding pitfalls. Credit to Karpathy and Forrest Chang for the original framing._

The concrete mechanism for activating these principles on any given task is **§3.5 Auto-Dispatch Rules** above. "Surgical changes" (8.3) is operationalized by routing fix / refactor tasks through the audit-fix-build workflow so that scope expansion is blocked by the workflow's halt gates rather than by agent restraint alone. "Goal-driven execution" (8.4) is operationalized by §3.5.5's clarification gate — halt and ask before guessing the goal. Karpathy's principles are the *why*; §3.5 is the *how*.

---

## 9. What This File Is NOT

- **Not a project CLAUDE.md.** When you adopt tanner-stack for a specific project, copy [docs/project-claude-md-template.md](docs/project-claude-md-template.md) to the project root as `CLAUDE.md` and fill in its slots (dead-code paths, inviolable rules, commands, danger zones). That template is the project-level file. **This file** (the one you're reading) stays in tanner-stack itself as the orientation for anyone working inside the stack.
- **Not an application.** There is no build, no runtime, no server. Just markdown and configuration.
- **Not exhaustive.** Anything not covered here lives in the docs/ directory or in the files each section links to. Follow the links.

---

## 10. Handoff to the Next Session

Before ending any session in this repo, run `/session-end` or `/end` to produce a structured handoff. Before starting, run `/session-start` or `/start` for a briefing. The lifecycle skills exist for a reason — use them.

When in doubt: halt and ask the operator. The stack rewards paranoia.
