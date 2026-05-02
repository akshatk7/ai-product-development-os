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

Every project README.md must have a Status block. Enforced by pre-commit hook (`.githooks/pre-commit` check #8).

**Standardized header format** (format is linted — don't drift):
```
# Project Name

## Status
- **Phase:** [phase]
- **Status:** [one-line current state]
- **Last updated:** YYYY-MM-DD
- **Team:** [PM, Eng, Design, DS as applicable]
```

Valid phase values (prefix-matched, case-insensitive): `Exploratory`, `Product Brief`, `Designs`, `Feasibility`, `RFCs`, `Coding`, `Live`, `Complete`, `Proposal`. Descriptors after the phase word are allowed (e.g., `Coding (PRD complete, impl in progress)`).

### Freshness SLA (by phase)

How stale `Last updated` can be before morning-sync flags the project in its daily hygiene section. Thresholds live in `config/repo-hygiene.json` → `freshness_sla_days`:

| Phase | Max days stale |
|---|---|
| Live | 7 |
| Coding | 14 |
| RFCs | 14 |
| Feasibility | 21 |
| Designs | 21 |
| Product Brief | 30 |
| Exploratory / Proposal | 60 |
| Complete | 180 |

**When you touch a project file for any reason, check if the status/phase is still accurate.** If it's wrong, fix it. Mandatory.

### Lifecycle-aware file requirements

When a project's phase crosses a threshold, required files must exist (enforced as a pre-commit WARNING — escalates to ERROR once the repo is fully compliant). Thresholds live in `config/repo-hygiene.json` → `phase_required_files`:

| Phase | Required files |
|---|---|
| Designs | `product-brief.md` |
| Feasibility | `product-brief.md` |
| RFCs | `product-brief.md` + (`rfc-frontend.md` OR `rfc-backend.md`) |
| Coding | `updates.md` + (`rfc-frontend.md` OR `rfc-backend.md`) |
| Live | `updates.md` + `decisions.md` |

Rationale: prevents "we're in Coding but have no updates.md" drift — forces seeding at phase-transition time instead of during monthly hygiene.

---

## Cross-Reference Requirements

> Rules below mentioning `data-science/` apply when the DS add-on is enabled (i.e., `data-science/` exists at the repo root). Otherwise skip.

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

## Automated Hygiene Enforcement

The repo runs four layers of hygiene enforcement.

### Layer 1 — Pre-commit hook (`.githooks/pre-commit`)
Runs on every `git commit`. Reads `config/repo-hygiene.json`. Blocks on:
- Root files/dirs outside the allowed list
- `CLAUDE.md` > 150 lines
- Dated section headers in `CONTEXT.md`
- Files > 500KB (non-binary)
- New `SKILL.md` without `gotchas.md` + `runs.log`
- `.DS_Store` additions
- Project README missing Phase/Status/Last updated/Team fields

Warns (commit proceeds) on:
- Project README changed without `projects/INDEX.md` staged
- Phase crossed a threshold but required files missing
- `beliefs.md` staged with no `Last challenged` fields

**To update a rule:** edit `config/repo-hygiene.json`, not the hook.

### Layer 2 — CI workflows (`.github/workflows/`)
- `repo-hygiene.yml` — reads the same config, blocks pushes/PRs that break structural rules.
- `link-check.yml` — runs [lychee](https://github.com/lycheeverse/lychee-action) on every push, PR, and weekly. Flags broken internal markdown links. External auth-gated domains are excluded.

### Layer 3 — Morning-sync daily hygiene
Every morning-sync run ends with a "Hygiene" section that surfaces:
- Projects with `Last updated` beyond the phase SLA (see table above)
- `projects/INDEX.md` phase mismatches vs individual READMEs
- Open strategic questions whose answer just landed in `data-science/INDEX.md`
- Beliefs with `Last challenged` older than 60 days
- Orphaned files (not referenced by any INDEX, README, or sibling doc)
- Strategic initiatives with stale `last_meaningful_update`

User approves/skips each item. Zero tax if nothing's off.

### Layer 4 — Monthly audit agent (scheduled)
Once a month — schedule via `/schedule` with the prompt in `.claude/agents/monthly-audit.md`. Runs the full deep audit, opens a single PR with proposed fixes.

---

## Monthly Repo Hygiene Checklist (manual backstop)

The checklist below is the manual backstop if the monthly audit agent isn't scheduled or has missed a month.

1. **CLAUDE.md tree** — matches the actual file system?
2. **projects/INDEX.md** — every project folder listed with current phase?
3. **data-science/INDEX.md** — every analysis listed?
4. **Orphaned files** — any files not referenced from anywhere?
5. **updates.md presence** — every project past Design phase has one?
6. **strategy/beliefs.md** — beliefs to retire? Stale `Last challenged` dates?
7. **strategy/open-questions.md** — questions resolved by recent readouts?
8. **Broken links** — link-check workflow green?

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

---

## RFC Workflow Integration

When an RFC (frontend or backend) exists for a project:

- **After creating or updating a PR**, update the PR link and status in the RFC task breakdown table. If your team uses a ticket tracker (Jira, Linear), transition the ticket status to "In Review."
- **Always use full PR URLs**, never `#number` shorthand. AI tools render `#12345` as a link to the current repo's issues -- which is the KB repo, not the source code repo. Write `https://github.com/your-org/your-repo/pull/12345`.

---

## Data Output Policy

Analysis code (Python, SQL, shell scripts) belongs in the repo. Data outputs (CSVs, large HTML reports, generated images) do NOT -- they bloat git history permanently.

- Store data outputs in your data warehouse or Google Drive
- Link from documentation, don't commit
- The pre-commit hook blocks files over 500KB (except PDFs and images)
- Small reference CSVs (<100 rows) can be force-added with `git add -f`
