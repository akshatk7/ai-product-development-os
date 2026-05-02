# Changelog

All notable changes to the Product Development OS. Each version represents a meaningful upgrade to the system. Users can check this file to see what's new and decide whether to pull updates into their fork.

---

## v6.0 — 2026-05-02 — "Bare-Bones Core + Opt-In Add-Ons"

Reshape pass driven by the goal of making the OS shareable beyond a single org. Previous versions accumulated patterns that worked well for a mid-size product team with separate Eng/DS/Design functions, on-call rotation, and a leadership forum — but landed as too-much-machinery for the wider audience the OS is intended for: solo PMs, startup PMs, consultants, and PMs at companies whose team shape doesn't match that template.

The core principle: **default install is the smallest useful thing; depth is available via add-ons.** No sophistication is removed — the morning-sync classifiers, beliefs lifecycle, monthly audit agent, four-layer hygiene system are all still there. They just don't dominate the first-clone experience.

### Added — `add-ons/` directory

A new top-level `add-ons/` folder for patterns that don't apply universally. Each add-on is a self-contained directory or skill that users move to their repo root to enable. New `add-ons/README.md` documents the index + enable instructions.

### Moved — Team-shape-specific patterns to add-ons

| Was | Now |
|---|---|
| `engineering/` (oncall, post-mortems, ways-of-working) | `add-ons/engineering/` |
| `data-science/` (DS folder structure, INDEX, backlog, conventions) | `add-ons/data-science/` |
| `meetings/leadership-forum/` (bi-weekly leadership prep workflow) | `add-ons/leadership-forum/` |
| `.claude/skills/customer-feedback-scan/` (Slack/chat feedback sweep) | `add-ons/skills/customer-feedback-scan/` |
| `.claude/skills/call-transcript-scan/` (recorded-call analysis) | `add-ons/skills/call-transcript-scan/` |
| `.claude/skills/investigate-alert/` (alert/oncall investigation) | `add-ons/skills/investigate-alert/` |

To enable any of these, move the directory or skill out of `add-ons/` to its target location. Skills/agents that reference these directories now check existence at runtime and skip gracefully when an add-on isn't enabled.

### Added — New add-ons ported from production use

- **`add-ons/memories/`** — Pattern for repo-wide persistent learnings (cross-cutting gotchas, resolved-incident signatures, standing rules learned the hard way). Different from skill-level `gotchas.md` — these apply to any agent or workflow in the repo. Folder ships with `README.md`, `INDEX.md`, and `_template.md`. Move to `.claude/memories/` to enable. Ported (and generalized) from production use.
- **`add-ons/skills/figma-screenshots/`** — Skill that exports Figma design screens as PNGs into a project's `screenshots/` folder. Uses a `figma-summary.md` node-ID reference table to make repeat exports near-free. Cost-optimized for the Figma MCP. Ported from production use.

### Reframed — `customer-intelligence/`

Rewrote `customer-intelligence/README.md` as a tool-agnostic voice-of-customer placeholder. Previously assumed Slack feedback channels and call transcripts as primary sources. Now treats the folder as an open-ended home for any customer signal — support tickets, NPS, sales calls, in-product feedback, social mentions, churn interviews, etc. — with the specific scan skills moved to add-ons. The folder ships with just the README; subfolders (deep-dives/, scans/) are created organically.

### Slimmed — `QUICKSTART.md`

Reduced from 11 setup steps to 5 essential ones (clone, fill team, describe product, seed strategy, create first project). Slack registration, glossary build, customer-feedback channel registration, and roadmap connection are now in a "Expanding from here" section that points to CUSTOMIZE.md and add-ons/. Goal: time-to-first-value drops from "did all 11 steps" to "did 5 steps; everything else can wait."

### Updated — Skills and agents made directory-conditional

`brief-starter`, `monthly-audit`, `project-analyst`, `repo-navigator`, and references in `.claude/rules/data-science.md` and `.claude/rules/meetings.md` now check whether `data-science/`, `engineering/`, or `meetings/leadership-forum/` exist at the repo root before referencing them. Add-on-disabled installs no longer hit "directory doesn't exist" paths.

### Updated — Routing artifacts

- `CLAUDE.md` repo structure tree, ownership table, and workflow table reflect the new core/add-ons split.
- `.cursorrules` routing table updated.
- `config/repo-hygiene.json` adds `add-ons` to `allowed_root_dirs`. `data-science` and `engineering` remain allowed (for users who enable those add-ons by moving them to root).
- `.githooks/pre-commit` routing-guide error message updated.
- `CUSTOMIZE.md` adds an Add-ons section with enable commands; the "Removing Optional Sections" table is trimmed to just the genuinely-removable core sections.

### Not changed

