# tanner-stack

A generic, reusable coding-methodology repository. Distilled from a production coding system and designed to be dropped into any project that will be driven by Claude Code.

## What this is

tanner-stack is a self-teaching starter kit for AI-assisted software engineering. It ships four things:

- **Sub-agents** — named Claude Code sub-agents (Architect, Chen, Code Reviewer, Grep Verifier) at `.claude/agents/` with auto-delegation via `description`-field triggers. Reference-doc copies mirror them in `personas/`.
- **Skills** — composable capabilities invoked by name (`/chen`, `/100`, `/adverse`, `/swarm`, etc.) that focus a session on a single discipline.
- **Workflows** — reusable processes for audit → fix → build loops, PR review, commit conventions, and incident response.
- **Infrastructure** — harness configuration (`.claude/`), rule scaffolds, and hook points for parallel-agent coordination.

The repository is designed to be read **cold** by a future Claude Code session with no prior context. Everything an agent needs to bootstrap is in `CLAUDE.md` and `docs/methodology.md`.

## How it works

Every task runs through this loop:

```
        ┌─────────────────┐
        │    Operator     │
        │  (you or agent) │
        └────────┬────────┘
                 │ task
                 ▼
        ┌─────────────────┐
        │    Sub-agent    │  ← .claude/agents/
        │  (mode declared)│
        └────────┬────────┘
                 │ invokes
                 ▼
        ┌─────────────────┐
        │     Skills      │  ← skills/
        │  (/chen, /100)  │
        └────────┬────────┘
                 │ executes
                 ▼
        ┌─────────────────┐
        │    Workflow     │  ← workflows/
        │  (audit→fix→    │
        │   build, PR)    │
        └────────┬────────┘
                 │ outputs
                 ▼
        ┌─────────────────┐
        │     Commit      │
        │  (conventional) │
        └────────┬────────┘
                 │ captures
                 ▼
        ┌─────────────────┐
        │    LEARNINGS    │  ← feeds back into skills/personas
        └─────────────────┘
```

Personas declare modes. Skills focus sessions. Workflows structure output. Commits preserve history. Learnings compound.

## Who this is for

- Consultants installing an AI agent stack for a client's engineering org.
- Teams adopting Claude Code who want a strong default configuration before customizing.
- Solo operators starting a new project and wanting a proven scaffold.

## Quick start

1. Copy this repository into the root of your target project (or clone it and vendor what you need).
2. Open the project in Claude Code.
3. The agent reads `CLAUDE.md` and `docs/methodology.md` first. Both are self-contained.
4. Customize the domain-specific slots noted in the files (operator name, dead-code paths, inviolable rules, etc.).

## Repo map

```
tanner-stack/
├── CLAUDE.md                 — entry point; teaches a future session how to use this stack
├── docs/
│   ├── methodology.md        — how the system works, in plain English
│   ├── architecture.md       — integration surfaces and hook points
│   └── extending.md          — how to add new personas, skills, and workflows
├── personas/                 — reference-doc mirror of sub-agents
├── skills/                   — single-purpose capabilities
├── prompts/                  — superprompts and reusable templates
├── workflows/                — audit-fix-build loop, PR review, commit conventions
└── .claude/
    ├── agents/               — operational sub-agents (Architect, Chen, Code Reviewer, Grep Verifier)
    ├── commands/             — slash-command workflows
    ├── rules/                — path-scoped constraints
    └── settings.json         — harness config
```

## Provenance

Extracted from a production Next.js legal-tech codebase and genericized for domain-agnostic reuse. Every file under `.claude/agents/`, `personas/`, `skills/`, `workflows/`, `docs/`, and `prompts/` carries a provenance header.

## License

MIT License. See LICENSE file.
