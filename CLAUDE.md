# CLAUDE.md — tanner-stack Orientation

<!-- provenance: written fresh for tanner-stack as the self-teaching entry point -->
<!-- Audience: a future Claude Code session with NO prior context about this repo -->

You are reading the root orientation document of **tanner-stack** — a generic, reusable coding-methodology repository. This file is your entry point. Read it end-to-end before taking any action. Every section below is binding.

---

## 1. What This Stack Is

tanner-stack is a drop-in starter kit for AI-assisted software engineering. It is not an application. It ships four things and nothing else:

| Asset | Where | What it is |
|---|---|---|
| **Personas** | `personas/` | Named operating identities (Architect, Chen, Code Reviewer, Grep Verifier) with declared modes and staged approval gates. |
| **Skills** | `skills/` | Single-purpose capabilities invoked by name (`/chen`, `/100`, `/adverse`, `/swarm`, etc.). |
| **Workflows** | `workflows/` | Reusable processes (audit → fix → build loop, PR review, commit conventions, audit-report template). |
| **Harness config** | `.claude/` | Settings, command shortcuts, rule scaffolds, launch config. |

**Use it when:** you are installing an AI agent stack for a client's engineering org, starting a new project, or standardizing AI coding discipline across a team.

**Do not use it as:** a drop-in application, a code-generation library, or something that "does work for you." It is a methodology, not a machine.

This stack auto-dispatches task signals to the right persona, skill, and workflow — see §3.5 for routing rules. Explicit slash commands (`/chen`, `/100`, `/adverse`, `/swarm`, etc.) remain available and override auto-dispatch when you want direct control.

---

## 2. Read the Methodology First

Before you do anything else in this repo, read [docs/methodology.md](docs/methodology.md) end-to-end. It explains the four disciplines this stack enforces (evidence labeling, live-path verification, pattern census, bidirectional tracing) and how the persona / skill / command / rule layers compose.

Then skim [docs/architecture.md](docs/architecture.md) to understand current integration surfaces and future hook points (Swarm, Hermes, Mythos, Arsenal).

Then read [docs/extending.md](docs/extending.md) before authoring any new persona, skill, command, or rule — it specifies the templates and quality bar.

Then read [AGENTS.md](AGENTS.md) for session-level governance: mode selection (IMPLEMENT / AUDIT / DISCOVERY), parallel-agent rules, model routing, and the GitNexus inviolable mandate. This is the governance layer that sits above the personas — a persona operates *within* one of the session modes defined there.

Finally, internalize §3.5 below (Auto-Dispatch Rules) — the routing table, tool-presence conditionals, and clarification gate that let a session route a user's task to the right persona and skill without waiting for an explicit slash command. Auto-dispatch is part of required onboarding, not optional polish.

If you skip this, you will rediscover pitfalls the stack already encodes.

---

## 3. Personas — How to Pick One

Personas live in `personas/`. Each declares its mode at the start of every response and halts between modes for operator approval.

| Persona | File | When to invoke |
|---|---|---|
| **Architect** | [personas/architect.md](personas/architect.md) | Extracting reusable methodology from a production codebase (DISCOVER → EXTRACT → VERIFY). Also for systems work that needs staged approvals. |
| **Chen** | [personas/chen.md](personas/chen.md) | Adversarial systems audit — hostile, evidence-driven. Deep subsystem audits, finding expansion, spec-to-code deltas, pre-launch failure audits. Defers to [prompts/superprompts/chen-audit-protocol.md](prompts/superprompts/chen-audit-protocol.md). |
| **Code Reviewer** | [personas/code-reviewer.md](personas/code-reviewer.md) | Post-diff review. Combines structural evidence (GitNexus) with text evidence (grep). Applies the project's inviolable-rule list. |
| **Grep Verifier** | [personas/grep-verifier.md](personas/grep-verifier.md) | Skeptical claim validator. Rates every claim CONFIRMED / LIKELY / FALSE POSITIVE with grep evidence. |

Invocation pattern: read the persona file end-to-end, adopt its identity, declare its mode, and follow its halt rules. Never mix modes in a single response.

---

## 3.5 Auto-Dispatch Rules

A fresh session should read the user's first message and route it to the right persona, skill, and workflow without waiting for an explicit slash command. **Explicit slash commands (`/chen`, `/100`, `/adverse`, `/swarm`, etc.) always override auto-dispatch** — a user who types `/chen` gets Chen, end of story. The rules below define defaults for when the user describes the work in natural language instead.

### 3.5.1 Task-Signal Routing

Consult this table before asking clarifying questions.