- `README.md` is intentionally untouched — it's the GitHub landing page and serves dual duty as marketing and onboarding.
- Core skills (morning-sync, digest-meeting, brief-starter, ship-review-prep, resolve-open-questions) and core agents (brief-reviewer, decision-auditor, project-analyst, repo-navigator, monthly-audit) remain in `.claude/`.
- All hygiene enforcement (pre-commit, CI, daily morning-sync, monthly audit) remains active.
- `data-science/` and `engineering/` remain in `allowed_root_dirs` — moving them out of `add-ons/` to root does not require a config change.

### Migration note for existing users

If you've been using a previous version with `engineering/` or `data-science/` at root, your repo will continue to work as-is — the directories remain in `allowed_root_dirs`. The change only affects fresh clones, where these directories now ship under `add-ons/` and require an explicit `mv` to enable.

---

## v5.0 — 2026-04-23 — "Config-Driven Hygiene + Skill Linting"

Major update backported from a second-wave audit of a production PM knowledge base. Where v4.0 added guardrails, v5.0 makes them **config-driven** (single source of truth), adds two new enforcement layers (link-check CI + monthly audit agent), and overhauls the morning-sync and weekly-prep skills so the PM's manual review drops from ~30% rejection rate to a categorized triage pass. Every change is battle-tested in production.

### Added — Config as single source of truth
- **`config/repo-hygiene.json`** — One config file, multiple consumers. Defines allowed dirs, freshness SLAs by phase, phase-required files, project categories, channel roles, experiment-decision keywords. Pre-commit, CI, and skills all read from here. No more CI-vs-hook drift.
- **`config/README.md`** — How to update. Rules: only edit the config, never hardcode values in consumers.

### Added — Enforcement layers
- **`.github/workflows/link-check.yml`** — Weekly + per-push lychee link check. Catches broken internal markdown links (the `chorus-call-analysis.md`-style rot we kept hitting manually). External auth-gated domains excluded by default — customize for your team's tools.
- **Pre-commit README header convention check (ERROR)** — Every `projects/*/*/README.md` must have Phase / Status / Last updated YYYY-MM-DD / Team. Format drift was a recurring cleanup target; now it's blocked at commit time.
- **Pre-commit lifecycle file check (WARNING)** — When a project's phase is set to Designs / RFCs / Coding / Live, required files (product-brief, rfc, updates, decisions) must exist. Forces seeding at phase-transition time instead of discovering the gap during monthly hygiene.
- **Pre-commit strategy activity check (WARNING)** — Warns if `beliefs.md` is committed with no `Last challenged` fields.
- **CONTEXT.md journal check promoted to ERROR** (was WARNING in v4.0).
- **Skills now require `runs.log` in addition to `gotchas.md`** (ERROR).

### Added — `.claude/agents/monthly-audit.md`
A scheduled deep-audit agent template. User runs `/schedule` with the included prompt; on the 1st of each month, it runs the full hygiene audit and opens a single PR with proposed fixes. Safety net for whatever slips through pre-commit + CI + daily morning-sync.

### Added — `roadmap/strategic-initiatives.md`
Template for ongoing strategic workstreams (big bets, adoption strategy, cross-team alignments) that don't generate daily Slack pings but need weekly coverage. The weekly prep skill auto-reads this file and includes `status: active` items in Top of Mind. Prevents "strategic item dropped from weekly update because no Slack thread fired" drift.

### Changed — `.githooks/pre-commit` (rewrite)
Hook now:
- Reads all thresholds from `config/repo-hygiene.json` via jq (fails loudly if jq missing or JSON invalid).
- Includes 10 checks (up from 7 in v4.0): root placement, CLAUDE.md size, CONTEXT journal (ERROR), file size, skills compliance (ERROR + runs.log), INDEX co-change (WARN), .DS_Store, README convention (ERROR), lifecycle files (WARN), strategy activity (WARN).

### Changed — `.github/workflows/repo-hygiene.yml` (rewrite)
Four jobs now (up from one): check-root-files, context-md-journal, claude-md-length, readme-convention. All four read from `config/repo-hygiene.json`.

### Changed — `.claude/rules/document-conventions.md`
- **Freshness SLA by phase** table added (Live 7d, Coding 14d, ... Exploratory 60d).
- **Lifecycle file requirements** table added.
- **Automated Hygiene Enforcement** section documents the 4-layer model (pre-commit, CI, morning-sync daily, monthly agent).
- Monthly hygiene checklist reframed as manual backstop to the agent.

