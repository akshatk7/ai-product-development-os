---
paths: ["**"]
---

# Document Conventions

## File Boundary Rules

Each project file has a specific purpose. Do NOT dump everything into CONTEXT.md. Route content to the correct file:

| Content Type | Target File | NOT here |
|---|---|---|
| Project overview, phase, team, key links | `README.md` | — |
| Narrative context, source doc links, problem statement, scope, current approach | `CONTEXT.md` | No dated sections, no chronological updates, no meeting-by-meeting notes |
| Chronological updates, meeting digests, status changes, action items | `updates.md` | Don't put dated "## March 16 Updates" sections in CONTEXT.md |
| Finalized decisions (numbered, dated, with rationale) | `decisions.md` | Don't leave decisions inline in CONTEXT.md or updates.md |
| UI/UX flows, modal copy, layout descriptions, Figma use cases | `designs.md` | Don't put design content in CONTEXT.md |
| Data table findings, API structures, system behavior, code paths | `truths.md` | Don't put eng findings in CONTEXT.md |
| Frontend technical design | `rfc-frontend.md` | — |
| Backend technical design | `rfc-backend.md` | — |
| Exploratory work, discarded options, scratch artifacts | `scratchpad.md` | — |
| Cross-cutting hypotheses about customers, product, market | `strategy/beliefs.md` | Don't put strategic hypotheses in project CONTEXT.md |
| Competitor/industry observations | `strategy/competitive.md` | Don't scatter competitive intel across project files |
| Unresolved strategic questions | `strategy/open-questions.md` | Don't leave big questions buried in meeting notes |

**CONTEXT.md is a narrative briefing, not a journal.** It should read like a project overview that a newcomer can scan in 2 minutes. It answers: what are we building, why, what's the current state, what's the scope. When an update changes the project's current state (new phase, revised approach, scope change), update the CONTEXT.md narrative inline — don't append a dated section.

**updates.md is the chronological log.** Newest entries first. Each entry is a dated section (e.g., `## 2026-03-27`) with meeting findings, status changes, and context that mattered at the time. Morning sync and meeting digestion append here. Completed action items can be pruned periodically.

**When you find content in the wrong file:** Move it (don't copy) and leave a one-line reference in the original: "See `decisions.md` for project decisions."

**Files are created organically.** Do not pre-create empty files. Create `updates.md`, `decisions.md`, `truths.md`, etc. only when there's content to put in them. Early-stage projects (pre-brief) may use CONTEXT.md as a catch-all until there's enough content to split.

**updates.md is seeded once Design phase completes.** When a project reaches or passes its Design phase, create `updates.md` if it doesn't exist. From that point, cross-functional handoffs, RFC reviews, and implementation decisions generate dated updates worth logging.

---

## Status Freshness

Every project README.md and CONTEXT.md must have:
- **`Last updated: YYYY-MM-DD`** in the header
- **`Status:`** one-line current state
- **`Phase:`** one of: Exploratory → Product Brief → Designs → Feasibility → RFCs → Coding → Live

**When you touch a project file for any reason, check if the status/phase is still accurate.** If it's wrong, fix it. This is mandatory — don't leave a stale status just because you were only editing decisions.md.

---

## Cross-Reference Requirements

- **DS analyses → project folders:** Every analysis in `data-science/` must link to the project(s) it informs. Add a "Projects informed" field. Also add the analysis to `data-science/INDEX.md`.
- **Project CONTEXT.md → DS analyses:** When a DS analysis informs a project, add a link in the project's CONTEXT.md source documents table.
- **Meeting notes → project folders:** When digesting a meeting, include project folder paths in the notes so readers can navigate to full context.
- **New files → INDEX:** When creating any new file in `data-science/` or `projects/`, check if the relevant INDEX.md needs updating.
- **Source doc links must include "Last verified" dates.** When reading a source Google Doc to update CONTEXT.md, add `(verified YYYY-MM-DD)` next to the link in the source documents table. This tracks link freshness without requiring re-reads.
- **Experiment readouts → open-questions.md:** When an experiment completes (readout available), check `strategy/open-questions.md` for questions the results can partially or fully resolve. Move resolved questions to the "Resolved" section with a date and pointer to where the answer lives.

---

## Beliefs Lifecycle

Beliefs in `strategy/beliefs.md` follow a lifecycle:

1. **Active** — Hypothesis with supporting/challenging evidence. Actively referenced in product work.
2. **Confirmed** — Sufficient evidence to treat as a working truth. Still listed in beliefs.md but marked `[Confirmed]` with pointer to the evidence.
3. **Disproven** — Evidence conclusively shows the belief was wrong. Moved to a `## Retired Beliefs` section at the bottom with date, evidence, and what replaced it.
4. **Superseded** — A newer, more precise belief replaced this one. Moved to `## Retired Beliefs` with pointer to the successor.

This prevents beliefs.md from growing indefinitely while preserving the learning record.

---

## Monthly Repo Hygiene (first Monday of each month)

During weekly meeting prep, also check:
1. **CLAUDE.md tree** — does it match the actual file system?
2. **projects/INDEX.md** — does it list all project folders with current phases? Cross-check each project's README.md Phase field.
3. **data-science/INDEX.md** — does it list all analyses?
4. **Orphaned files** — any files not referenced from anywhere?
5. **updates.md presence** — every project past Design phase should have updates.md. Seed it if missing.
6. **strategy/beliefs.md** — any beliefs not challenged in 30+ days? Flag for review. Any beliefs that should be retired (disproven or superseded)?
7. **strategy/open-questions.md** — any questions resolved by recent experiment readouts? Move to Resolved. Any questions sitting without progress? Flag for triage.
Flag issues in the weekly meeting prep report.

---

## Knowledge Propagation

When digesting new information (meeting notes, briefs, Slack, docs), check it against existing files (`product/overview.md`, `roadmap/`, relevant project CONTEXT.md, **`strategy/beliefs.md`**) and update any files that are now stale. Don't just accumulate — self-correct. **Always check strategy/beliefs.md** — if new data confirms, challenges, or invalidates a belief, update the evidence and confidence level.

---

## Conflict Resolution

When conflicting information is found across project files:
1. Surface both versions to the user.
2. Ask which is correct before proceeding.
3. Update all conflicting files after resolution.
`decisions.md` is likely most up-to-date but not automatically authoritative.
