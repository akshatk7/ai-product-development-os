---
name: morning-sync
description: Run the morning knowledge base sync — pull all context from the previous business day(s) and propose repo updates. Triggered by /morning-sync or "let's sync".
---

Daily sync that scans Slack, meetings, and DMs, then proposes targeted repo updates.

**Before starting:** Read `gotchas.md` and `runs.log` in this skill folder. Apply known failure patterns.

## Configuration

Channel registry, meeting patterns, and DM settings live in `config/morning-sync.md`. Read that file for the full source list.

## Phase 1: Gather

### Lookback Window
- **Default:** Previous business day (Monday = Friday, otherwise = yesterday)
- **Custom:** PM can specify: "past 2 days", "Wednesday", "Mar 10-12"
- **Date validation:** If PM provides both a day name and a date (e.g., "Thursday, March 20"), verify they match. If they conflict, ask — don't silently use one over the other.

### Parallel Agent Pattern
Launch these concurrently in a single message:
1. **Granola agent** — list meetings, ask PM for triage, then pull transcripts + notes for approved meetings
2. **Core channels agent** — scan all core channels + thread replies
3. **Project channels agent** — scan all project + monitor channels + thread replies
4. **DM agent** — scan DMs for the lookback window
5. **Auto-discovery agent** — search for new channels not in config

Do NOT scan channels sequentially. If a scan takes more than 5 minutes, something is wrong.

### Thread Scanning
`slack_read_channel` with `oldest`/`latest` only returns top-level messages. Thread replies to older messages are missed. After reading channels, also search for thread replies posted in the lookback window via `slack_search_public_and_private` with `is:thread` + timestamp filters.

## Phase 2: Classify & Synthesize

### Pre-Synthesis (MANDATORY)
1. Re-read `gotchas.md` — cross-check proposed updates against known failure patterns.
2. Read `runs.log` — check what the last run covered.

### Chronological Synthesis
Merge ALL inputs (meetings, Slack, DMs) into a single chronological timeline per project. A meeting at T0 and its Slack follow-up at T1 are one narrative. The timeline is internal working material — NOT shown to the PM.

### Deduplication
Before proposing any update, **search the full repo** for existing files on the topic. Use grep for key terms. Existing coverage may live in `data-science/`, a different project folder, or `reference-docs/`. Propose appending to existing files, not creating new ones.

### Filtering
- **Eng/DS-only items:** If purely engineering or DS backlog with no product implications, don't surface it. Eng and DS update the repo themselves.
- **DM noise:** Don't surface casual chat, logistics, repo access requests, tooling tips, or passing mentions. Only extract concrete decisions, timeline changes, staffing changes, and items the PM explicitly asked someone to do.

### Status Freshness Check

After classifying all items, identify every project that was mentioned in today's sources. For each:
1. Read the project's `README.md` and `CONTEXT.md` header (status, phase, last updated).
2. Compare against what was discussed. Does the status match reality?
3. If the status is stale (e.g., says "Designs" but meeting discussed eng implementation), add a **Status update** item to the proposals.
4. Also check `Last updated` — if more than 14 days old and the project was actively discussed, flag it.

### INDEX.md Drift Check

After the status freshness check, cross-check `projects/INDEX.md` against the actual README.md Phase field for every project mentioned today. If INDEX.md shows a different phase than the project's README.md, flag it in the report as an INDEX drift item. This prevents INDEX.md from going stale between monthly hygiene runs.

### File Routing Enforcement

When routing content to project files, follow the file boundary rules in `.claude/rules/document-conventions.md`:
- Decisions → `decisions.md` (NOT inline in CONTEXT.md or updates.md)
- Design flows/copy → `designs.md` (NOT in CONTEXT.md)
- System/data findings → `truths.md` (NOT in CONTEXT.md)
- Design WIP items that will come via Figma → skip (don't track intermediate design explorations)
- **Chronological meeting content, status changes, action items → `updates.md`** (NOT as dated sections in CONTEXT.md)
- CONTEXT.md is for **narrative context only** — what we're building, why, current state, scope. No dated "## March X Updates" sections.
- If an update changes the project's current state (new phase, revised approach, scope change), update CONTEXT.md's narrative inline (status line, approach section) — but do NOT append a dated section. The dated content goes in updates.md.
- Create `updates.md` on first use — don't pre-create empty files.

## Phase 3: Report

The report is **proposed updates only**. The PM reviews diffs, not event logs.

```
## Proposed Updates

### [#]. [Project Name] — `[target file path]` ([create/append/update])
[1-3 sentence synthesized description of what to add or change.]
```

**Rules:**
- Each item approvable/rejectable in seconds. No timelines, no source tables.
- Group related updates under the same project even if from different sources.
- "Needs Your Input" section only for items that genuinely need a PM routing decision. Keep the bar high.
- If the report is longer than a page, it's too long.

## Phase 4: Execute

After PM approves:
1. `git pull` to ensure latest state.
2. Make approved updates. Append/merge — never overwrite teammates' content.
3. Cross-check for stale or contradictory info in updated files.
   - If you created or updated a DS analysis, check `data-science/INDEX.md` — add it if missing.
   - If you updated a project file, check `projects/INDEX.md` — update the phase if it changed.
4. Commit with descriptive message. Ask before pushing.

## Post-Run
Append to `runs.log`. If new failure patterns discovered, add to `gotchas.md`.