### Changed — `.claude/skills/morning-sync/SKILL.md` (major rewrite)
Ten additions, all addressing friction patterns observed in production:
- **Phase 0 mandatory pre-run checklist** — python3 timestamp computation with inverse verification (fixes recurring 1-year-off bug), runs.log overlap check, gotchas read, day-name/date validation.
- **Channel role tags** (pm/eng/ds/monitor) — eng/ds roles only surface items with product keywords. Cuts ~25% of dismissable eng-noise items.
- **DM Heuristic Classifier** — 4-bucket filter (logistical / casual / concrete / ack). Only extracts concrete. Cuts DM over-extraction by ~70%.
- **Experiment-Decision Confidence Classifier** — "decided"/"winner" claims require explicit confirmed-tier keyword from transcript. Prevents the recurring overclaim-a-still-running-experiment pattern.
- **Auto-Actions Layer (Phase 2b)** — auto-propose (never write) updates to `team/people.md` on staffing signals; propose README phase changes on keyword triggers; propose `projects/INDEX.md` rows for new projects; draft Slack summary for posting (awaits approval).
- **Report Format: 4 sections** — Hygiene Scorecard at the top (stale SLAs, INDEX drift, stale beliefs, orphans) + Proposed Updates + Deliverables & New Work + categorized Needs-Your-Input (5 buckets instead of a flat pile).
- **Pre-Output Linter** — 8 checks run before presenting draft. Turns accumulated gotchas into code, not re-reads.
- **user_rejection_rate** tracked in runs.log — >30% for 2 runs = stop and analyze instead of shipping a broken classifier.

### Changed — `meetings/weekly/CLAUDE.md` (major rewrite)
- **Phase 0** — load `config/repo-hygiene.json` + read `roadmap/strategic-initiatives.md`.
- **Step 2 git-diff filter** — projects with zero commits in the week are auto-omitted (unless `monitoring: yes` or in strategic-initiatives).
- **Step 2.0** — strategic-initiatives auto-inclusion.
- **Step 5 spreadsheet write-back** — requires a proposed-changes table in the draft before any cell is written.
- **Step 5.0** — cross-reference mismatch detection (the unique value of weekly-status skill, now folded in).
- **Step 6 roll-forward discipline** — explicit `[changed]/[unchanged]/[complete]/[monitoring]` tags per item.
- **Step 9 Pre-Output Linter** — 9 checks including section scoping lint (categories in config), discussion-topics cap=5, duplication check, keyword-based experiment-decision check.

### Deprecated
- **`.claude/skills/weekly-status/`** → `.claude/skills/_deprecated/weekly-status/`. Its unique logic is folded into weekly prep Step 5.0. Two overlapping skills produced split gotchas and unclear trigger semantics. Scheduled for deletion after one month of parallel availability per STANDARDS.md.

### Context
Six weeks after v4.0 shipped, a deep audit of a production PM knowledge base revealed two categories of residual friction: (1) rule sprawl — gotchas.md was accumulating lessons but the rules weren't enforced automatically, so the PM did the enforcement on every run; (2) skill overlap — two skills for "weekly" work with unclear boundaries. v5.0 moves enforcement from "read the gotchas" to "linter checks before output," introduces config as the single source of truth across all consumers, and consolidates the weekly flow into one skill.

---

## v4.0 — 2026-04-22 — "Guardrails & Analytics"

Major update backported from 6 weeks of production use. Adds structural guardrails that prevent the most common drift patterns, 4 analytics/DS skills, 4 PM workflow commands, and documentation improvements. Every addition was battle-tested in production before being generalized.

### Added — Structural Guardrails
- **Enhanced pre-commit hook** — 7 checks: root file placement, CLAUDE.md line limit (150 max), CONTEXT.md journal detection (blocks dated section headers), file size limit (500KB), skills compliance (SKILL.md requires gotchas.md), INDEX.md co-change reminder, .DS_Store blocking. Errors block; warnings advise.
- **`.claude/rules/claude-md-ownership.md`** — Routing table for where content belongs. Prevents CLAUDE.md bloat by defining that it's orientation only (max 150 lines, PM-owned) and directing rules/setup docs to their correct homes.

### Added — Skills
- **`/power-analysis`** — Statistical power sizing for A/B experiments: sample size, MDE, duration, sensitivity analysis. Supports proportions, continuous, and ratio metrics.
- **`/causal-analysis`** — Full causal inference workflow: DiD, RDD, IV, synthetic control, propensity matching, CATE, meta-learners, DML. Method selection guide, implementation templates, validation checks.
- **`/sql-validation`** — Static SQL analysis: fanout join detection, NULL handling, date boundary errors, aggregation issues, performance anti-patterns, Snowflake-specific pitfalls. Run before executing queries.
- **`/bug-bash`** — Structured test plan execution: orient → learn designs → execute test plan → file bugs → report. Works with Google Sheets test plans, Figma designs, and any tracker.

