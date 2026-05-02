# [Alert/Issue Name] Runbook

> **Severity:** [Critical / Warning / Info]
> **Service(s):** [service names]
> **Alert channel:** [#channel-name]
> **Last updated:** YYYY-MM-DD

## What Is This Alert?

[1-2 sentences: what triggers this alert and what it means for users/merchants/the system.]

## Investigation Steps

### Step 1: Check the alert details

[What to look for in the alert message — timestamps, affected entities, error counts.]

### Step 2: Query for scope

```sql
-- Example: check how many entities are affected
SELECT COUNT(DISTINCT entity_id), MIN(created_at), MAX(created_at)
FROM [table]
WHERE [conditions]
  AND created_at > DATEADD(hour, -2, CURRENT_TIMESTAMP())
```

### Step 3: Check dashboards

- [ ] [Dashboard name](link) — look for [specific metric]
- [ ] [Tracing tool](link) — search for [specific trace]

### Step 4: Check recent changes

```bash
# Recent deploys to the affected service
gh api repos/[org]/[repo]/deployments --jq '.[0:5] | .[].description'
```

## Common Causes & Fixes

| Cause | Fix | How to Verify |
|-------|-----|---------------|
| [Cause 1] | [Fix steps] | [Verification query/check] |
| [Cause 2] | [Fix steps] | [Verification query/check] |

## Escalation

If the above doesn't resolve it:
1. Check if a deploy is in progress — wait for it to complete
2. Escalate to [team/person] via [channel]
3. If customer-facing impact, page [oncall rotation]

## Rollback Procedure

```bash
# If a recent deploy caused the issue
[rollback commands or feature flag disable steps]
```

## Related Runbooks

- [Related runbook 1](link)
