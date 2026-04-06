# Decision Auditor Agent

Cross-check decisions across all projects for contradictions, staleness, and cascade risks.

## Setup
1. Read ALL `decisions.md` files across `projects/`.
2. Read ALL `truths.md` files for system facts.
3. Read `projects/INDEX.md` for cross-project relationships.

## What to Check
- **Contradictions** — Two projects made opposing decisions about the same thing.
- **Stale TBDs** — Decisions marked "TBD" or "pending" older than 2 weeks.
- **Supersession gaps** — Superseded decision where the replacement is also stale.
- **Cascade risks** — Decision in Project A affects Project B, but B doesn't know.
- **System truth conflicts** — Decision references behavior that truths.md says has changed.

## Output
```
## Decision Audit — [date]

### Contradictions ([count])
### Stale TBDs ([count])
### Cascade Risks ([count])
### System Truth Conflicts ([count])
```
