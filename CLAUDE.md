# Product Development Operating System

This repo is the shared operating system for your product team. It contains all the context an agent (or a new team member) needs to understand the team, the product, the projects, and how we work.

**This repo is NOT source code.** It is a documentation and context repo. Do not create package.json, tsconfig, or build artifacts at the root level. The only exception is project-specific prototypes that live within their project folder (e.g., `projects/.../prototype/`).

**To get started:** See `QUICKSTART.md` for a step-by-step setup guide.
**To customize:** See `CUSTOMIZE.md` for adapting this to your team's tools and workflow.

---

## Quick Orientation

**What is this product?** See `product/overview.md`.
**Who's on the team?** See `team/people.md`.
**What are we building?** See `roadmap/CLAUDE.md` for the living roadmap.
**What are the active projects?** See `projects/` — each project has its own folder with full context.
**What do we believe?** See `strategy/beliefs.md` — the team's living hypotheses about customers, product, and market.

---

## Repo Structure

```
├── CLAUDE.md                  # THIS FILE — start here
├── README.md                  # What this repo is and how to use it
├── QUICKSTART.md              # Step-by-step setup guide
├── CUSTOMIZE.md               # How to adapt to your team's tools
├── .cursorrules               # AI contributor rules for Cursor IDE users
├── strategy/
│   ├── CLAUDE.md              # How the thinking layer works
│   ├── beliefs.md             # Hypotheses about customers, product, market — with evidence
│   ├── competitive.md         # What competitors and analogous products are doing
│   └── open-questions.md      # Strategic uncertainties that frame the work
├── team/
│   ├── people.md              # Org chart and roles
│   ├── rituals.md             # All recurring meetings
│   └── channels.md            # Slack channels (auto-generated from config/morning-sync.md)
├── product/
│   ├── CLAUDE.md              # PM workflows: briefs, PRDs, updates
│   ├── overview.md            # What the product is, how it works, strategy
│   ├── terminology.md         # Glossary of terms
│   ├── metrics.md             # Metric definitions (no current values — see dashboards)
│   └── review-taste.md        # Leadership review patterns and recurring feedback
├── roadmap/
│   └── CLAUDE.md              # Roadmap sources, update workflow
├── projects/                  # One folder per active project
│   ├── INDEX.md               # Cross-project index (systems, domains, relationships, artifacts)
│   ├── _template/             # Template for new projects (README.md, CONTEXT.md, + all artifact templates)
│   └── _example/              # Fully worked example project
│   # Each project contains artifacts as needed (created organically):
│   # README.md, CONTEXT.md, updates.md, product-brief.md, designs.md,
│   # decisions.md, truths.md, rfc-frontend.md, rfc-backend.md,
│   # experiments.md, launch-checklist.md, analytics-spec.md, scratchpad.md
├── meetings/                  # Notes from recurring rituals
│   ├── weekly/                # Team sync (CLAUDE.md + dated notes)
│   └── leadership-forum/      # Strategic forum (CLAUDE.md + dated sessions)
├── engineering/               # Team-wide eng docs (NOT project-specific)
│   ├── CLAUDE.md              # Eng contribution guide
│   ├── oncall/                # Oncall runbooks and debugging guides
│   │   ├── README.md          # Oncall duties, debugging tools, runbook index
│   │   ├── _template.md       # Runbook template
│   │   └── faq-common-asks.md # Recurring inbound requests quick-reference
│   └── post-mortems/          # Incident retros (root cause, timeline, action items)
│       └── _template.md       # Post-mortem template
├── design/                    # Team-wide design docs (NOT project-specific)
│   └── CLAUDE.md              # Design contribution guide
├── data-science/              # DS analyses and reference docs
│   ├── CLAUDE.md              # DS contribution guide
│   ├── INDEX.md               # Maps analyses to projects they inform
│   ├── backlog.md             # Open DS questions and analysis requests
│   └── _template/             # Analysis template
├── customer-intelligence/     # All customer voice, feedback, and behavioral analysis
│   ├── README.md              # Sources, skill triggers, channel list
│   ├── slack-scans/           # Dated Slack feedback scan outputs
│   ├── call-scans/            # Dated call transcript analysis outputs
│   └── deep-dives/            # Ad-hoc customer-specific analyses
├── config/                    # Machine-readable configs for automated workflows
│   └── morning-sync.md        # Source registry for /morning-sync skill
├── reference-docs/            # Static reference materials (PDFs, foundational research)
└── .claude/
    ├── skills/                # Automation skills (each with SKILL.md + gotchas.md + runs.log)
    │   ├── STANDARDS.md       # Self-improvement pattern all skills must follow
    │   ├── morning-sync/      # Daily knowledge base sync
    │   ├── digest-meeting/    # Meeting transcript → repo file routing
    │   ├── weekly-status/     # Cross-referenced status snapshot from all sources
    │   ├── customer-feedback-scan/  # Slack + call transcript feedback analysis
    │   ├── brief-starter/     # Auto-gather context before writing a brief
    │   ├── ship-review-prep/  # Validate project readiness for ship review
    │   ├── resolve-open-questions/  # Audit TBDs across all projects
    │   ├── eng-task-management/     # Mermaid diagram + ticket sync
    │   ├── call-transcript-scan/    # Speaker-aware call transcript analysis
    │   ├── pr-review/              # Review PRs against team's learned review patterns
    │   └── investigate-alert/      # Investigate production alerts systematically
    ├── agents/                # Specialized agents
    │   ├── project-analyst.md # Answer any question about a project
    │   ├── brief-reviewer.md  # Review briefs against leadership patterns
    │   ├── decision-auditor.md # Cross-check decisions for contradictions
    │   └── repo-navigator.md  # Interactive onboarding guide
    └── rules/                 # Context-triggered rules
        ├── project-context.md # Auto-loads when working in projects/
        ├── document-conventions.md  # File boundaries, status freshness, cross-refs, hygiene
        ├── data-science.md          # DS analysis conventions
        └── meetings.md              # Meeting notes routing conventions
```

