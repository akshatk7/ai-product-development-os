# Changelog

All notable changes to the Product Development OS. Each version represents a meaningful upgrade to the system. Users can check this file to see what's new and decide whether to pull updates into their fork.

---

## v3.3 — 2026-04-07 — "Pull, Don't Push"

Backported from a cross-repo audit comparing a production knowledge base against a second team's implementation. Key insight: prescriptive "read everything before doing anything" instructions burn agent context window on prerequisites instead of the actual task. These changes make the system pull-based — agents load context proportional to the task, not proportional to the repo.

### Changed
- **CLAUDE.md shortened** — Cut from 318 lines to ~100 lines. Now an orientation doc only. All prescriptive rules remain in `.claude/rules/`. Reduces context window consumption at session start.
- **beliefs.md tiered** — Three tiers: Core Beliefs (5-6 max, inform all product work), Working Hypotheses (experiment-specific, change monthly), Market Context (stable background). Agents now know which beliefs to prioritize. Template and examples updated.
- **Strategy layer: pull-based context loading** — Replaced prescriptive "every session: read beliefs, every meeting: check open-questions" with "pull context as needed based on the task." Strategy signals flagged only when genuinely present, not forced on every workflow.
- **GH Action now fails** — `repo-hygiene.yml` exits with error (not warning) when unauthorized root files are added. Matches pre-commit hook severity.

### Context
A comparative audit with a second team's knowledge base revealed that our template's "read everything first" philosophy, while thorough, creates a context window problem at scale. The fix: keep all the same knowledge infrastructure, but let agents pull what they need instead of pushing everything upfront.

---

## v3.2 — 2026-04-04 — "Self-Maintaining Knowledge"

Backported from a full repo audit of the production knowledge base (27 projects, 3+ months). Addresses the most common drift patterns: INDEX.md going stale between hygiene runs, source doc links losing freshness tracking, experiment results not feeding back into strategic questions, and beliefs accumulating without lifecycle management.

### Added — Conventions
- **INDEX.md drift check in morning-sync** — Morning sync now cross-checks `projects/INDEX.md` phases against actual README.md phases for every project mentioned that day. Catches drift between monthly hygiene runs.
- **"Last verified" dates on source doc links** — CONTEXT.md template now includes `(verified YYYY-MM-DD)` in the source documents table. Tracks when a human last confirmed the Google Doc matches the markdown.
- **updates.md seeding rule** — Projects should get `updates.md` once they reach or pass Design phase (not Coding). This is when cross-functional handoffs start generating dated updates worth logging.
- **Experiment readouts → open-questions routing** — When an experiment completes, check `strategy/open-questions.md` for questions the results can resolve. Move resolved questions to the Resolved section.
- **Beliefs lifecycle** — Replaces simple "invalidated" pattern with full lifecycle: Active → Confirmed (mark with evidence pointer) → Disproven/Superseded (move to Retired Beliefs section). Prevents beliefs.md from growing indefinitely.
- **Expanded monthly hygiene checklist** — Now includes: updates.md presence check (Design+ projects), open-questions resolution audit, beliefs retirement review, INDEX.md phase cross-check.

### Changed
- `strategy/beliefs.md` — Added `## Retired Beliefs` section. Updated header to describe lifecycle.
- `strategy/CLAUDE.md` — Updated beliefs lifecycle description.
- `projects/_template/CONTEXT.md` — Source doc table now shows verified date pattern.
- `.claude/rules/document-conventions.md` — Added beliefs lifecycle section, experiment→open-questions routing, source doc verification convention, updates.md seeding rule, expanded hygiene checklist.
- `.claude/skills/morning-sync/SKILL.md` — Added INDEX.md drift check step after status freshness check.

### Context
A full audit of the production knowledge base revealed that the most persistent maintenance issues weren't structural (the file boundaries work well) but lifecycle-related: beliefs accumulate but never retire, experiment results don't flow back to strategic questions, source doc links go stale silently, and INDEX.md drifts between monthly hygiene runs. These six changes make the system more self-maintaining by closing feedback loops that were previously manual.

---

## v3.1 — 2026-04-04 — "Beyond the File Cabinet"

Reframes the OS from "structured knowledge base" to "proactive, connected operating system." New README communicates what makes this different from a wiki: cross-referencing, proactive flagging, self-improving loops, and compounding knowledge. Adds engineering operational infrastructure and two new skills.

