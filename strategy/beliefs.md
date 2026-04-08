# Beliefs

> Last reviewed: YYYY-MM-DD

Living hypothesis register. Each belief captures what the team thinks is true about customers, product, and market — with evidence and confidence levels.

**Lifecycle:** Active → Confirmed (sufficient evidence) → or Disproven/Superseded (moved to Retired Beliefs section). See `.claude/rules/document-conventions.md` for full lifecycle rules.

Beliefs are organized into three tiers:
- **Core Beliefs** — High-confidence hypotheses that inform all product work. Agents should prioritize these. Keep to 5-6 max.
- **Working Hypotheses** — Experiment-specific observations that may change as new data arrives. Updated frequently.
- **Market Context** — Stable background truths about customers and competitive landscape. Rarely updated.

---

## Core Beliefs

_The 5-6 beliefs that inform every brief, RFC, and prioritization decision. Read these first._

<!-- Example:
### B1: Customers who see value within the first 7 days retain at 2x the rate
- Confidence: High
- Evidence for: Cohort analysis shows 68% 90-day retention for week-1 engagers vs 34% for others. Confirmed by three subsequent quarterly analyses.
- Evidence against: Self-selection bias not fully controlled — engaged users may be inherently more likely to retain.
- Implications: Onboarding should optimize for time-to-first-value, not feature coverage. Every new feature should have a "first 7 days" story.
- Last challenged: 2026-03-15
-->

## Working Hypotheses

_Active experiment-specific observations. These may change monthly as new data arrives._

<!-- Example:
### B4: Push notifications drive re-engagement but only within 48 hours of last activity
- Confidence: Medium
- Evidence for: Experiment A showed 12% lift in D2 return rate with push, but no effect on D7+
- Evidence against: Experiment B (different copy) showed a smaller D2 effect — copy quality may matter more than timing
- Implications: Push strategy should focus on 24-48 hour win-back, not weekly digests
- Last challenged: 2026-03-20
-->

## Market Context

_Stable truths about customers and competitive landscape. Rarely updated._

<!-- Example:
### B8: Our target customers are not power users — they want simple tools, not configurable ones
- Confidence: High
- Evidence for: User interviews (n=30) consistently show preference for defaults over customization. Competitors with simpler UIs have higher NPS.
- Evidence against: Enterprise segment (10% of users) actively requests configuration
- Implications: Default to opinionated design. Add configuration only when the segment requesting it is large enough to justify the complexity.
- Last challenged: 2026-02-28
-->

---

## Retired Beliefs

_Beliefs moved here when disproven or superseded. Preserves the learning record._

<!-- Example:
### ~~B0: Free users never convert after 30 days~~
**Disproven 2026-02-15:** Reactivation campaign showed 8% conversion from 60+ day dormant users.
Evidence: `data-science/reactivation-analysis/README.md`
-->
