# Weekly Meeting Prep — Workflow

**Owner:** [PM Name]
**Purpose:** Automate preparation of the team weekly meeting doc every Monday morning.
**Trigger:** "write top of mind for [date]"

---

## The Document

**Google Doc:** [YOUR_WEEKLY_DOC_URL]
- **Doc ID:** `[YOUR_DOC_ID]`

Every week a new "Top of Mind" section is added just below the standing sections.

---

## Workflow (run Sunday or Monday)

### Step 1: Read previous week's entry
- Read the most recent entry from the Google Doc
- This is the baseline — items roll forward with updated statuses

### Step 2: Pull meeting notes (Mon-Fri of prior week)
- Use meeting notes tool (Granola, Otter, or manual) to find all meetings from the prior work week
- Extract: decisions, status updates, blockers, new items relevant to the team

### Step 3: Check Slack channels
- Read the full channel list from `config/morning-sync.md`
- Use the same lookback window (previous business week)
- Extract: project updates, experiment launches, blockers, cross-team discussions, decisions

### Step 4: Check calendar
- Pull meetings for the upcoming week
- Look for: brief/design/ship reviews, cross-team syncs, planning sessions
- Include relevant meetings with dates and times in the Discussion Topics section

### Step 5: Update the roadmap spreadsheet
- The roadmap is an OUTPUT — we write TO it, not read FROM it
- Compare repo project statuses against the spreadsheet
- Update any stale cells (status, estimated launch, current notes)
- Show proposed changes to the PM before writing

### Step 6: Draft the new entry
- Follow the template below
- Roll forward items from last week with updated statuses
- Add new items that surfaced during the week

### Step 7: Save to local markdown
- Save as `meetings/weekly/MMDDYY.md` (e.g., `030226.md` for March 2, 2026)
- Show draft to PM for approval before inserting into Google Doc

### Step 8: Insert into Google Doc
- After approval, insert the entry below the standing sections, above the previous week's entry

---

## Top of Mind Template

```
Top of Mind [M/DD]

Discussion topics:
- Metrics update
- Upcoming reviews:
  - [date]: [review name] — [time] ET
- [Any ad-hoc discussion items]

Project updates:

[Area 1]:
- [Project Name]: [status]
- [Project Name]: [status]

[Area 2]:
- [Project Name]: [status]

Design priorities:
- [Current design workstream + status]
```

---

## Dynamic Project Discovery

Do NOT use a hardcoded project list. At runtime:

1. Run `find projects -name "README.md" -not -path "projects/_template/*" -not -path "projects/_example/*"`
2. Read the first ~20 lines of each README to extract: project name, phase, target launch
3. Use these live statuses to populate the project updates section
4. Cross-reference against the roadmap spreadsheet for discrepancies

---

## Conventions

- Date format in headers: M/DD (e.g., "Top of Mind 3/2")
- Entries are reverse chronological (newest at top)
- Keep each project update to 1-2 lines unless something significant happened
- Only include meetings relevant to the full working team (not 1:1s or cross-team syncs)
- Each major workstream gets its own top-level bullet — keep hierarchy flat