### Added — Skills
- `.claude/skills/pr-review/` — Review PRs against the team's learned review patterns. 8 generic patterns (safety & rollback, DRY, operational readiness, scope, edge cases, performance, design patterns, API consistency). Teams customize with their own examples.
- `.claude/skills/investigate-alert/` — Systematic production alert investigation: gather → map to code → query error data → trace services → document. Creates runbooks for new alert types.

### Added — Engineering Oncall Infrastructure
- `engineering/oncall/README.md` — Oncall duties, debugging tool registry, runbook index
- `engineering/oncall/_template.md` — Runbook template: what fired, how to investigate, common causes, escalation, rollback
- `engineering/oncall/faq-common-asks.md` — Quick-reference for recurring inbound requests

### Changed
- **README.md** — Complete rewrite. Leads with "This Is Not a File Cabinet." Explains the 6 differentiators: automatic cross-referencing, proactive flagging, strategy layer, self-improving loops, 7 integrations, and compounding knowledge. Written for LinkedIn/Slack audiences.
- **CLAUDE.md** — Updated repo structure tree with oncall/ and new skills. Added oncall runbooks to engineering ownership. Added PR review and alert investigation to recurring workflows table.
- **engineering/CLAUDE.md** — Added oncall runbook workflow, FAQ maintenance guide, and investigate-alert skill reference.

### Context
The most common reaction to this repo was "oh cool, a shared knowledge base." That undersells it by 10x. The README now leads with what actually makes it different: it cross-references past projects when you write a brief, flags stale decisions before you notice them, learns from its own mistakes, and gets smarter with every use. The engineering additions close the biggest capability gap — production operations (oncall, alerts, runbooks) were missing entirely despite being core to how eng teams actually work.

---

## v3.0 — 2026-04-02 — "The Thinking Layer"

The OS evolves from project management into strategic product thinking. Adds a strategy layer (beliefs, competitive intel, open questions), customer intelligence infrastructure, goal tracking, and repo guardrails. Backported from 3+ months of production use across 20+ projects.

### Added — Strategy Layer
- `strategy/CLAUDE.md` — How the thinking layer works, conventions, integration instructions
- `strategy/beliefs.md` — Living hypothesis register with confidence levels, evidence, challenge dates. Template with 4 thematic sections.
- `strategy/competitive.md` — Competitive observations organized by domain with source/relevance tracking
- `strategy/open-questions.md` — Strategic uncertainty register with resolution lifecycle (Active → Resolved)
- Strategy integration in all workflows: every meeting digestion, morning sync, brief/PRD/RFC now checks and updates beliefs
- File boundary rules updated: strategic hypotheses → `strategy/beliefs.md`, competitive intel → `strategy/competitive.md`, big questions → `strategy/open-questions.md`
- Monthly hygiene now checks for stale beliefs (>30 days unchallenged) and stagnant open questions

### Added — Customer Intelligence
- `customer-intelligence/README.md` — Sources, skill triggers, channel registry, folder structure
- `.claude/skills/customer-feedback-scan/` — New skill: Slack channel sweep for product signal. 5-phase execution, signal classification (bug/feature/confusion/churn/positive), theme synthesis, strategy cross-reference.

### Added — Skills
- `.claude/skills/digest-meeting/` — Meeting transcript → repo file routing. Pulls transcript, identifies meeting type, extracts decisions/actions/context, routes to correct files, checks strategy layer.
- `.claude/skills/weekly-status/` — Cross-referenced status snapshot from all tracking sources. Flags mismatches between repo, tracker, and roadmap.

### Added — Engineering
- `engineering/post-mortems/_template.md` — Incident retro template: what happened, timeline, root cause, impact, action items, lessons learned

### Added — Repo Guardrails
- `.cursorrules` — AI contributor rules for Cursor IDE users: file routing table, strategy layer integration, organic file creation rule
- `.githooks/pre-commit` — Blocks new files/dirs at repo root outside approved list. Allowlist-based with routing guidance in error messages.
- `.github/workflows/repo-hygiene.yml` — CI workflow that warns on unauthorized root files (advisory, non-blocking)

