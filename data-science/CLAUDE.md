# Data Science — Ways of Working

This file defines how the Data Science team works within this repo. It is a living document — update it proactively whenever meaningful learnings, patterns, or preferences emerge from DS work sessions.

---

## Team

- **[DS Name]** — [Focus area]
- **Manager:** [DS Manager Name]

## What DS Contributes

DS work includes experiment design, monitoring, and readouts; ad-hoc analyses that inform product and engineering decisions; and cross-cutting investigations.

### Folder Structure

```
data-science/
├── CLAUDE.md              # THIS FILE
├── _template/             # Analysis template
├── {time-period}/         # e.g., 2026-H1
│   └── {project-name}/   # e.g., churn-analysis
│       ├── *.md           # Documentation, findings
│       └── *.py           # Analysis scripts (optional)
```

- Organize by **half-year** (e.g., `2026-H1`, `2026-H2`) then by **project name**.
- Only push **core files** the team needs: documentation, key scripts, and findings.
- Charts, CSVs, and scratch scripts stay local — they can be regenerated from scripts.

### What Belongs Here vs In Project Folders

- **Experiment readouts and analysis scripts** → `data-science/{period}/{project}/`
- **Data findings that inform eng decisions** → also cross-post to the relevant `projects/{area}/{project}/truths.md`
- **Meeting-ready summaries** → can live here or in `meetings/` depending on context

## Tools

- [Your data warehouse] for queries
- Python (Pandas, etc.) for analysis
- [Your dashboard tool] for dashboards

## Conventions

[Add domain-specific conventions here. Examples:]
- Define key metrics and how they're computed so analyses are consistent
- Document known data quality issues and workarounds
- Note any table/column gotchas that trip people up

## Patterns & Learnings
