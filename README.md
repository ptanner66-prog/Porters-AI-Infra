# tanner-stack

A generic, reusable coding-methodology repository. Distilled from a production coding system and designed to be dropped into any project that will be driven by Claude Code.

## What this is

tanner-stack is a self-teaching starter kit for AI-assisted software engineering. It ships four things:

- **Sub-agents** — four named Claude Code sub-agents at `.claude/agents/` (Architect, Chen, Code Reviewer, Grep Verifier) with auto-delegation driven by each sub-agent's `description` frontmatter field. Architect owns swarm orchestration. Chen operates in four focus modes (deep subsystem, finding expansion, spec-to-code delta, pre-launch failure). Reference-doc copies of each mirror in `personas/`.
- **Skills** — 28 composable capabilities in `skills/`. Eight are preloaded into sub-agents; nineteen auto-invoke from the main session based on their own `description` triggers; one scaffold (`_template`) for authoring new skills.
- **Workflows** — reusable processes for audit → fix → build loops, PR review, commit conventions, and incident response.
- **Infrastructure** — harness configuration (`.claude/`), rule scaffolds, hook points, and a live markdown cross-reference graph built on every session start (see [CLAUDE.md §3.5.6](CLAUDE.md)) so routing stays resilient as the repo evolves.

The repository is designed to be read **cold** by a future Claude Code session with no prior context. Everything an agent needs to bootstrap is in `CLAUDE.md` and `docs/methodology.md`.

## No slashes, no /commands — auto-dispatch

You speak to Claude Code naturally. "Audit this subsystem." "What would break at launch?" "Verify this claim." "Refactor these 40 files consistently."

Claude Code reads each sub-agent's and each skill's `description` frontmatter field at session start and routes your signal to the best match. Sub-agents run in their own isolated context and return findings. Main-session skills auto-invoke for focused tasks. You do not need to memorize slash commands — the descriptions carry the triggers.

Structured slash-command workflows in `.claude/commands/` (e.g. `/blast`, `/verify`, `/zt`) remain available as overrides when you want direct control. But the default experience is: describe the task, and the right handler runs.

## How it works

Two tiers: sub-agents with isolated context and preloaded domain skills, and main-session skills that auto-invoke for focused work.

```
                    ┌─────────────────────────────┐
                    │      Your Task Signal       │
                    │   (spoken naturally, no     │
                    │    slashes, no /commands)   │
                    └──────────────┬──────────────┘
                                   │
                                   ▼
                    ┌─────────────────────────────┐
                    │    Claude Code Native       │
                    │    Auto-Dispatch Engine     │
                    └──────────────┬──────────────┘
                                   │
                        ┌──────────┴──────────┐
                        │                     │
                        ▼                     ▼
           ┌────────────────────┐  ┌────────────────────┐
           │    SUB-AGENTS      │  │  MAIN-SESSION      │
           │  (.claude/agents/) │  │     SKILLS         │
           │                    │  │    (skills/)       │
           │  Isolated context, │  │                    │
           │  preloaded skills  │  │  Invoked live by   │
           │                    │  │  Skill tool based  │
           │                    │  │  on description    │
           │                    │  │  matching          │
           └─────────┬──────────┘  └─────────┬──────────┘
                     │                       │
                     ▼                       ▼
     ┌───────────────────────────┐   ┌───────────────────┐
     │ architect  (swarm boss)   │   │ decide, diagnose, │
     │ chen       (4 focus modes)│   │ adverse, 100,     │
     │ code-reviewer             │   │ zero, soft, st,   │
     │ grep-verifier             │   │ shutup, lossy,    │
     │                           │   │ code, session-    │
     │ Each runs in own context  │   │ start/end, and    │
     │ window. Each returns only │   │ gitnexus-*        │
     │ summary to main session.  │   │                   │
     └─────────────┬─────────────┘   └─────────┬─────────┘
                   │                           │
                   ▼                           ▼
           ┌───────────────┐           ┌───────────────┐
           │   Commit(s)   │           │ Task complete │
           │  via workflows│           │ in-session    │
           └───────┬───────┘           └───────┬───────┘
                   │                           │
                   └─────────────┬─────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────────┐
                    │     LEARNINGS captured      │
                    │   (feeds back to skills     │
                    │    and sub-agent memory)    │
                    └─────────────────────────────┘
```

Sub-agents handle personas with isolated context and preloaded domain skills. Main-session skills auto-invoke based on task signals. Architect owns swarm orchestration. Chen runs in four focus modes. You speak naturally — Claude Code routes.

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