### Changed
- CLAUDE.md: Added strategy layer section, customer-intelligence section, goals section, updated repo structure tree, expanded meeting→file mapping with strategy routes, expanded file boundary rules, updated ownership (PM owns strategy/), expanded proactive optimization checklist
- QUICKSTART.md: Added steps 7 (seed strategy), 8 (set big goal), 11 (register feedback channels). Updated daily workflow summary with strategic signals and feedback scanning.
- CUSTOMIZE.md: Added sections for strategy customization, customer intelligence customization, meeting notes tool, post-mortems, repo guardrails, and optional add-ons (goal tracking, ML models, diagnosis workflows, OKRs). Updated removable sections table.
- README.md: Updated "four layers" description (added strategy), added "What's Included" table with full component inventory
- `.claude/rules/document-conventions.md`: Added strategy file routing to boundary table, added strategy propagation to knowledge propagation rule, added beliefs/questions staleness to monthly hygiene checklist
- Project organization: Documented categorical project organization pattern (flat vs. by-area)

### Context
After 3+ months of production use across 20+ active projects, the biggest gap was strategic thinking. Projects were well-managed but hypotheses lived in people's heads, competitive observations were scattered across meeting notes, and strategic questions went untracked. The strategy layer makes this explicit and integrates it into every workflow — meeting digestion checks beliefs, morning syncs flag strategic signals, briefs reference open questions. Customer intelligence was the second gap: feedback from multiple channels (Slack, calls, support) wasn't being systematically captured. The customer-intelligence folder and scanning skills close this loop. Repo guardrails prevent the most common structural drift pattern: files appearing at the wrong level.

---

## v2.2 — 2026-03-29 — "Narrative vs Journal"

Introduces `updates.md` to separate chronological project logs from narrative context. Learned from production use: active projects' CONTEXT.md files were becoming 50-70% dated update sections ("## March 16 Updates"), burying the stable narrative. Clean projects (65 lines, 0% chronological) proved the system works when content is properly routed.

### Added
- `updates.md` — New project file type for chronological updates (meeting digests, status changes, action items). Newest entries first. Created organically when needed.
- File boundary rules now distinguish CONTEXT.md (narrative briefing) from updates.md (chronological log)
- Morning sync and meeting digestion now route dated content to `updates.md` instead of appending to CONTEXT.md

### Changed
- CONTEXT.md convention: now explicitly "narrative only, no dated sections." Reads like a 2-minute project briefing.
- Meeting → file mapping: secondary updates now route to `updates.md` instead of CONTEXT.md
- Project context rule: added `updates.md` for "what happened recently" questions
- Reinforced: files created organically, never pre-created empty

### Context
Audit of 20 project folders found two failure patterns: (1) one project's CONTEXT.md was 835 lines, 52% chronological across 10 dated sections. (2) Another was 133 lines, 71% chronological. Meanwhile, well-structured projects (65 lines, 0% chronological) proved the pattern works when content is properly routed. The fix separates the "what is this project" narrative from the "what happened this week" journal.

---

## v2.1 — 2026-03-26 — "Systemic Resilience"

Backported learnings from 2+ months of production use. Focused on preventing the most common failure patterns: status rot, content in wrong files, and weak cross-linking.

### Added
- `.claude/rules/document-conventions.md` — File boundary rules (what content goes in which file), status freshness requirements, cross-reference requirements, monthly repo hygiene checklist
- `.claude/rules/data-science.md` — DS analysis conventions (naming, indexing, cross-posting findings)
- `.claude/rules/meetings.md` — Meeting notes routing conventions
- `data-science/backlog.md` — Template for tracking open DS questions and analysis requests
- `projects/_template/CONTEXT.md` — Was missing from the template despite being a core project file
- Morning sync: status freshness check phase (catches stale project statuses automatically)
- Morning sync: file routing enforcement (routes decisions to decisions.md, designs to designs.md, etc.)
- Morning sync: INDEX update step in execution phase
- `eng-task-management` skill: added missing gotchas.md and runs.log (now compliant with STANDARDS.md)
- `.claude/skills/call-transcript-scan/` — New skill: speaker-aware call transcript analysis for product feedback. Includes parameterized SQL templates, universal signal patterns (cancel intent, confusion, spend concern, etc.), co-occurring signal analysis, tenure breakdowns, and qualitative sample pull. Generalized from production use analyzing 2,174+ call transcripts.

