---
paths: ["meetings/**"]
---

# Meeting Notes Conventions

- After digesting a meeting, always check the Meeting → File Mapping in CLAUDE.md to route information to the correct files.
- Meeting notes should reference project folder paths (e.g., `projects/my-project/`) so readers can navigate to full context.
- **Chronological meeting content → `updates.md`** in the relevant project folder. Do NOT append dated sections to CONTEXT.md.
- Decisions discussed in meetings should also be added to the relevant project's `decisions.md`.
- If a meeting reveals that a project's status has changed, update the project's README.md and CONTEXT.md status fields (inline narrative update, not a dated section).

---

## Meeting -> File Routing

When digesting a meeting, use this table to route information to the correct files. Customize this table for your team's meeting types.

| Meeting Type | Primary Output | Secondary Updates |
|---|---|---|
| Weekly team meeting | `meetings/weekly/[date].md` | Project CONTEXT.md, decisions.md, `strategy/beliefs.md` |
| Cross-functional design review | Relevant project CONTEXT.md | `product/review-taste.md`, `strategy/beliefs.md` |
| External partner syncs | Relevant project CONTEXT.md | `strategy/competitive.md` |
| Leadership/stakeholder sync (if leadership-forum add-on enabled) | `meetings/leadership-forum/[date]/` | `product/overview.md`, `roadmap/`, `strategy/beliefs.md` |
| DS/Analytics sync (if data-science add-on enabled) | `data-science/` (relevant analysis folder) | Project `truths.md`, `strategy/beliefs.md` |
