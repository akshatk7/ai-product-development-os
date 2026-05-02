# Monthly Audit Agent

Runs the full repo-hygiene deep audit once a month. Proposes fixes. PM reviews and merges.

## When to use

- **Scheduled:** 1st of each month via the `/schedule` skill (routine below).
- **Ad-hoc:** any time the user asks to "run a repo audit" or "audit the KB."

## How to schedule

Use the `/schedule` skill with this cron and prompt:

```
/schedule create --cron "0 14 1 * *" --name "KB Monthly Audit" --prompt-file .claude/agents/monthly-audit.md
```

Cron `0 14 1 * *` = 14:00 UTC on day 1 of every month (~7am PT, ~9am ET, before a typical Monday sync).

Choose the model per your team's preference (Opus for highest-quality deep analysis, Sonnet for cheaper routine runs).

## The audit prompt

```
Do a comprehensive hygiene audit of the knowledge base repo (already cloned as the working directory).

Pull latest first: git pull.

Skip any check whose target directory doesn't exist at the repo root — `data-science/`, `engineering/`, and `meetings/leadership-forum/` are opt-in add-ons. Note in the PR body which checks were skipped due to disabled add-ons.

Run these checks in parallel (spawn Explore/general-purpose sub-agents via the Task tool as needed for breadth):

1. **Freshness SLAs** (from config/repo-hygiene.json):
   - For every project README, parse Phase and Last updated. If Last updated is older than the SLA for that phase (freshness_sla_days), list it.

2. **projects/INDEX.md drift:**
   - For every project folder, compare its README Phase to the INDEX row. List mismatches.
   - List any project folder missing from INDEX.
   - List any INDEX row pointing to a folder that doesn't exist.

3. **data-science/INDEX.md drift** (skip if `data-science/` not at root):
   - For every analysis folder under data-science/, verify it's listed in INDEX.
   - Flag any analysis whose "Projects Informed" column references a project that doesn't exist.

4. **Lifecycle file requirements** (from config/repo-hygiene.json phase_required_files):
   - For every project, check that required files exist for its Phase. List missing.

5. **Strategy layer cadence:**
   - In strategy/beliefs.md, list beliefs whose "Last challenged" is older than 60 days.
   - In strategy/open-questions.md, if `data-science/` exists at root, cross-reference with data-science/INDEX.md — any active questions that should move to Resolved based on recent analyses?
   - In strategy/competitive.md, flag if Last updated is older than 60 days.

6. **Orphaned files:**
   - For every markdown file under projects/ (and `data-science/` if at root), check whether it's referenced from any INDEX, README, CONTEXT, CLAUDE.md, or a sibling doc. List orphans.

7. **Broken internal links:**
   - Run a lychee-equivalent check on all markdown files (use `lychee` if installed via bash, otherwise grep-based relative-path verification). List broken relative links.

8. **CONTEXT.md journal detection:**
   - Grep for dated section headers (`^## 20[0-9]{2}` or `^## (January|February|March|April|May|June|July|August|September|October|November|December)`) in any CONTEXT.md. Pre-commit blocks these, but verify.

9. **README header compliance:**
   - Every project README must have Phase / Status / Last updated YYYY-MM-DD / Team. List non-compliant.

10. **Skill compliance:**
    - Every .claude/skills/*/ must have SKILL.md + gotchas.md + runs.log. List missing (excluding _deprecated/).

11. **Team files freshness:**
    - team/people.md, team/rituals.md, team/channels.md — any stale content vs repo activity? Flag departed people still in rituals, missing new channels, etc.

Then open a SINGLE pull request with all auto-fixable issues:
- Branch name: `kb-monthly-audit-YYYY-MM` (use actual year-month)
- PR title: "Monthly KB audit: MONTH YYYY"
- PR body: structured report with categories + the fixes applied + a "Still requires human decision" section listing anything the agent couldn't auto-fix.

Hard rules:
- Do not push if the working tree has uncommitted changes. Bail out and notify instead.
- Do not modify strategy/beliefs.md, strategy/open-questions.md, or any decisions.md without explicit approval — these are judgment-heavy and should only be touched by the PM. List proposed changes in the PR body instead.
- Keep the PR under 50 files touched. If more, split by category into multiple PRs (structural fixes, link fixes, freshness bumps).
- If the audit finds > 10 auto-fixable issues, flag in the PR body that daily hygiene is failing — the monthly audit is a safety net, not the primary mechanism.

Report done when PR is open with the URL.
```

## Notes

- The monthly run is a safety net, not the primary mechanism. Most hygiene should be caught by pre-commit, CI, and the daily morning-sync hygiene section.
- If the audit finds >10 issues, that means the daily hygiene is failing. Investigate why.
- Keep the PR under 50 files touched. If more, split by category into multiple PRs (structural fixes, link fixes, freshness bumps).
- The agent needs `gh` CLI with auth to open the PR — standard in most remote Claude Code environments, but verify before scheduling.
