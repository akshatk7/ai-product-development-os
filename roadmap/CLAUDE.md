# Roadmap

## Sources

| Source | Location | Notes |
|--------|----------|-------|
| Roadmap spreadsheet | [YOUR_SPREADSHEET_URL] | Living source of truth for project statuses, timelines, and DRIs |
| Eng timeline / Gantt | [YOUR_GANTT_URL] | Week-by-week eng allocation |
| [Other tracker] | [URL] | [What it tracks] |

Spreadsheet details (for agent automation):
- **Spreadsheet ID:** `[YOUR_SPREADSHEET_ID]`
- **Tab:** `[YOUR_TAB_NAME]`
- **Key columns:** Project Name, Status, Estimated Launch, DRI, Current Notes

## Update Workflow

PM prompts "update the roadmap" → agent gathers latest status from Slack, meeting notes, and project READMEs → agent shows proposed diff with rationale → PM approves → agent writes to spreadsheet.

**Rules:**
- Never silently update the spreadsheet. Always show proposed changes first.
- Verify statuses against Slack and meeting notes before proposing changes.
- Do not assume launch dates — confirm from primary sources (eng, project channels).
- If a new project has emerged that is not in the spreadsheet, propose adding a new row.