### Fixed
- QUICKSTART.md: `cd pm-dev-os` → `cd ai-product-development-os` (correct repo name)
- QUICKSTART.md: Step 3 now correctly says to edit `config/morning-sync.md` (not `team/channels.md` which is auto-generated)
- `.gitignore`: replaced oversized Node.js template with minimal markdown-repo gitignore
- CLAUDE.md: standardized path references to flat `projects/{project}/` (removed inconsistent `[area]/` nesting)
- CLAUDE.md: added CONTEXT.md to project structure documentation

### Context
These changes address the three most common failure patterns observed in production:
1. **Status rot** — project statuses going stale as work advances through phases
2. **Content in wrong files** — CONTEXT.md becoming a dumping ground for decisions, designs, and system findings
3. **Weak cross-linking** — analyses, meeting notes, and projects not referencing each other

---

## v2.0 — 2026-03-22: Proactive Agent Layer

The OS evolves from a passive knowledge base into a proactive agent. Skills get smarter per run, agents answer questions from repo context, and cross-project knowledge flows automatically.

### New: Self-Improving Skills
- **`/brief-starter`** — Auto-gathers learnings from past projects, customer feedback, system constraints, and leadership review patterns before you start writing a brief.
- **`/ship-review-prep`** — Validates project readiness: checks artifact completeness, flags open questions and blockers, checks cross-project dependencies.
- **`/resolve-open-questions`** — Scans all projects for TBD/pending items, checks Slack and meetings for resolution evidence, proposes edits.
- **`/morning-sync`** — Upgraded from config-only to a full skill. Now uses parallel agents, chronological synthesis, report-as-diffs (not raw event logs), DM noise filtering, eng/DS-only filtering, and date validation.
- **Skill Standards (`.claude/skills/STANDARDS.md`)** — Every skill must have `gotchas.md` + `runs.log` + mandatory pre-run/post-run steps. The self-improvement loop: run → capture failures → next run reads failures → avoids them.

### New: Agents
- **`project-analyst`** — Given a project name, reads all its files and answers any question about it.
- **`brief-reviewer`** — Reviews draft briefs against leadership feedback patterns from `product/review-taste.md`.
- **`decision-auditor`** — Cross-checks decisions across all projects for contradictions, stale TBDs, and cascade risks.
- **`repo-navigator`** — Interactive onboarding guide. Answers "where do I find X?" with file paths.

### New: Cross-Project Knowledge Retrieval
- **`projects/INDEX.md`** — Cross-project index mapping every project to its systems, domains, related projects, and key artifacts. The lookup table for "what past projects touched this system?"
- **`data-science/INDEX.md`** — Maps DS analyses to the projects they inform.
- **`.claude/rules/project-context.md`** — Auto-loads when working in `projects/`. When creating new work (brief, RFC, project), automatically scans past projects for relevant constraints, decisions, and patterns.
- **`projects/_template/README.md`** — Added "Related Projects & Inherited Constraints" section.

### New: Templates & Conventions
- **`product/review-taste.md`** — Template for capturing recurring leadership feedback patterns across reviews.
- **Proactive Optimization directive** in CLAUDE.md — Agent proactively flags stale files, broken skill loops, and drifted structures.

---

## v1.1 — 2026-03-20: Template Alignment

Minor updates to align the template with learnings from production usage.

---

## v1.0 — 2026-03-17: Initial Release

The foundational Product Development OS.

### Core Structure
- **`product/`** — Product overview, terminology glossary, metrics definitions, PM workflows
- **`team/`** — Org chart, meeting rituals, Slack channel registry
- **`roadmap/`** — Roadmap management workflow with spreadsheet integration
- **`projects/`** — Project lifecycle with 11-file template (README, CONTEXT, brief, designs, decisions, truths, RFCs, experiments, analytics spec, launch checklist, scratchpad) + fully worked example
- **`meetings/`** — Weekly and leadership forum prep workflows
- **`engineering/`**, **`design/`**, **`data-science/`** — Discipline-specific contribution guides

### Workflows
- Weekly meeting prep ("write top of mind for [date]")
- Leadership forum prep
- Roadmap updates
- Meeting digestion
- Morning sync (config-based)

### Conventions
- Decision tracking with supersession pattern (strikethrough + "Superseded by")
- Truths accumulation (incremental research findings)
- Google Docs as living source of truth, markdown as snapshots
- Knowledge propagation (update stale files when new info arrives)
- Conflict resolution protocol
- Mermaid task diagrams with 4 states

### Skills
- `eng-task-management` — Sync Mermaid diagrams with project tracker tickets
