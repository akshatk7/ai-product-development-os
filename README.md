# AI Product Development OS

A plug-and-play operating system for product development teams. Fork this repo, fill in your team's context, and start shipping faster.

---

> **This is a template repo — fork it, don't contribute to it.**
>
> 1. Click **"Fork"** (top right on GitHub) to copy this repo to your own space
> 2. Rename it to match your team (e.g., `my-team-product-os`)
> 3. Follow `QUICKSTART.md` to set it up
> 4. All your changes go in **your fork** — not here
>
> Questions or feedback? [Open an issue](https://github.com/akshatk7/ai-product-development-os/issues).

---

## This Is Not a File Cabinet

Product teams usually have context scattered across Google Docs, Slack threads, Figma files, meeting notes, and people's heads. The natural instinct is to organize it — create a wiki, a shared drive, a Notion workspace. That helps, but it's still passive storage. You put things in, you take things out.

**This repo is different.** Yes, it structures your team's knowledge. But more importantly, it *connects* that knowledge, *acts on it* proactively, and *gets smarter* the more you use it. The value compounds over time.

Here's what that means concretely:

### It cross-references everything automatically

When you start writing a brief for a new project, the system doesn't wait for you to remember what happened in past projects. It automatically:
- Reads `projects/INDEX.md` to find related past projects
- Pulls their research findings (`truths.md`), decisions (`decisions.md`), and context
- Checks `data-science/INDEX.md` for relevant analyses
- Reads `product/review-taste.md` to surface what leadership has pushed on before
- Reads `strategy/beliefs.md` to ground the brief in the team's current hypotheses

No one asked it to do this. It's wired to do it by default. Every new piece of work is informed by everything that came before.

### It flags problems before you notice them

The system doesn't wait for you to ask "is anything stale?" It proactively catches:
- **Stale project statuses** — a project hasn't been updated in 14+ days but is actively discussed in Slack
- **Decision contradictions** — Project A decided X, Project B decided the opposite, and nobody noticed
- **Unresolved TBDs** — open questions older than 2 weeks that might have already been answered in a meeting or Slack thread
- **Source mismatches** — the repo says a project is in "Design" phase, but the tracker says "Coding"
- **Unchallenged beliefs** — a strategic hypothesis hasn't been re-evaluated in 30+ days

### The strategy layer is the real unlock

`strategy/beliefs.md` isn't a document you write once. It's a living hypothesis register — what the team believes about customers, product, and market, with confidence levels, evidence chains, and challenge dates.

The key: **every workflow reads and updates it.** Digest a meeting? Check if anything confirms or challenges a belief. Morning sync? Same. Customer feedback scan? Cross-reference signals. Design review? Same. Over time, this creates institutional memory of *what the team thinks and why* — something that normally lives only in people's heads and vanishes when they leave.

### It gets smarter every time you use it

Every automated skill maintains two files:
- `gotchas.md` — failure patterns from past runs ("last time I misrouted a design note to CONTEXT.md")
- `runs.log` — execution history with outcomes

Before each run, the skill reads both files. After each run, it updates them. The morning sync that made a mistake on Day 1 learns not to repeat it on Day 2. These are self-improving loops — the system literally debugs itself.

### It connects to where your team already works

The OS integrates with the tools your team uses daily — not as a one-way import, but as a two-way connection that routes information to the right place:

| Integration | What It Does | How It Connects Back |
|---|---|---|
| **Slack** | Morning scan, customer feedback scan, channel auto-discovery | Routes findings to project files, flags strategy implications |
| **Meeting tools** (Granola, Otter, etc.) | Pulls transcripts automatically | Digests route to meeting notes + project updates + decisions + strategy |
| **Project trackers** (Jira, Linear, GitHub Issues) | Bidirectional task sync | Mermaid diagrams in repo stay in sync with your tracker |
| **Google Docs** | Source of truth for briefs/PRDs | CONTEXT.md snapshots with links back to the living doc |
| **Google Sheets** | Roadmap tracking | Weekly status reconciles spreadsheet against repo state |
| **Figma** | Design files | `designs.md` links with flow descriptions |
| **Data warehouse** (Snowflake, etc.) | Customer call analysis, data queries | Speaker-aware transcript scanning with signal detection |

### Knowledge compounds, not just accumulates

The difference between a wiki and this system:
- **Decisions** track supersession chains — old decisions are struck through with pointers to what replaced them. You can traverse the evolution.
- **Beliefs** build evidence chains that grow over time. Each piece of data strengthens or weakens a hypothesis.
- **`review-taste.md`** captures leadership feedback patterns extracted after each review. After 5 reviews, you know what leadership will push on before they say it.
- **`truths.md`** accumulates system behaviors the team discovers over time — the tribal knowledge that usually lives in one engineer's head.
- **Customer intelligence** builds thematic signal libraries — not just "we got feedback," but patterns across channels, call transcripts, and time periods.

Every new piece of information gets **checked against everything else** and updates stale context. That's compounding, not accumulation.

---

## What's Inside

### Four layers work together

1. **Shared context** — `team/`, `product/`, `roadmap/` give anyone full context about the team, the product, and where things stand
2. **Strategy** — `strategy/` captures what the team believes and why, competitive landscape, and open strategic questions — integrated into every workflow
3. **Project lifecycle** — `projects/` uses templates to move work from brief → design → RFC → code → launch with standardized artifacts at each phase
4. **Automation** — Skills, agents, and rules turn structured context into action

### 11 automated skills

| Skill | What It Does |
|-------|-------------|
| **Morning sync** | Scans Slack channels + meetings from the previous day, proposes targeted repo updates, catches stale projects |
| **Meeting digestion** | Parses transcripts, extracts decisions/actions, routes to correct project files and strategy layer |
| **Weekly status** | Cross-references repo, tracker, and roadmap — flags mismatches and overdue items |
| **Brief starter** | Auto-gathers context from past projects, customer feedback, review patterns before you start writing |
| **Customer feedback scan** | Sweeps Slack for product feedback, classifies by theme and severity, cross-references to active projects |
| **Call transcript scan** | Speaker-aware analysis of customer calls with signal detection and tenure breakdowns |
| **Ship review prep** | Validates project readiness: artifact completeness, open TBDs, cross-project dependency risks |
| **Open question audit** | Finds unresolved TBDs across all projects, searches for evidence they've been answered elsewhere |
| **Task management** | Keeps Mermaid task diagrams in sync with Jira/Linear, auto-unblocks dependents when tasks complete |
| **PR review** | Reviews pull requests against your team's learned review patterns — safety, performance, scope, edge cases |
| **Alert investigation** | Investigates production alerts: maps to code areas, queries error data, traces through backend services |

### 4 specialized agents

| Agent | What It Does |
|-------|-------------|
| **Brief reviewer** | Reviews your draft against leadership review patterns from `review-taste.md` — predicts what they'll push on |
| **Decision auditor** | Cross-checks all project decisions for contradictions, stale TBDs, and cascade risks |
| **Project analyst** | Answers any question about a project by cross-referencing all its files + related projects |
| **Repo navigator** | Interactive onboarding — walks new team members through the system |

### 3 structural guardrails

- **`.cursorrules`** — AI contributor rules with a routing table for where content goes
- **Pre-commit hook** — Blocks unauthorized files at the repo root
- **CI hygiene check** — Warns on structural drift in pull requests

---

## Who Is This For

- **Product managers** who want a system that thinks with them, not just stores for them
- **Engineers** who want clear RFCs, decision logs, runbooks, and data findings per project
- **Designers** who want design specs living alongside product and eng context
- **Data scientists** who want analysis outputs that feed back into product decisions
- **New team members** who need to get up to speed in hours, not weeks

---

## How to Start

1. **Fork** this repo to your own team's GitHub space
2. **Rename** your fork to something meaningful
3. **Follow** `QUICKSTART.md` for step-by-step setup
4. **Customize** using `CUSTOMIZE.md` to adapt to your tools and workflow

All changes happen in your fork. This upstream repo is the starting template — your fork becomes **your team's knowledge base** and will diverge from here. That's expected and encouraged.

See `CLAUDE.md` for the full system overview once you're set up.

---

## Repo Structure at a Glance

```
├── strategy/                  # Beliefs, competitive intel, open questions
├── product/                   # Overview, metrics, terminology, review patterns
├── team/                      # People, rituals, channels
├── roadmap/                   # Sources, tracking, update workflow
├── projects/                  # One folder per project with full lifecycle artifacts
│   ├── _template/             # Copy to start a new project
│   └── _example/              # Fully worked example
├── meetings/                  # Weekly + leadership sync notes with routing workflows
├── engineering/               # Eng contribution guide, oncall runbooks, post-mortems, ML model reference
├── design/                    # Design conventions and contribution guide
├── data-science/              # Analyses, experiment tracking, backlog
├── customer-intelligence/     # Feedback scans, call analyses, deep dives
├── config/                    # Machine-readable configs for automation
├── reference-docs/            # Static reference materials
└── .claude/
    ├── skills/                # 11 automated workflows (each self-improving)
    ├── agents/                # 4 specialized agents
    └── rules/                 # Context-triggered conventions
```

---

**If you're using this, [star the repo](https://github.com/akshatk7/ai-product-development-os)** — it helps track adoption and prioritize improvements.

---

## Credits

Created and maintained by **[Akshat Khandelwal](https://github.com/akshatk7)**.
