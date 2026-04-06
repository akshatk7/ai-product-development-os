# Engineering — Ways of Working

This file defines how the Engineering team works within this repo. It is a living document — update it proactively whenever meaningful learnings, patterns, or preferences emerge from eng work sessions.

Product seeds the structure; eng fills in the details.

---

## Key References

- **Eng Gantt / Timeline:** [YOUR_GANTT_URL]

## Team Structure

See `team/people.md` for the full eng org chart.

## For Engineers: What to Contribute

### 1. Rituals
Add your team's rituals to `team/rituals.md`:
- Meeting name, cadence, time, who attends, what doc/tool is used

### 2. Slack Channels
Add eng-specific channels to `team/channels.md`:
- Channel name, purpose, ID

### 3. RFCs
Add eng RFCs to the relevant **project folder** (e.g., `projects/{project}/rfc-backend.md`). RFCs are project-specific, not team-wide. Use the template from `projects/_template/`.

### 4. On-Call & Incidents
- **Runbooks** live in `engineering/oncall/`. Copy `_template.md` to create a new runbook for any alert or incident type.
- **FAQ** (`engineering/oncall/faq-common-asks.md`) covers recurring inbound requests — update it when you see the same question twice.
- **Post-mortems** live in `engineering/post-mortems/`. Write a retro for any significant incident using the template.
- **Skill:** Use the `/investigate-alert` skill when an alert fires to systematically investigate and document findings.

### 5. Post-Mortems
Post-mortems live in `engineering/post-mortems/`. These are **engineering incident retros** — root cause analyses, timelines, and action items from production incidents. They are NOT product retrospectives or project wrap-ups — those belong in the relevant project folder.

## Patterns & Learnings
