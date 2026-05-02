# Product Development Operating System

This repo is the shared operating system for your product team. It contains all the context an agent (or a new team member) needs to understand the team, the product, the projects, and how we work.

**This repo is NOT source code.** It is a documentation and context repo.

**To get started:** See `QUICKSTART.md` for a step-by-step setup guide.
**To customize:** See `CUSTOMIZE.md` for adapting this to your team's tools and workflow.

---

## Quick Orientation

- **What is this product?** See `product/overview.md`.
- **Who's on the team?** See `team/people.md`.
- **What are we building?** See `roadmap/CLAUDE.md`.
- **Active projects?** See `projects/INDEX.md`.
- **Strategy & beliefs?** See `strategy/beliefs.md` — the team's living hypotheses.

## Repo Structure

```
├── strategy/              # Beliefs, competitive context, open questions
├── team/                  # Org chart, rituals, channels
├── product/               # Overview, terminology, metrics, review patterns
├── roadmap/               # Living roadmap
├── projects/              # One folder per active project (CONTEXT.md, decisions.md, truths.md, etc.)
│   ├── _template/         # Template for new projects
│   └── _example/          # Fully worked example
├── meetings/              # Notes from recurring rituals (weekly is core)
├── design/                # Design docs
├── customer-intelligence/ # Voice of customer (feedback, research, deep dives)
├── config/                # Machine-readable configs (repo-hygiene.json, morning-sync.md)
├── reference-docs/        # Static reference materials
└── add-ons/               # Optional patterns (engineering, data-science, leadership-forum, etc.) — see add-ons/README.md
```

## Project Files

Each project folder can contain (created organically, not pre-created):

| File | Purpose | Who |
|------|---------|-----|
| `README.md` | Overview, phase, team | PM seeds, anyone updates |
| `CONTEXT.md` | Narrative briefing (not a journal) | PM |
| `updates.md` | Chronological log (newest first) | Anyone |
| `decisions.md` | Key decisions with supersession tracking | Anyone |
| `truths.md` | System/data findings | Eng, DS |
| `designs.md` | UI/UX flows, Figma links | Design |
| `rfc-frontend.md` / `rfc-backend.md` | Technical design | Eng |
| `product-brief.md` | Requirements and goals | PM |
| `experiments.md` | A/B test tracking | PM, DS |
| `scratchpad.md` | Exploratory work | Anyone |

**Lifecycle:** Exploratory → Product Brief → Designs → Feasibility → RFCs → Coding → Live

## Ownership

- **PM:** CLAUDE.md, product/, roadmap/, team/, strategy/, customer-intelligence/
- **Engineering:** project RFCs, decisions.md, truths.md (plus `add-ons/engineering/` if enabled)
- **Design:** design/, project designs.md
- **Data Science:** project truths.md, experiments (plus `add-ons/data-science/` if enabled)

## Workflows

| Workflow | Trigger | Details |
|----------|---------|---------|
| Weekly meeting prep | "write top of mind for [date]" | `meetings/weekly/CLAUDE.md` |
| Roadmap updates | "update the roadmap" | `roadmap/CLAUDE.md` |
| Meeting digestion | "digest the notes from [meeting]" | `.claude/skills/digest-meeting/SKILL.md` |
| Morning sync | `/morning-sync` | `config/morning-sync.md` |
| Brief starter | "start a brief for X" | `.claude/skills/brief-starter/SKILL.md` |
| Ship review prep | "prep ship review for X" | `.claude/skills/ship-review-prep/SKILL.md` |
| Resolve open questions | "audit open questions" | `.claude/skills/resolve-open-questions/SKILL.md` |
| Debrief Slack thread | `/debrief-thread <url>` | `.claude/commands/debrief-thread.md` |
| Prep for a meeting | `/prep-meeting <topic>` | `.claude/commands/prep-meeting.md` |
| Generate launch email | `/generate-launch-email` | `.claude/commands/generate-launch-email.md` |
| Review launch email | `/review-launch-email` | `.claude/commands/review-launch-email.md` |

Additional skills (analytics, engineering, customer-feedback scans) live in `.claude/skills/` and `add-ons/skills/`. See `add-ons/README.md` for opt-in patterns.

## Rules & Conventions

All prescriptive rules live in `.claude/rules/`:
- **document-conventions.md** — File boundaries, status freshness, cross-references, beliefs lifecycle, data output policy, monthly hygiene
- **project-context.md** — How to use project files, cross-project knowledge retrieval
- **data-science.md** — DS analysis conventions
- **meetings.md** — Meeting notes routing conventions, meeting → file routing table
- **claude-md-ownership.md** — CLAUDE.md is max 150 lines, orientation only. Content routing table.

## Skills & Agents

Skills follow the self-improvement pattern in `.claude/skills/STANDARDS.md` — gotchas.md, runs.log, pre-run/post-run steps, portability rules, deprecation procedure.

Agents in `.claude/agents/`: project-analyst, brief-reviewer, decision-auditor, repo-navigator, monthly-audit (scheduled).

## Hygiene enforcement

Four layers: pre-commit hook (blocks structural issues), CI workflows (repo-hygiene + link-check), morning-sync daily hygiene scorecard, monthly audit agent. Rules in `config/repo-hygiene.json` — single source of truth. See `.claude/rules/document-conventions.md` § Automated Hygiene Enforcement.

## Changelog

Every commit that changes a skill, agent, rule, template, or convention MUST include an entry in `CHANGELOG.md`.

---

> Built on the [AI Product Development OS](https://github.com/akshatk7/ai-product-development-os) by Akshat Khandelwal.
