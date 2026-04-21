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

---

## 2. Read the Methodology First

Before you do anything else in this repo, read [docs/methodology.md](docs/methodology.md) end-to-end. It explains the four disciplines this stack enforces (evidence labeling, live-path verification, pattern census, bidirectional tracing) and how the persona / skill / command / rule layers compose.

Then skim [docs/architecture.md](docs/architecture.md) to understand current integration surfaces and future hook points (Swarm, Hermes, Mythos, Arsenal).

Then read [docs/extending.md](docs/extending.md) before authoring any new persona, skill, command, or rule — it specifies the templates and quality bar.

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

---

## 9. What This File Is NOT

- **Not a project CLAUDE.md.** When you adopt tanner-stack for a specific project, copy [docs/project-claude-md-template.md](docs/project-claude-md-template.md) to the project root as `CLAUDE.md` and fill in its slots (dead-code paths, inviolable rules, commands, danger zones). That template is the project-level file. **This file** (the one you're reading) stays in tanner-stack itself as the orientation for anyone working inside the stack.
- **Not an application.** There is no build, no runtime, no server. Just markdown and configuration.
- **Not exhaustive.** Anything not covered here lives in the docs/ directory or in the files each section links to. Follow the links.

---

## 10. Handoff to the Next Session

Before ending any session in this repo, run `/session-end` or `/end` to produce a structured handoff. Before starting, run `/session-start` or `/start` for a briefing. The lifecycle skills exist for a reason — use them.

When in doubt: halt and ask the operator. The stack rewards paranoia.