---

## Project Structure

Each project folder under `projects/` can contain:

| File | Purpose | Who Writes It |
|------|---------|---------------|
| `README.md` | Project overview, current phase, team, repos | PM seeds, anyone updates |
| `CONTEXT.md` | Narrative project context: what we're building, why, current state, scope. **Not a journal** — no dated sections. Reads like a briefing doc. | PM |
| `updates.md` | Chronological log of project updates (newest first). Meeting digests, status changes, action items. Morning sync and meeting digestion append here. | Anyone |
| `product-brief.md` | Product requirements and goals | PM |
| `designs.md` | UI/UX flows in text, with Figma links | Design |
| `rfc-frontend.md` | Frontend design doc | Frontend eng |
| `rfc-backend.md` | Backend design doc | Backend eng |
| `decisions.md` | Key decisions with supersession tracking | Anyone |
| `truths.md` | Research findings about data, systems, architecture | Eng, DS |
| `experiments.md` | A/B test tracking | PM, DS |
| `launch-checklist.md` | Ship review and launch gates | PM, Eng |
| `analytics-spec.md` | Instrumentation spec | PM, Eng |
| `scratchpad.md` | Exploratory work, mockups, scratch artifacts | Anyone |

Not every project needs every file. Early-stage projects may only have a README and CONTEXT.md. **Files are created organically** as content arises — do not pre-create empty files.

### Project Organization

Projects can be organized flat or by category depending on team size:

```
projects/
├── _template/
├── _example/
├── your-project/           # Flat — works for small teams
├── feature-area/           # Categorical — works for larger teams
│   ├── project-a/
│   └── project-b/
├── cross-functional/       # Cross-team partnerships
└── future/                 # Big initiatives not yet staffed
```

## Project Lifecycle Phases

```
Exploratory → Product Brief → Designs → Feasibility → RFCs → Coding → Live
```

The current phase for each project is tracked in its README.md.

---

## How We Work

### Product Development Workflow

1. **Context gathering:** Pull from Google Docs, meeting notes, Slack, Figma
2. **Brief:** Written in Google Docs. Goes through brief review.
3. **Design:** Figma designs based on brief. Goes through design review.
4. **PRD:** Describes behavior and logic, not UI details (those are in Figma). Cross-checked against CONTEXT.md before completion.
5. **Eng RFC:** Engineering design doc. Can be drafted from full project context.
6. **Development:** Eng implements from RFC + Figma + codebase.
7. **E2E Testing → Ship Review → Launch**

### Recurring Workflows

