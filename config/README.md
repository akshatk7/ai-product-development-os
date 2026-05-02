# Repo configuration

Machine-readable configs consumed by:

- `.githooks/pre-commit` — structural lints run at commit time
- `.github/workflows/repo-hygiene.yml` — structural checks run on push
- `.github/workflows/link-check.yml` — broken-link detection
- `.claude/skills/morning-sync/SKILL.md` — daily hygiene scorecard
- `meetings/weekly/CLAUDE.md` — section scoping lint, no-change filtering, strategic items
- `.claude/agents/monthly-audit.md` — monthly deep-audit scope

## Files

- **`repo-hygiene.json`** — allowed dirs, freshness SLAs, phase requirements, project categories, channel roles, experiment keywords. The generalized version includes example values under `_description` keys — customize for your team.
- **`morning-sync.md`** — channel registry, meeting patterns, DM settings for the morning-sync skill.

## Rules

1. **Only add rules here** — do not hardcode values in scripts. If a new lint is needed, add the threshold/keyword to this config and have the consumer read it.
2. **Pre-commit parses with `jq`.** Any change must be valid JSON. The hook will error out noisily if jq fails.
3. **`_description` keys are documentation** — they are ignored by parsers but read by humans.
4. **Never delete a key without searching consumers first.** Use `grep -r '"key_name"' .githooks .github .claude meetings` before removal.
5. **Customize `project_categories` and `product_keywords_for_eng_ds_filter`** for your product. The shipped values are generic examples.
