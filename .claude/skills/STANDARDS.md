# Skill Standards

Every skill in this repo must follow this self-improvement pattern.

## Required Files

```
.claude/skills/{skill-name}/
├── SKILL.md          # Main instructions with YAML frontmatter (name, description)
├── gotchas.md        # Failure patterns accumulated per run. Starts empty.
├── runs.log          # TSV log of every run (date, inputs, outputs, status, notes)
```

## Self-Improvement Loop

The core loop: **run → capture failures → next run reads failures → avoids them.**

### Pre-Run (mandatory)
1. Read `gotchas.md` — every known failure pattern. Cross-check your work against these.
2. Read `runs.log` — check what the last run covered to avoid overlap.
3. If the skill touches repo files, grep the repo for existing coverage before proposing new files.

### Post-Run (mandatory)
1. Append to `runs.log` with date, inputs, outputs, status, and notes.
2. If any failure was discovered (user correction, routing mistake, format feedback), add it to `gotchas.md`.

### The Loop Must Close
If a gotcha exists and the same mistake is repeated, the loop is broken. The pre-run read step is not optional.

## Report Format
Skills that produce reports must present **proposed actions only** — not raw data or event logs. Keep reports under a page. Each item should be approvable/rejectable in seconds.

## SKILL.md Conventions
- YAML frontmatter: `name` (slash command), `description` (triggers auto-loading)
- Keep SKILL.md under 5,000 words. Use bundled files for detailed content.
- Include explicit "Read gotchas.md and runs.log" step early in instructions.
- Include explicit "Update runs.log and gotchas.md" step at end.