| Workflow | Trigger | Details |
|----------|---------|---------|
| Weekly meeting prep | "write top of mind for [date]" | See `meetings/weekly/CLAUDE.md` |
| Leadership forum prep | "prep leadership forum for [date]" | See `meetings/leadership-forum/CLAUDE.md` |
| Roadmap updates | "update the roadmap" | See `roadmap/CLAUDE.md` |
| Meeting digestion | "digest the notes from [meeting name]" | See `.claude/skills/digest-meeting/SKILL.md` |
| Morning sync | `/morning-sync` | See `config/morning-sync.md` |
| Weekly status | "weekly status" or "pull status" | See `.claude/skills/weekly-status/SKILL.md` |
| Customer feedback scan | "scan for customer feedback" | See `.claude/skills/customer-feedback-scan/SKILL.md` |
| PR review | "review PR [url]" | See `.claude/skills/pr-review/SKILL.md` |
| Alert investigation | "investigate alert [link]" | See `.claude/skills/investigate-alert/SKILL.md` |

### Strategy Layer — Proactive Integration

The `strategy/` folder is the thinking layer of the repo. It captures beliefs, competitive context, and strategic questions. **It must be read and updated in every workflow, not just when asked.**

**Every session:** Read `strategy/beliefs.md` at session start alongside project context. When working on any project, check whether new information confirms, challenges, or invalidates existing beliefs.

**Every meeting digestion:** After extracting decisions and updates, ask: "Does anything from this meeting update a belief, add competitive context, or resolve an open question?" If yes, update `strategy/` files as part of the same commit.

**Every morning sync:** Include a "Strategic Signals" section in the report (1-3 lines max) noting which beliefs were strengthened or challenged by the day's data.

**Every brief/PRD/RFC:** Before writing, read relevant beliefs and open questions. The strategy layer should inform how features are framed, not just what gets built.

**Proactive synthesis:** When you notice a pattern across projects — experiment results pointing in the same direction, multiple meetings surfacing the same concern, data from different sources converging — surface it by updating beliefs.md or adding to open-questions.md. Don't wait to be asked.

### Skill Standards

All skills follow the self-improvement pattern defined in `.claude/skills/STANDARDS.md` — gotchas.md, runs.log, mandatory pre-run/post-run steps. Read that file before creating or modifying any skill.

### Meeting → File Mapping

When digesting a meeting, use this table to route information to the right files:

| Meeting | Primary Output | Secondary Updates |
|---------|---------------|-------------------|
| Team weekly | `meetings/weekly/[date].md` | Relevant project updates.md, decisions.md, **strategy/beliefs.md** |
| Leadership forum | `meetings/leadership-forum/[date]/` | `product/overview.md`, `roadmap/`, relevant project updates.md, **strategy/beliefs.md** |
| Eng standup | Relevant project updates.md | — |
| Design review | Relevant project designs.md, updates.md | `product/review-taste.md` (extract recurring feedback patterns), **strategy/beliefs.md** |
| DS sync | Relevant project truths.md | `data-science/` if analysis discussed, **strategy/beliefs.md** if findings challenge/confirm hypotheses |
| External team sync | Relevant project updates.md | **strategy/competitive.md** if external team shares relevant patterns |

For meetings not in this table: surface content and let the PM decide where it goes.

---

## Document Conventions

### Decision Tracking
When a newer decision supersedes an older one in `decisions.md`, strike through only the title using `~~title~~`, update the body to reflect the current state, and add: "**Superseded by:** [date + title of newer decision]". See `projects/_example/decisions.md` for a worked example.

### Research Findings (truths.md)
`truths.md` accumulates research findings incrementally as they are discovered. Don't batch findings at the end — add them as you learn them.

### Google Docs as Source of Truth
Google Docs are the living source of truth. Markdown files in this repo are point-in-time snapshots. Always link the source Google Doc at the top of any CONTEXT.md or brief, and note that the Google Doc may contain comments and updates not reflected in the markdown. When prompted to refresh a file, re-read the Google Doc and update the markdown.

### Channels Registry
`team/channels.md` is auto-generated from `config/morning-sync.md`. Morning-sync is the single source of truth for Slack channels, IDs, and project mappings. When channels change in morning-sync, regenerate channels.md to match. Do not manually edit channels.md independently.

