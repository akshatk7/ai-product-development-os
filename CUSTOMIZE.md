# Customization Guide

This repo is designed to be adapted. Here's how to make it fit your team's tools and workflow.

---

## Project Tracking: Jira vs Linear vs GitHub Issues

The `eng-task-management` skill (`.claude/skills/eng-task-management/SKILL.md`) syncs Mermaid task diagrams with your project tracker. To configure:

1. Set your instance URL (e.g., `yourcompany.atlassian.net` for Jira, `linear.app` for Linear)
2. Set your transition IDs (e.g., "Done" = transition ID `31` in Jira)
3. Update the JQL or query syntax for finding tickets under an epic

If you use GitHub Issues instead, replace the Jira integration section with label-based status tracking.

## Design Tools: Figma vs Other

Figma links appear in two places:
- `projects/{project}/designs.md` — per-project design specs
- `design/CLAUDE.md` — team-wide design contribution guide

If you use a different tool (Sketch, Adobe XD, Framer), replace Figma references in both locations. The `designs.md` template is tool-agnostic — it focuses on text-based flow descriptions alongside the design tool link.

## Communication: Slack vs Teams vs Discord

Slack channel IDs are used in:
- `team/channels.md` — channel registry
- `config/morning-sync.md` — automation routing
- `customer-intelligence/README.md` — feedback channel registry

If you use Microsoft Teams or Discord, replace channel IDs with your platform's equivalents. The morning sync and customer feedback scan skills need a way to programmatically read messages from channels — ensure your communication tool's MCP or API supports this.

## Meeting Notes: Granola vs Otter vs Other

The `digest-meeting` skill (`.claude/skills/digest-meeting/SKILL.md`) pulls transcripts from your meeting tool. By default it references Granola. To use a different tool:

1. Update the "Get the Transcript" step in the skill to use your tool's API
2. If your tool has an MCP server, configure it and update the skill's tool references

The meeting-to-file routing and content extraction logic is tool-agnostic — only the transcript source changes.

## Strategy Layer Customization

The `strategy/` folder ships with four thematic sections in `beliefs.md`: Customer Behavior, Product & Growth, Experiment Insights, Market & Competitive. Rename these to match your domain:

- B2B SaaS → "Buyer Behavior", "Adoption & Expansion", "Churn Drivers", "Market Dynamics"
- Consumer app → "User Psychology", "Engagement Loops", "Monetization", "Competitive"
- Platform → "Developer Experience", "Platform Flywheel", "Ecosystem", "Standards & Regulation"

The schema (confidence, evidence for/against, implications, last challenged) stays the same regardless of domain.

## Customer Intelligence Customization

The `customer-intelligence/` folder uses generic signal types. Customize:

1. **Channels:** Update the channel registry in `customer-intelligence/README.md` with your actual feedback sources
2. **Signal types:** The feedback scan skill classifies signals as bug/feature request/confusion/churn risk/positive. Add or rename categories for your domain.
3. **Add a diagnosis workflow:** If your team does structured customer investigations, create a `diagnosis-workflow.md` in `customer-intelligence/` with your analysis dimensions, data sources, and output format.

## Adding Custom Meeting Types

To add a new recurring meeting:

1. Create a folder under `meetings/` (e.g., `meetings/sprint-retro/`)
2. Add a `CLAUDE.md` with the meeting's automation workflow (see `meetings/weekly/CLAUDE.md` for the pattern)
3. Add the meeting to `team/rituals.md`
4. Add a row to the "Meeting → File Mapping" table in the root `CLAUDE.md`

## Adding Post-Mortems / Retros

Post-mortems live in `engineering/post-mortems/`. A template is provided at `engineering/post-mortems/_template.md`. To add a retro:

1. Copy the template: `cp engineering/post-mortems/_template.md engineering/post-mortems/YYYY-MM-DD-incident-name.md`
2. Fill in the sections
3. Add "retro" as a meeting type in `team/rituals.md` if you want recurring retrospectives

## Adding Custom Skills

Skills live in `.claude/skills/`. To create one:

1. Create a folder: `.claude/skills/your-skill-name/`
2. Add a `SKILL.md` with YAML frontmatter (`name`, `description`, `alwaysApply`) and the skill's instructions
3. Add a `gotchas.md` (read before every run, updated after issues)
4. Reference any config files the skill needs (e.g., `config/morning-sync.md`)
5. Follow the standards in `.claude/skills/STANDARDS.md`

## Removing Optional Sections

Not every team needs every section. Here's what's safe to remove:

| Section | Safe to Remove? | Impact |
|---------|----------------|--------|
| `strategy/` | Not recommended | High-value thinking layer; remove only if team has a separate strategy tool |
| `customer-intelligence/` | Yes | No other files depend on it; remove if customer feedback is tracked elsewhere |
| `data-science/` | Yes | No other files depend on it |
| `meetings/leadership-forum/` | Yes | Remove the corresponding row from the meeting table in CLAUDE.md |
| `reference-docs/` | Yes | No dependencies |
| `engineering/post-mortems/` | Yes | Remove if incident retros live elsewhere |
| `experiments.md` in project template | Yes | Remove from `_template/` if your team doesn't run A/B tests |
| `analytics-spec.md` in project template | Yes | Remove if instrumentation is handled elsewhere |
| `design/` | Not recommended | Projects reference designs.md; keep the contribution guide even if minimal |
| `engineering/` | Not recommended | RFCs and codebase guide are high-value for agents |

## Optional Add-Ons

These aren't included in the base repo but are patterns you can add if your team needs them:

### Goal / BHAG Tracking
If your team has a single North Star metric or BHAG, create `roadmap/goals/README.md` with: target, current state, base-case projection, gap, and a projections table. Add an `opportunity-tree.md` if you want to break the addressable universe into actionable segments.

### ML Model Reference
If your product is ML-powered, create `engineering/ml-models.md` with a section per model: what it does, how it works, key parameters, limitations, doc link, status, and DRI.

### Customer Diagnosis Workflow
If your team does structured entity-level investigations (e.g., "why is this customer churning?"), create `customer-intelligence/diagnosis-workflow.md` with: trigger phrase, data sources, lookup steps, analysis dimensions, output format, and gotchas.

### OKR Tracking
If your team uses OKRs, create `roadmap/okrs/` with a file per quarter (e.g., `2026-Q2.md`). Each file lists objectives, key results with current vs. target, and confidence levels.

---

## Repo Guardrails

The repo ships with three layers of guardrails to keep the structure clean:

1. **`.cursorrules`** — Rules for AI contributors using Cursor IDE. Edit the routing table if you add/remove top-level folders.
2. **`.githooks/pre-commit`** — Blocks new files/dirs at the repo root outside the approved list. Update the `ALLOWED_ROOT_FILES` and `ALLOWED_ROOT_DIRS` variables. Enable with: `git config core.hooksPath .githooks`
3. **`.github/workflows/repo-hygiene.yml`** — CI check that warns on unauthorized root files. Update the allowed lists to match your structure.