| User signal | Default dispatch | Notes |
|---|---|---|
| "audit" / "review this" / "is this correct" / "find issues" | **Chen persona.** Pick audit submode per [personas/chen.md § Mode](personas/chen.md): DEEP SUBSYSTEM (broad), FINDING EXPANSION (known defect), SPEC-TO-CODE DELTA (spec comparison), PRE-LAUNCH FAILURE (ship gate). | All four submodes are audit-only. Findings report, no edits. |
| "fix X" / "resolve Y" / "clean up Z" | **[workflows/audit-fix-build.md](workflows/audit-fix-build.md) Phase 2 (FIX).** If a prior audit report exists in context, resume at Phase 2 with that finding. Otherwise dispatch to Chen first, then Phase 2. | Chen has no FIX mode. The implementing session does the fix per the workflow. |
| "build X" / "add X" / "create a feature" | **[workflows/audit-fix-build.md](workflows/audit-fix-build.md) Phase 3 (BUILD)** via main session; conclude with `/pr`. | For net-new features without prior code, Phase 1 AUDIT may be skipped. |
| "refactor" / "restructure" / "reorganize" | **Full audit-fix-build loop** starting with Chen MODE 1 (DEEP SUBSYSTEM) or MODE 2 (FINDING EXPANSION) to map structural scope before any edits. Use `/blast` on all modified symbols. | Refactoring is the highest-risk change class — always audit first. |
| "PR review" / "review this pull request" / "review this diff" | **[personas/code-reviewer.md](personas/code-reviewer.md)** + **[workflows/pr-review.md](workflows/pr-review.md)**. | Code Reviewer is post-diff, not pre-implementation. |
| "verify X" / "confirm Y" / "check that Z" | **[personas/grep-verifier.md](personas/grep-verifier.md)** for text-level claims (does file X contain Y?). **Chen MODE 2 (FINDING EXPANSION)** for semantic claims (does subsystem X correctly handle Y?). | Choose by claim type. |
| "explain this codebase" / "onboard me" / "how does X work" | **[personas/architect.md](personas/architect.md) DISCOVER mode.** If GitNexus is present (see §3.5.2), also `/gitnexus-exploring`. | DISCOVER is read-only; no edits. |
| "bulk task" / "do X across all files" / "parallel" | `/swarm` — see §3.5.4 for trigger conditions and the go/no-go check. | |

### 3.5.2 Tool-Presence Conditionals

tanner-stack's core (personas, skills, workflows) is always available. External tools may or may not be installed — detect before using.

| Tool | Detection | Use for | If absent |
|---|---|---|---|
| **GitNexus** | `.gitnexus/` directory at the project root, or `gitnexus_*` MCP tools visible in the session | Structural queries: who calls X, what depends on Y, blast-radius before edits, safe multi-file renames | Note in LEARNINGS.md that GitNexus is not installed. Fall back to `grep -rn 'importPattern'` for call-graph; manual impact analysis. [personas/code-reviewer.md](personas/code-reviewer.md) runs in degraded mode (grep-only) per its "GitNexus availability contract" section. |
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

Invoke `/swarm` when the task is genuinely parallelizable. Skip `/swarm` when the task requires holistic reasoning.

**Trigger `/swarm` if any of these hold:**
- Task operates across 10+ independent files.
- No cross-file dependencies between the subtasks.
- Audit work where findings are independent per subsystem.
- Bulk refactor following a consistent, mechanical pattern.

