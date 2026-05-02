# Weekly Meeting Prep — Workflow

**Owner:** [PM Name]
**Last updated:** 2026-04-23
**Purpose:** Automate preparation of the team weekly meeting doc every Monday morning.
**Trigger:** "write top of mind for [date]"

> This is the canonical weekly prep skill. If your repo has a `.claude/skills/weekly-status/` skill, prefer this one — the mismatch-detection logic is folded into Step 5.0 below.

---

## The Document

**Google Doc:** [YOUR_WEEKLY_DOC_URL]
- **Doc ID:** `[YOUR_DOC_ID]`

Every week a new "Top of Mind" section is added just below the standing sections.

---

## Phase 0 — Pre-run checklist (mandatory)

1. Read `gotchas.md` — every known formatting/content failure pattern.
2. Read `runs.log` — check what the last run covered and what feedback was given.
3. Load `config/repo-hygiene.json`:
   - `project_categories` is the source of truth for section scoping (used by Phase 9 linter).
   - `freshness_sla_days` tells you which projects need hygiene flags.
4. Read `roadmap/strategic-initiatives.md` — ongoing items that don't generate daily Slack pings but must appear in Top of Mind when `status: active`.

---

## Workflow (run Sunday or Monday)

### Step 1: Read previous week's entry
- Read the most recent entry from the Google Doc.
- This is the baseline — items roll forward with updated statuses.

### Step 2: Read repo project files for updates (morning sync handles Slack/meetings)
Since `/morning-sync` runs daily, the repo already has Slack and meeting context routed into project files. This step is pure repo-read.

- Read all active project READMEs, CONTEXT.md, updates.md, and decisions.md for changes since last Monday.
- **Git-diff filter (mandatory):** For each project, run `git log --since="last monday" --until="friday" --format='%h %s' -- projects/<cat>/<project>/`. If a project has **zero commits** in the window AND its README doesn't have a `monitoring: yes` marker AND it's not in `roadmap/strategic-initiatives.md` with `status: active`, **omit from Top of Mind**. Rationale: running work with no updates should not get a line.
- **Fallback:** If morning sync was missed for any day, do a supplementary gather for those days only.

### Step 2.0: Read strategic-initiatives register
- Read `roadmap/strategic-initiatives.md`.
- Auto-include every item where `status: active` in the appropriate Top-of-Mind section (per its `section:` field).
- Flag any `active` item where `last_meaningful_update` is older than `2× cadence_days` as a Hygiene warning.
- This captures items (big bets, adoption strategy, cross-team alignments) that don't generate daily Slack pings but need weekly visibility.

### Step 3: Check calendar
- Pull meetings for the upcoming week.
- Look for: brief/design/ship reviews, cross-team syncs, planning sessions.
- Include relevant reviews inline in Discussion Topics (not as a separate section).

### Step 4: Cross-reference external sources
If your team has an ML roadmap spreadsheet, eng Gantt, or similar authoritative source of truth for timelines:
- Pull the latest from each.
- Compare against repo project statuses.
- Flag discrepancies.

### Step 5: Update the roadmap spreadsheet (approval-gated write)
If your team writes back to a roadmap spreadsheet:

**Mandatory: proposed-changes table MUST appear in the draft before any cell is written.** Include a dedicated "### Roadmap spreadsheet changes (pending approval)" section with this format:

```
| Cell | Column | Old | New | Reason | Source |
|------|--------|-----|-----|--------|--------|
| F11 | Status | On Track | Blocked | Dependency on X | Meeting 4/22 |
| G11 | GTM    | Q2 2026 | Late Q2 | Week 1 results strong | Readout 4/21 |
```

- No writes happen until the user replies with approval ("approved" / "approve F11 only" / "reject all").
- When sources conflict (e.g., ML tab vs morning-sync data captured mid-week), prefer the most recent. Flag the conflict in the Reason column.

### Step 5.0: Cross-reference mismatch detection
Run these cross-refs and list any mismatches as Hygiene warnings:
- Projects in one authoritative tab but not in the roadmap (or vice versa)
- Status discrepancies between sources
- Projects with no git activity this week but "On Track" status
- Overdue ETAs (launch date passed, status not updated)

