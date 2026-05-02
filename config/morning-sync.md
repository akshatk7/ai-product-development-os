# Morning Sync Configuration

Source registry for the `/morning-sync` skill. Defines what to scan and where to route extracted information.

**Maintained by:** [PM Name]

---

## Slack Channels

### Core Channels (always scan — route by topic to relevant project)

| Channel | ID | Notes |
|---------|-----|-------|
| #[your-team-main] | [CHANNEL_ID] | Main team channel |
| #[your-team-eng] | [CHANNEL_ID] | Eng + cross-functional |

### Project Channels (scan + auto-route to mapped project)

| Channel | ID | Maps to | Auto-prune |
|---------|-----|---------|------------|
| #proj-[project-1] | [CHANNEL_ID] | projects/[area]/[project-1]/ | 30 days |
| #proj-[project-2] | [CHANNEL_ID] | projects/[area]/[project-2]/ | 30 days |

### Monitor-Only Channels (not your project, but useful context)

| Channel | ID | Notes |
|---------|-----|-------|
| #[partner-team-channel] | [CHANNEL_ID] | [Why you're monitoring this] |

---

## DMs

Scan the PM's DMs from the lookback window. PM's Slack user ID: `[YOUR_SLACK_USER_ID]`.

- Skip clearly irrelevant DMs (personal, unrelated teams).
- Include anything with potential relevance — let PM decide routing.

---

## Meeting Patterns

| Meeting name pattern | Primary output | Secondary updates |
|---------------------|----------------|-------------------|
| [Team Weekly] | meetings/weekly/[date].md | Relevant project updates.md, decisions.md |
| [Eng Standup] | Relevant project updates.md | — |
| [Design Review] | Relevant project designs.md, updates.md | — |
| [Leadership Forum] (if leadership-forum add-on enabled) | meetings/leadership-forum/[date]/ | product/overview.md, roadmap/, relevant project updates.md |
| [DS Sync] (if data-science add-on enabled) | Relevant project truths.md | data-science/ if analysis discussed |

For meetings not matching any pattern: surface in report with extracted content and let PM decide routing.

---

## Rules

### Lookback Window
- **Default:** Previous business day (Mon morning = Friday, otherwise = yesterday)
- **Custom:** PM can specify: "past 2 days", "Wednesday", "Mar 10-12"

### Deduplication
- Before proposing any update, read the target file's current content.
- If the information is already captured (by a teammate or a prior run), mark as "already captured" and skip.
- Do NOT overwrite content others have written.

### Auto-Prune
- Project channels with 0 messages in the lookback window AND no messages in the past 30 days: flag in report as "Consider removing from scan."
- Core channels are never auto-pruned.

### Channel Auto-Discovery
At the start of each run, search for channels the PM is a member of using relevant keyword patterns (project names, team names). Compare against this config. For new channels not listed here, include in the scan for this run and flag: "New channel discovered: #channel-name. Should I add to config?"
