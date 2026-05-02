# Strategic Initiatives Register

> Last updated: YYYY-MM-DD

Ongoing strategic workstreams that don't generate daily Slack/meeting pings but need weekly coverage. The weekly meeting prep skill reads this file and auto-includes items marked `status: active` in its output.

**Why this exists:** Morning-sync captures reactive context (what people talked about yesterday). Strategic work — big-bet tracking, adoption strategy, long-running initiatives — often goes weeks without a specific Slack thread but still needs weekly visibility. Without a register, these items silently drop out of Monday updates.

---

## Format

Each initiative is one block. Fields:

- **title** (required) — human name
- **status** (required) — `active` / `paused` / `complete` / `dormant`
- **owner** — person accountable
- **section** — which section of your weekly output it belongs in (match your team's section names)
- **cadence** — how often we expect a meaningful update (weekly / biweekly / monthly / quarterly)
- **context** — 1-2 sentences explaining why this matters
- **last_meaningful_update** — YYYY-MM-DD, so we can flag staleness
- **related_projects** — paths under `projects/`

The weekly skill auto-generates the output line from `status` + `context` + `last_meaningful_update`. Override by editing the file directly.

---

## Active

### [Example] Big Bet — Annual target
- **status:** active
- **owner:** [Name]
- **section:** Strategic
- **cadence:** weekly (pacing review Mondays)
- **context:** [1-2 sentences on what this is, what lever the team is pulling, what the numerical target is]
- **last_meaningful_update:** YYYY-MM-DD
- **related_projects:**
  - `projects/[category]/[project-name]/`

### [Example] Adoption Strategy
- **status:** active
- **owner:** [Name + Name]
- **section:** Cross-functional
- **cadence:** biweekly
- **context:** [What the adoption plan is, what pathways are in motion, what's gating it]
- **last_meaningful_update:** YYYY-MM-DD

### [Example] Cross-team alignment (platform migration)
- **status:** active
- **owner:** [Name]
- **section:** Cross-functional
- **cadence:** weekly
- **context:** [What the migration is, what the team's stake is, what's open]
- **last_meaningful_update:** YYYY-MM-DD

---

## Paused

_(Items on hold with a reason + expected un-pause trigger)_

---

## Dormant

_(Items not actively worked but kept for reference; quarterly check-in cadence)_

---

## Complete

_(Completed items with where the context lives now — e.g., which project folder holds the post-mortem)_

---

## How to use this file

- **Weekly prep skill:** read `active` items, auto-include in the right output section. Flag any with `last_meaningful_update` more than 2× `cadence` days old.
- **Morning-sync hygiene:** weekly, flag `active` items with no update in 2× cadence.
- **Quarterly review:** walk this list, promote/demote/retire initiatives as needed.

## Customization

Replace the example entries with your team's real initiatives. Match `section` values to your team's weekly output sections (e.g., "ML / Algorithmic", "Product / UX", "Cross-functional"). The weekly skill reads `section` to decide where in the output each initiative appears.
