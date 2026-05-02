Debrief a Slack thread — get the full context, key perspectives, and what's expected of you.

The user will provide a Slack thread URL. Parse the channel ID and message timestamp from it.

Slack URL format: `https://<workspace>.slack.com/archives/<channel_id>/p<timestamp>`
- The channel ID is the segment after `/archives/`
- The message timestamp is the `p` value with a dot inserted before the last 6 digits

Instructions:
1. Parse the channel ID and message timestamp from the provided Slack URL.
2. Read the thread using `slack_read_thread` with the channel ID and message timestamp.
3. Read the FULL thread (set a high limit).
4. Scan for linked resources (Google Docs, GitHub PRs/issues, Jira tickets, etc.).
5. Produce a summary:

---

**Topic**: One-line summary.

**Context**: 2-3 sentences on background.

**Key Perspectives**:
- **[Person Name]**: Their stance (1-2 sentences)

**What's Expected of You**: Action or input needed, deadlines.

**Linked Resources**: Docs, PRs, tickets referenced.

---

Important:
- Always read the full thread, not just the first few messages.
- Focus on surfacing disagreements or open questions.
- If the user wasn't explicitly tagged, note that and summarize generally.