### Step 6: Draft the new entry
- Follow the template below.
- **Roll-forward discipline:** Pull each item from last week's entry. For each, check the git-diff filter (Step 2) and the timeline. Mark each as:
  - `[changed]` — new info this week → include with updated status
  - `[unchanged]` — no activity → **omit entirely** (zero-liners > one-liners)
  - `[complete]` — has shipped or closed → omit, archive reference if needed
  - `[monitoring]` — live experiment, no new data, but status worth confirming (max 1 line) — only if README has `monitoring: yes` marker
- Add any NEW items from morning-sync, strategic-initiatives, or calendar reviews.

### Step 7: Save to local markdown
- Save as `meetings/weekly/MMDDYY.md` (e.g., `030226.md`).
- Show draft to PM for approval before inserting.

### Step 8: Insert into Google Doc
- After approval, insert below standing sections, above the previous week's entry.

### Step 9: Pre-output linter (run before presenting draft)
Fix in place. Do not present a draft that fails any:

1. **Section scoping lint** (hard error) — for every project listed under a section header, verify its folder path matches the category in `config/repo-hygiene.json` → `project_categories[X].path`. Example: an "ML / Algorithmic" section can only include projects under `projects/ml/`. If a project appears in the wrong section, MOVE it before presenting.
2. **Discussion topics cap** (hard error) — max 5 items. Trim the lowest-signal items first.
3. **One-liner discipline** (warn) — any project bullet > 120 chars? Flag for compression.
4. **Duplication check** (hard error) — no project name appears in both "Discussion topics" AND "Project updates." If a project is a discussion topic this week, don't also list it as a routine update.
5. **No-change items** (hard error) — grep the draft for items with no content after the colon ("Project X:") — these are rollovers that should have been dropped in Step 6.
6. **Established-fact repetition** (warn) — if the same fact was stated last week, flag for removal. Re-state only if new evidence.
7. **Strategic items covered** — every `status: active` item in `roadmap/strategic-initiatives.md` is either in the draft or explicitly filtered. Anything active that's missing → add it.
8. **Approval table present** — if any proposed cell change, there's a `### Roadmap spreadsheet changes (pending approval)` section.
9. **Experiment-decision keywords** — no "winner"/"decided" claim without a `confirmed`-tier keyword from source transcript (see `config/repo-hygiene.json` → `experiment_decision_keywords`).

---

## Top of Mind Template

```
Top of Mind [M/DD]

Discussion topics:
- Metrics update
- [Specific review this week]: [date] at [time] ET
- [Any ad-hoc discussion items needing decision/alignment]

Project updates:

[Section 1 per project_categories]:
- [Project Name]: [status change + 1-line context]

[Section 2]:
- [Project Name]: [status change + 1-line context]

Strategic:
- [Ongoing initiative from strategic-initiatives.md]: [weekly note]

Design priorities:
- [Current design workstream needing attention]
```

---

## Dynamic Project Discovery

Do NOT use a hardcoded project list. At runtime:

1. `find projects -name "README.md" -not -path "projects/_template/*" -not -path "projects/_example/*"`
2. Read the first ~20 lines of each README for: project name, phase, target launch.
3. Apply Step 2 git-diff filter.
4. Cross-reference against the roadmap spreadsheet for discrepancies.

---

## Conventions

- Date format in headers: M/DD (e.g., "Top of Mind 3/2")
- Entries reverse chronological (newest at top)
- Names referenced by first name
- Reviews listed with date, name, and time in ET
- Experiment statuses: launching / live / complete / readout
- Open questions phrased as direct asks ("launch date?", "Any call outs?")

### Formatting preferences (enforced by Step 9 linter)

- **Section scoping is enforced by `project_categories`.** Projects NEVER appear under the wrong section. Source of truth: `config/repo-hygiene.json`.
- **No separate "Upcoming reviews" section.** Merge specific reviews into Discussion Topics as bullets. Don't list standing meetings unless a specific project is being reviewed.
- **Each major workstream gets its own top-level bullet.** Keep the hierarchy flat.
- **Omit projects with no change since last week.** Enforced by the git-diff filter.
- **Discussion topics capped at 5.**
- **Technical decisions and individual work allocation stay out.** Those belong in project decisions.md. The weekly sync is about status, blockers, discussion items.
- **Don't repeat established facts.** Once the team knows something, stop restating it.
- **Design priorities: only items needing attention.** Don't list things already in progress.

---

## Post-Run

Append to `runs.log`. Track `user_rejection_rate` (approved / proposed). If > 30% for two consecutive runs, stop and analyze — don't keep shipping a broken draft format.

If a new failure pattern surfaced, add to `gotchas.md` immediately.
