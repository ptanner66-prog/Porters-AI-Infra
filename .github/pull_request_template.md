## What

_One paragraph: what change does this PR make?_

## Why

_One paragraph: what problem does it solve, or what requirement does it satisfy? Reference the relevant task, ticket, or inviolable rule if applicable._

## How

_Key implementation decisions. What was the approach? What alternatives were considered and rejected?_

## Testing

- [ ] Type checker passes (`npx tsc --noEmit` or equivalent)
- [ ] Relevant tests updated or added
- [ ] Tests pass (`npm test` or equivalent)
- [ ] Manual verification: [describe what you tested]
- [ ] Zero-tolerance scan clean (if applicable)

## Breaking Changes

_Does this PR change any public APIs, data schemas, or event contracts? If yes, list them and describe migration path._

## Deploy Notes

_Any steps required at deploy time: migrations, env var changes, feature flags, cache invalidation, service restarts._

## Checklist

- [ ] Branch check: not committing directly to `main`/`master`
- [ ] Dead code check: not editing any known-dead files
- [ ] GitNexus impact: ran `gitnexus_detect_changes()` and results match expected scope
- [ ] No inviolable rules violated (see CLAUDE.md → Inviolable Rules)
