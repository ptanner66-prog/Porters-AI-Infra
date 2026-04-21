# BACKLOG.md

<!-- provenance: template from Motion-Granted-Production/BACKLOG.md -->

**Purpose:** Living backlog of out-of-scope issues found during development sessions. Claude Code adds items here when it spots problems that shouldn't be fixed in the current task. Humans review and prioritize.

---

## How to add items

When you discover an issue that is out of scope for the current task:

1. Add it to the appropriate category below.
2. Use this format:
   ```
   - [ ] **Short title** — One-line description of the problem.
     _Found: YYYY-MM-DD | Context: what you were doing when you found it_
   ```
3. Do NOT fix it. Do NOT expand scope. Just log it and move on.

---

## Cleanup / Organization

- [ ] **F5 (v0.2 polish) — Rename project template file for clearer signaling** — `docs/project-claude-md-template.md` currently disambiguates its role via prose in CLAUDE.md §9. A rename to something like `docs/downstream-project-CLAUDE-template.md` or `docs/project-CLAUDE-md-starter.md` would make the role obvious from the filename alone. Non-blocking.
  _Found: 2026-04-21 | Context: MODE 3 verification report (VERIFICATION.md F5)_

## Technical Debt

_(empty — add items as discovered)_

## Bugs

_(empty — add items as discovered)_

## Feature Requests

_(empty — add items as discovered)_

## Security

_(empty — add items as discovered)_

## Documentation

- [ ] **F2 (v0.2 polish) — Extend dependency graph in architecture.md** — `docs/architecture.md § Dependency Graph (Current)` lists CLAUDE.md, AGENTS.md, LEARNINGS.md, BACKLOG.md, rules, commands, settings, personas, skills — but omits `workflows/*.md` and `docs/*.md`. Add two rows so the graph matches reality.
  _Found: 2026-04-21 | Context: MODE 3 verification report (VERIFICATION.md F2)_

- [ ] **F3 (v0.2 polish) — Tighten Karpathy §8.3 enforcement mapping** — In root CLAUDE.md §8.3 (Surgical Changes), the current mapping lists `/adverse` integration-wiring check. `/adverse` is about completeness coverage, which is a looser fit than the principle requires. Replace with: Code Reviewer's blast-radius + scope-drift checks, Chen's "do not propose rewrites when precise fixes are available" directive, and `/100`'s "no fix without proven root cause" rule.
  _Found: 2026-04-21 | Context: MODE 3 verification report (VERIFICATION.md F3)_

- [ ] **F6 (v0.2 polish) — Add rename-a-persona guidance to extending.md** — `docs/extending.md` covers adding personas/skills/commands/rules/workflows but does not cover renaming an existing persona. For a downstream adopter who wants to rebrand (e.g., rename Chen to Auditor for client optics), a short subsection walking through the rename sequence (file rename, frontmatter `name:` field, skill-wrapper rename, CLAUDE.md §3 link updates, grep sweep for the old name) would be operator-friendly.
  _Found: 2026-04-21 | Context: MODE 3 verification report (VERIFICATION.md F6)_

- [ ] **F8 (v0.2 polish) — Consider per-directory INDEX.md files** — `personas/`, `skills/`, `workflows/` directories lack individual index files. Fresh sessions browsing the directory tree directly (bypassing the CLAUDE.md teaching chain) don't get an at-a-glance catalog. CLAUDE.md §3 and §4 already provide the index, so this is optional — only worth doing if the duplication is a net positive for some future user.
  _Found: 2026-04-21 | Context: MODE 3 verification report (VERIFICATION.md F8)_
