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
├── team/                  # Org chart, rituals, Slack channels
├── product/               # Overview, terminology, metrics, review patterns
├── roadmap/               # Living roadmap
├── projects/              # One folder per active project (CONTEXT.md, decisions.md, truths.md, etc.)
│   ├── _template/         # Template for new projects
│   └── _example/          # Fully worked example
├── meetings/              # Notes from recurring rituals
├── engineering/            # Eng docs, oncall runbooks, post-mortems
├── design/                # Design docs
├── data-science/          # DS analyses, experiment artifacts
├── customer-intelligence/ # Customer voice, feedback scans, deep dives
├── config/                # Machine-readable configs for automated workflows
└── reference-docs/        # Static reference materials
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
- **Engineering:** engineering/, project RFCs, decisions.md, truths.md
- **Design:** design/, project designs.md
- **Data Science:** data-science/, project truths.md, experiments

## Workflows

| Workflow | Trigger | Details |
|----------|---------|---------|
| Weekly meeting prep | "write top of mind for [date]" | `meetings/weekly/CLAUDE.md` |
| Leadership forum prep | "prep leadership forum for [date]" | `meetings/leadership-forum/CLAUDE.md` |
| Roadmap updates | "update the roadmap" | `roadmap/CLAUDE.md` |
| Meeting digestion | "digest the notes from [meeting]" | `.claude/skills/digest-meeting/SKILL.md` |
| Morning sync | `/morning-sync` | `config/morning-sync.md` |
| Customer feedback scan | "scan for customer feedback" | `.claude/skills/customer-feedback-scan/SKILL.md` |

## Rules & Conventions

All prescriptive rules live in `.claude/rules/`:
- **document-conventions.md** — File boundaries, status freshness, cross-references, beliefs lifecycle, monthly hygiene
- **project-context.md** — How to use project files, cross-project knowledge retrieval
- **data-science.md** — DS analysis conventions
- **meetings.md** — Meeting notes routing conventions

## Skills & Agents

Skills follow the self-improvement pattern in `.claude/skills/STANDARDS.md` — gotchas.md, runs.log, pre-run/post-run steps.

Agents in `.claude/agents/`: project-analyst, brief-reviewer, decision-auditor, repo-navigator.

## Changelog

Every commit that changes a skill, agent, rule, template, or convention MUST include an entry in `CHANGELOG.md`.

---

> Built on the [AI Product Development OS](https://github.com/akshatk7/ai-product-development-os) by Akshat Khandelwal.
