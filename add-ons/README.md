# Add-Ons

Add-ons are opt-in patterns that don't apply to every team. The default OS ships with a minimal core; if your team's shape needs one of these, enable it.

## How to enable an add-on

Move the directory (or skill) from `add-ons/` to your repo root:

```bash
# Example: enable the engineering add-on
mv add-ons/engineering .

# Example: enable the customer-feedback-scan skill
mv add-ons/skills/customer-feedback-scan .claude/skills/

# Example: enable the memories add-on
mv add-ons/memories .claude/memories

# Example: enable the leadership-forum prep workflow
mv add-ons/leadership-forum meetings/

# Example: enable the data-science folder
mv add-ons/data-science .
```

That's it. The skills and agents that reference these directories check for their existence at runtime — they activate automatically once the directory is at the root.

To disable later, move the directory back into `add-ons/`.

## What's available

| Add-on | When to enable | What it includes |
|---|---|---|
| `engineering/` | Your team has engineers, on-call, or runs incident retros | `oncall/` runbooks + FAQ template, `post-mortems/` template, `engineering/CLAUDE.md` ways-of-working |
| `data-science/` | Your team has a DS function or runs structured analyses | DS folder structure (organized by half-year), analysis template, `INDEX.md`, `backlog.md`, DS ways-of-working |
| `leadership-forum/` | Your team has a recurring leadership/exec sync ritual | Bi-weekly leadership-forum prep workflow (lives under `meetings/leadership-forum/` once enabled) |
| `memories/` | You want repo-wide persistent learnings (cross-cutting gotchas, resolved-incident patterns) | Folder + `INDEX.md` + `_template.md`. Move to `.claude/memories/`. Skills can read this folder before runs to avoid known pitfalls. |
| `skills/customer-feedback-scan/` | You collect product feedback in a chat tool (Slack, Teams, Discord) | Skill that sweeps registered channels, classifies signals, writes a dated scan |
| `skills/call-transcript-scan/` | You record customer/sales calls and want speaker-aware analysis | Skill that pulls transcripts, runs thematic + speaker analysis, classifies signals |
| `skills/investigate-alert/` | You have an on-call rotation and want a structured alert-investigation workflow | Skill that maps alerts to runbooks, queries logs, traces through services. Pairs with `engineering/` add-on. |
| `skills/figma-screenshots/` | You use Figma and want to export screen PNGs into a project's `screenshots/` folder | Skill that creates/uses a `figma-summary.md` with a node-ID reference table, then downloads PNGs in batched calls |

## Adding your own add-ons

If you build a pattern that's useful to your team but won't apply universally, you can keep it under `add-ons/` in your fork. The convention: each add-on is self-contained, has its own README explaining when to enable, and references no paths outside its own folder unless documented.

## Note on skills inside add-ons

Skills under `add-ons/skills/` are not loaded by default. To activate one, move it to `.claude/skills/`. The skill's `SKILL.md`, `gotchas.md`, and `runs.log` move together.
