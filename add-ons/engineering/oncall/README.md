# Oncall Runbooks

This folder contains operational runbooks for the team's on-call rotation. Each runbook documents a specific alert or incident type with steps to investigate and resolve.

## How to Use

- **Start here** for any alert — check if a matching runbook exists below
- **FAQ** (`faq-common-asks.md`) covers recurring inbound requests and quick-reference procedures
- If no runbook exists, investigate and **create one** using the template so the next person doesn't start from scratch

## Contents

- [FAQ: Common Asks & Oncall Reference](./faq-common-asks.md) -- **Start here for recurring asks**
<!-- Add runbooks here as they're created:
- [Service X Alert Runbook](./service-x-alert-runbook.md)
- [E2E Test Failure Runbook](./e2e-test-failure-runbook.md)
-->

## Oncall Duties

- **Alerts:** Respond to production alerts in the team's alert channel
- **Inbounds:** Handle requests from partner teams via Slack channels
- **SLO monitoring:** Check SLO burn rate dashboards during rotation
- **E2E tests:** Investigate and fix flaky or broken end-to-end tests

## Creating a New Runbook

Copy `_template.md` and fill in the sections. Name the file descriptively: `{service-or-feature}-{alert-type}-runbook.md`.

Every runbook should answer:
1. **What fired?** — Alert name, severity, what it means
2. **How to investigate** — Step-by-step with specific queries, dashboards, and log commands
3. **How to resolve** — Common fixes, rollback procedures, escalation paths
4. **How to verify** — Confirm the fix worked

## Useful Debugging Tools

| Tool | Purpose | Link |
|------|---------|------|
| Observability dashboard | SLO burn rate, error rates | `[your dashboard URL]` |
| Tracing tool | Request traces, latency breakdown | `[your tracing URL]` |
| Log search | Structured log queries | `[your log search URL]` |
| Order/entity debugger | Look up specific entities | `[your debug tool URL]` |