### Added — Commands
- **`/debrief-thread`** — Parse a Slack thread URL → read full thread → structured debrief with topic, context, key perspectives, expected actions, and linked resources.
- **`/prep-meeting`** — Gather context from meeting notes, project files, Slack, and status trackers to draft a meeting briefing and agenda.
- **`/generate-launch-email`** — Generate internal product launch emails following best practices: TL;DR, problem, solution, visuals, results, what's next, team.
- **`/review-launch-email`** — Audit a draft launch email against best practices. Section-by-section checklist, scoring (Strong/Solid/Needs Work), and prioritized rewrite suggestions.

### Changed — Rules & Standards
- **document-conventions.md** — Added RFC Workflow Integration (full PR URLs, ticker tracker sync after PR creation) and Data Output Policy (no CSVs in git, 500KB limit, data warehouse guidance).
- **meetings.md** — Added Meeting → File Routing template table: 5 meeting types mapped to primary outputs and secondary update targets. Teams customize for their meetings.
- **STANDARDS.md** — Added Portability Rules (no absolute paths, no hardcoded IDs/URLs/names) and Deprecation Procedure (mark → keep 1 month → archive/delete).

### Changed — Documentation
- **CUSTOMIZE.md** — Added two optional add-ons: Prototyping Workflow (live codebase prototyping pattern) and Operational Memory / Oncall Learnings (`.claude/memories/` with self-improving investigation patterns).

### Context
A deep audit of a production PM knowledge base (200+ commits, 27 projects, 5 months) revealed three categories of issues: (1) structural drift that guardrails can prevent (CLAUDE.md bloat from 100→220 lines, CONTEXT.md becoming journals, large CSVs committed), (2) missing PM workflow tooling (meeting prep, Slack debriefs, launch emails, QA), and (3) missing analytics capabilities (power analysis, causal inference, SQL validation). This release closes all three gaps with proven patterns — every skill and guardrail was tested against real project work before being generalized.

---

## v3.4 — 2026-04-08 — "Action Items"

Morning sync now extracts PM action items alongside repo updates. Previously, the sync only captured knowledge (decisions, context, status) but missed things the PM needs to DO (send a Slack message, schedule a meeting, reply to a DM, update a doc). Action items are presented in the report as an executable table, and the agent helps draft and send messages, create calendar events, etc. after repo updates are committed.

### Changed
- **morning-sync skill** — Added action item extraction in Phase 2, "Action Items (You)" section in Phase 3 report, and Phase 5 for executing approved action items via Slack/Calendar/Docs MCPs.

### Context
Real-world usage showed that ~40% of meeting outcomes are action items, not knowledge updates. The morning sync was capturing "what we learned" but losing "what we need to do." PMs were tracking action items in a separate mental list or a different Claude session. Now it's one workflow: scan → propose repo updates → surface action items → execute both.

---

## v3.3 — 2026-04-07 — "Pull, Don't Push"

Backported from a cross-repo audit comparing two production PM knowledge bases. Key insight: prescriptive "read everything before doing anything" instructions burn agent context window on prerequisites instead of the actual task. These changes make the system pull-based — agents load context proportional to the task, not proportional to the repo.

### Changed
- **CLAUDE.md shortened** — Cut from 318 lines to ~100 lines. Now an orientation doc only. All prescriptive rules remain in `.claude/rules/`. Reduces context window consumption at session start.
- **beliefs.md tiered** — Three tiers: Core Beliefs (5-6 max, inform all product work), Working Hypotheses (experiment-specific, change monthly), Market Context (stable background). Agents now know which beliefs to prioritize. Template and examples updated.
- **Strategy layer: pull-based context loading** — Replaced prescriptive "every session: read beliefs, every meeting: check open-questions" with "pull context as needed based on the task." Strategy signals flagged only when genuinely present, not forced on every workflow.
- **GH Action now fails** — `repo-hygiene.yml` exits with error (not warning) when unauthorized root files are added. Matches pre-commit hook severity.

### Context
A comparative audit with another PM team's knowledge base revealed that this template's "read everything first" philosophy, while thorough, creates a context window problem at scale. The fix: keep all the same knowledge infrastructure, but let agents pull what they need instead of pushing everything upfront.

---

## v3.2 — 2026-04-04 — "Self-Maintaining Knowledge"

Backported from a full repo audit of a production PM knowledge base (27 projects, 3+ months). Addresses the most common drift patterns: INDEX.md going stale between hygiene runs, source doc links losing freshness tracking, experiment results not feeding back into strategic questions, and beliefs accumulating without lifecycle management.

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
Audit of 20 project folders found two failure patterns: (1) SL Integration CONTEXT.md was 835 lines, 52% chronological across 10 dated sections. (2) First Redemption Acceleration was 133 lines, 71% chronological. Meanwhile, well-structured projects like Transparency Metrics (65 lines, 0% chronological) proved the pattern works when content is properly routed. The fix separates the "what is this project" narrative from the "what happened this week" journal.

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
