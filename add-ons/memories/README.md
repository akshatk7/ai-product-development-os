# Persistent Learnings (Memories)

A place for cross-cutting facts, gotchas, and resolved-incident patterns that should apply to any future agent or session in this repo. Different from skill-level `gotchas.md` (which lives next to a single skill) — these are repo-wide learnings that any workflow might benefit from.

To enable this add-on, move it to `.claude/memories/`:

```bash
mv add-ons/memories .claude/memories
```

Skills and agents that benefit from prior learnings should read `.claude/memories/INDEX.md` (or specific subfolder INDEX files) before starting work, and append new learnings after a noteworthy resolution.

---

## What goes here

**Good fits:**
- Repeated debugging patterns across different incidents (e.g., "this alert always means X is misconfigured upstream")
- System behaviors that catch people off guard (e.g., "field A is unreliable for purpose B; use field C instead")
- Standing rules learned the hard way (e.g., "never mutate production data directly; always go through Z")
- Resolved on-call/customer-escalation patterns with reproducible signatures

**Not good fits:**
- Project-specific findings → those go in the project's `truths.md`
- Decisions and rationale → those go in `decisions.md` (project-level) or `strategy/beliefs.md` (cross-cutting)
- Skill failure patterns → those go in the skill's own `gotchas.md`

---

## Suggested structure

Start flat with just an `INDEX.md` and individual entries. Subfolder organization emerges as the count grows.

```
.claude/memories/
├── INDEX.md                       # Top-level index, links to entries
├── _template.md                   # Template for new entries
├── feedback_<topic>.md            # Standing rules / corrections
├── reference_<topic>.md           # Pointers to authoritative external resources
└── <subfolder>/                   # Group by domain when count > ~15 entries
    ├── INDEX.md                   # Subfolder index
    └── <pattern-name>.md          # Individual entries
```

The `feedback_` and `reference_` prefixes are conventions for entry types — feel free to adapt to your team's vocabulary.

---

## Adding a new entry

1. Copy `_template.md` to a new file with a short, descriptive kebab-case name.
2. Fill in the symptom, investigation, resolution, and pointer sections.
3. Add a one-line bullet to `INDEX.md` (or the relevant subfolder INDEX).
4. If the learning is a one-line standing rule, you can skip the per-file structure and add it directly as a bullet under a section in `INDEX.md`.

---

## Usage in skills

Skills that should consult memories before running can add a step in their `SKILL.md`:

```markdown
## Pre-Run

1. Read `.claude/memories/INDEX.md` for any cross-cutting learnings that might apply.
2. If this skill has a domain (e.g., on-call investigations), also read `.claude/memories/<domain>/INDEX.md`.
```

After a skill resolves a noteworthy incident or discovers a reproducible pattern, it should append a new entry and update INDEX.