**Do NOT swarm when:**
- Single-file work.
- Sequential dependencies (subtask B consumes subtask A's output).
- Architectural changes requiring holistic reasoning about the whole system.
- Fewer than ~5 files — swarm overhead exceeds benefit.

**Invocation:** `/swarm <task description and target file list>`. Full protocol in [skills/swarm/SKILL.md](skills/swarm/SKILL.md) — decompose by file boundary, deploy up to 5 subagents, synthesize results, no two agents edit the same file. Synthesis step is mandatory.

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
- `personas/*.md`
- `skills/*/SKILL.md`
- `workflows/*.md`
- `docs/**/*.md`
- `.claude/rules/*.md`
- `.claude/commands/*.md`

For each markdown file, build a **node** with:

| Attribute | Value |
|---|---|
| `path` | File path relative to repo root |
| `type` | `skill`, `persona`, `workflow`, `doc`, `rule`, `command`, or `other` — inferred from the directory |
| `frontmatter` | Parsed YAML frontmatter if present; `null` otherwise |
| `summary` | One-line summary — from the `description:` frontmatter field if present, else the first prose paragraph after the top-level heading |

For each internal reference in that file, build an **edge**:

- Markdown links: `[label](path/to/other.md)` or `[label](path/to/other.md#anchor)` — anchor targets are edge metadata.
- File path mentions: bare references like `skills/chen/SKILL.md` appearing in prose or code fences.
- Frontmatter references: any field value that names another `.md` file.

**Use the graph for:**

1. **"What exists" queries.** `graph.filter(n => n.type === 'skill')` returns all skills. `graph.filter(n => n.type === 'persona')` returns all personas. Etc. The categorized list in §4 and the routing table in §3.5.1 are hints that may be stale; the graph is authoritative.
2. **Auto-dispatch routing.** When §3.5.1 points at a skill or persona by name, resolve the reference against the graph to confirm the target still exists and look up its one-line summary.
3. **Drift detection.** Any edge that points to a file not present in the graph is a **broken link**. Report all broken links to the operator at session start as a drift signal. Do not silently continue past a broken link that affects the current task.

**Do not persist the graph.** Build it live in the agent's context on every session start. It is derived state, not committed state. No `.mdgraph.json` artifact, no pre-built index, no cache file in `.claude/`.

**Rebuild triggers:**

- Session start (mandatory).
- After a `git pull`, `git checkout`, or branch switch.
- When a referenced file is unexpectedly missing mid-session.
- When any `.md` file in the walked locations is edited during the session.

**Edge cases:**

- **Alias skills** (`/end` → `/session-end`, `/gv` → `/grep-verify`, `/start` → `/session-start`) — both the primary and alias directories are separate nodes. Both are valid dispatch targets and the graph preserves both.
- **Malformed markdown** — directory or file exists but frontmatter is unparseable, or the file has no top-level heading — include the node with `frontmatter: null` and `summary: "(unparseable — flagged)"`. Flag to the operator and continue.
- **Non-markdown references** — files like `.claude/settings.json`, `.claude/launch.json`, or shell scripts may be linked from markdown. Optionally add them as leaf nodes with `type: other` so link-breakage detection still covers them; `frontmatter` and `summary` are n/a.
- **Scope boundary** — the `.git/` directory is out of scope. `docs/build-notes/` is in scope but every node under it carries `type: other` and a note that the subtree is extraction history, not methodology. `node_modules/` and generated build output (if any) are out of scope.

The live graph makes routing decisions in §3.5.1 through §3.5.5 resilient to repo evolution. Anything you would otherwise have to remember from this document becomes a lookup against the graph instead. If §3.5.1 and the graph disagree, the graph wins.

---

## 4. Skills — Check Before Writing Code

**Always check `skills/` before writing a new script, query, prompt, or procedure.** There is a good chance the discipline you need already exists as a named skill.

Invocation is by name: `/chen <target>`, `/100`, `/swarm <task>`, etc.

Index by category:

- **Rigor modes** (stay active for the session): `/100`, `/zero`, `/soft`, `/shutup`
- **Adversarial audit**: `/chen`, `/adverse`, `/code`, `/lossy`, `/st`
- **Verification / diagnosis**: `/grep-verify` (alias `/gv`), `/diagnose`, `/decide`
- **Session lifecycle**: `/session-start` (alias `/start`), `/session-end` (alias `/end`), `/pr`
- **Utilities**: `/sql`, `/swarm`
- **GitNexus integration**: `/gitnexus-cli`, `/gitnexus-debugging`, `/gitnexus-exploring`, `/gitnexus-guide`

Full catalog under `skills/`. New skills go through [docs/extending.md](docs/extending.md) and the [skills/_template/SKILL.md](skills/_template/SKILL.md) scaffold.

---

## 5. Audit → Fix → Build Loop

This is the default workflow for any non-trivial change. Full description in [workflows/audit-fix-build.md](workflows/audit-fix-build.md). Summary:

1. **Audit** — invoke `/chen <subsystem>` or a Code Reviewer pass. Produce a findings report with evidence labels and severity. **Do not fix anything in this phase.**
2. **Fix** — only after audit findings are reviewed and approved. Fix one finding at a time. Run `/grep-verify` on the key claims. Run the project typecheck after every edit.
3. **Build** — integrate the fixes. Run the full test suite. Run `/adverse` for a completeness check. Open a PR via `/pr` — which delegates back to the Code Reviewer for a final safety pass before submission.

**Rules:**
- Never skip the audit phase. "I already know the problem" is how false-positive-rate climbs.
- Never batch multiple findings into a single fix commit unless they are a grouped change set (same file, same function, same contract).
- Never merge a PR whose audit pass was skipped or waived.

PR discipline: [workflows/pr-review.md](workflows/pr-review.md). Commit messages: [workflows/commit-conventions.md](workflows/commit-conventions.md). Audit-report format: [workflows/audit-report-template.md](workflows/audit-report-template.md).

---

## 6. Extending the Stack

When you need something the stack doesn't already provide, **add it through the extending workflow, not ad-hoc**:

- **New persona** → copy a `personas/` file as a starting point, follow [docs/extending.md § Adding a Persona](docs/extending.md).
- **New skill** → `cp -r skills/_template skills/<name>`, fill in the slots, follow [docs/extending.md § Adding a Skill](docs/extending.md).
- **New command** → add a short `.md` under `.claude/commands/`; if it overlaps with a skill, the command is a thin wrapper that delegates.
- **New rule** → add a `.md` under `.claude/rules/`, following the pattern in [example-rule.md](.claude/rules/example-rule.md) (path-scoped DO NOT list, architecture notes, thresholds, key files).
- **New workflow** → add a `.md` under `workflows/`, modeled on the existing three.

Every new file under `personas/`, `skills/`, `workflows/`, `prompts/`, and `docs/` must carry a provenance header (see [docs/extending.md § Provenance Headers](docs/extending.md)).

---

## 7. Swarm Mode — Parallel-Agent Activation

**What it is:** a coordination discipline for deploying multiple agents in parallel on independent subtasks, then synthesizing their outputs.

**How to activate it:** invoke `/swarm <task description and target areas>`. Full protocol: [skills/swarm/SKILL.md](skills/swarm/SKILL.md). Command wrapper: [.claude/commands/swarm.md](.claude/commands/swarm.md).

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