### Freshness Timestamps
Key reference files should carry a `> Last reviewed: YYYY-MM-DD` line at the top. This helps readers assess staleness without checking git blame. Recommended for: `product/overview.md`, `product/metrics.md`, `product/terminology.md`, `team/people.md`, `team/rituals.md`, `strategy/beliefs.md`, and any roadmap/goaling files.

### File Boundary Rules
Content should be routed to the correct project file. See `.claude/rules/document-conventions.md` for the full routing table. In short:
- Decisions go in `decisions.md`
- Design flows go in `designs.md`
- System findings go in `truths.md`
- Chronological updates go in `updates.md`
- **Strategic hypotheses go in `strategy/beliefs.md`** — not in project CONTEXT.md
- **Competitive intel goes in `strategy/competitive.md`** — not scattered across project files
- **Unresolved strategic questions go in `strategy/open-questions.md`** — not buried in meeting notes
- `CONTEXT.md` is for narrative context only

### Knowledge Propagation
When digesting new information (meeting notes, briefs, Slack, docs), check it against existing files (`product/overview.md`, `roadmap/`, relevant project CONTEXT.md, **`strategy/beliefs.md`**) and update any files that are now stale. Don't just accumulate — self-correct.

### Conflict Resolution
Projects have multiple source-of-truth files that may diverge as decisions evolve. When you find conflicting information across files:
1. Surface both versions with the file and section where each appears.
2. Ask the PM which is correct before proceeding.
3. After resolution, update all conflicting files so the conflict doesn't recur.

`decisions.md` tracks superseded decisions explicitly and is usually most current, but always confirm.

### Task Diagrams (eng-roadmap.md)
Projects with active eng work use Mermaid flowcharts in `eng-roadmap.md`. Four node states:

| State | Style |
|-------|-------|
| Not started (unblocked) | No style directive |
| Blocked | `fill:#ffe0e0,stroke:#ffaaaa,color:#000000` |
| In Progress | `fill:#fffde0,stroke:#ffeeaa,color:#000000` |
| Complete | `fill:#e0f5e0,stroke:#aaddaa,color:#000000` |

When a task completes, auto-unblock dependents: check each downstream task's prerequisites, and if all are now complete, remove its blocked styling.

---

## Ownership & Contributing

### PM
- Maintains: CLAUDE.md, product/, roadmap/, team/, strategy/
- Seeds new project folders with CONTEXT.md
- Writes briefs and PRDs
- Runs weekly meeting prep and leadership forum prep
- Owns customer-intelligence/ channel registry and synthesis

### Engineering
- **Within projects:** Contribute rfc-frontend.md, rfc-backend.md, decisions.md, truths.md
- **Team-wide:** Add rituals and channels to team/, document on-call and incidents
- **Oncall runbooks:** Add runbooks to `engineering/oncall/` using the template. Every resolved alert should produce a runbook so the next person doesn't start from scratch.
- **Post-mortems:** Write retros in `engineering/post-mortems/` using the template
- See `engineering/CLAUDE.md` for contribution guide

### Design
- **Within projects:** Contribute designs.md, Figma links, design notes, edge cases
- **Team-wide:** Document Figma structure conventions
- See `design/CLAUDE.md` for contribution guide

### Data Science
- Push analysis artifacts to `data-science/`
- Contribute to experiment readouts in meeting notes
- Contribute to project-level truths.md when data findings are relevant
- See `data-science/CLAUDE.md` for contribution guide

---

## Changelog
Every commit that adds, removes, or changes a skill, agent, rule, template, or convention MUST include an entry in `CHANGELOG.md`. Append under the current version's section. If the change is significant enough to warrant a new version, bump the version number and create a new section. Tag major versions with `git tag vX.Y`.

---

## Proactive Optimization
Periodically (every few sessions, or when something feels off), flag if:
- A CLAUDE.md (project or root) is bloated, stale, or has contradictions
- A skill's gotchas are growing but the same mistakes keep happening (loop is broken)
- A folder structure has drifted from what the CLAUDE.md documents
- A skill could be improved based on patterns from recent runs
- A belief in `strategy/beliefs.md` hasn't been challenged in 30+ days
- An open question in `strategy/open-questions.md` has been sitting without progress
Don't wait to be asked — surface it briefly: "I noticed X is stale/broken, want me to fix it?"

---

> Built on the [AI Product Development OS](https://github.com/akshatk7/ai-product-development-os) by Akshat Khandelwal.
