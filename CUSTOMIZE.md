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

The `customer-intelligence/` folder is intentionally bare-bones — it's a home for voice-of-customer artifacts in whatever shape fits your product. To populate it:

1. **Pick your sources.** Customer signal can come from support tickets, NPS surveys, sales calls, in-product feedback, social mentions, churn interviews, app store reviews, Slack escalations, user research — whatever your team uses.
2. **Add a diagnosis workflow** (optional). If your team does structured investigations of individual customers/segments/issues, create `customer-intelligence/diagnosis-workflow.md` with your data sources, dimensions, and output format.
3. **Enable scan skills** (optional). If you collect feedback in chat tools or record customer calls, two opt-in skills are available:
   - `add-ons/skills/customer-feedback-scan` — sweep registered chat channels for product signal
   - `add-ons/skills/call-transcript-scan` — speaker-aware analysis of recorded calls

   Move either skill into `.claude/skills/` to enable. Each is tool-pluggable (works with Slack, Teams, Discord; Granola, Otter, Chorus; etc.) — see the skill's SKILL.md for configuration.

## Adding Custom Meeting Types

To add a new recurring meeting:

1. Create a folder under `meetings/` (e.g., `meetings/sprint-retro/`)
2. Add a `CLAUDE.md` with the meeting's automation workflow (see `meetings/weekly/CLAUDE.md` for the pattern)
3. Add the meeting to `team/rituals.md`
4. Add a row to the "Meeting → File Mapping" table in the root `CLAUDE.md`

## Adding Post-Mortems / Retros

Post-mortems live under `engineering/post-mortems/`, which ships as part of the engineering add-on (under `add-ons/engineering/`). To enable:

1. Move the engineering folder up: `mv add-ons/engineering .`
2. Copy the post-mortem template for a specific incident: `cp engineering/post-mortems/_template.md engineering/post-mortems/YYYY-MM-DD-incident-name.md`
3. Fill in the sections
4. Add "retro" as a meeting type in `team/rituals.md` if you want recurring retrospectives

If your team doesn't have engineers but does want incident retros (e.g., for a marketing/content team), copy just the `_template.md` to a folder of your choice — the structure is generic.

## Adding Custom Skills

Skills live in `.claude/skills/`. To create one:

1. Create a folder: `.claude/skills/your-skill-name/`
2. Add a `SKILL.md` with YAML frontmatter (`name`, `description`, `alwaysApply`) and the skill's instructions
3. Add a `gotchas.md` (read before every run, updated after issues)
4. Reference any config files the skill needs (e.g., `config/morning-sync.md`)
5. Follow the standards in `.claude/skills/STANDARDS.md`

## Add-ons (opt-in patterns)

The default install is intentionally minimal. Patterns that don't apply to every team — engineering, data science, leadership forum, customer-feedback-scan skills — ship under `add-ons/`. To enable any of them, move the directory or skill out of `add-ons/` to its target location:

```bash
mv add-ons/engineering .              # Enable engineering add-on (oncall, post-mortems)
mv add-ons/data-science .             # Enable data-science folder + conventions
mv add-ons/leadership-forum meetings/ # Enable leadership-forum prep workflow
mv add-ons/skills/customer-feedback-scan .claude/skills/  # Enable Slack/Teams/Discord feedback sweep
```

See `add-ons/README.md` for the full list and what each one includes.

## Removing Core Sections

Most core sections are universal, but a few can be removed if not relevant:

| Section | Safe to Remove? | Impact |
|---------|----------------|--------|
| `strategy/` | Not recommended | High-value thinking layer; remove only if team has a separate strategy tool |
| `customer-intelligence/` | Yes | Remove if customer feedback is tracked elsewhere; no other files depend on it |
| `reference-docs/` | Yes | No dependencies |
| `experiments.md` in project template | Yes | Remove from `_template/` if your team doesn't run A/B tests |
| `analytics-spec.md` in project template | Yes | Remove if instrumentation is handled elsewhere |
| `design/` | Not recommended | Projects reference designs.md; keep the contribution guide even if minimal |

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

### Prototyping Workflow
If your team prototypes features against a live codebase (not standalone mockups), create `engineering/prototyping-workflow.md` with:
- Steps: load project context -> load codebase -> find components -> implement with mock data -> iterate
- Prerequisites (repos, tooling, dev server commands)
- Rule: prototype changes are local-only. Learnings go back to the KB.

### Operational Memory (Oncall Learnings)
If your team does oncall or handles recurring investigation patterns, create a `.claude/memories/` folder:
- `oncall-learnings/INDEX.md` -- index of known patterns
- One `.md` per pattern: symptom, investigation steps, resolution
- Skills read INDEX.md before investigating new issues, write new patterns after resolving them
- This creates a self-improving investigation capability -- the agent gets smarter with each incident.
