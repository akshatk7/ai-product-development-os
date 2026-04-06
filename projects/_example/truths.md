# Merchant Dashboard Redesign — Research & Findings

> This is a fictional example demonstrating how truths.md captures incremental data/system discoveries.

Facts about data tables, backend systems, and data flows discovered during implementation. Updated incrementally as new findings emerge.

---

## Revenue Attribution Tables

_Discovered: 2026-03-01, by Sam Chen_

- The `merchant_revenue` table splits into `organic_revenue` and `marketing_revenue` columns, but `marketing_revenue` double-counts co-funded orders (orders where both the merchant and the platform contributed to the discount).
- **Workaround:** Join against `campaign_attribution` table and deduplicate by `order_id` before aggregating.
- This is a known data quality issue tracked in JIRA as DATA-5678.

## Dashboard API Performance

_Discovered: 2026-03-08, by Sam Chen_

- The current dashboard API response time exceeds 3 seconds for merchants with >500 orders/day when querying 90-day ranges.
- Root cause: real-time aggregation over the `orders` table with no pre-computed views.
- **Solution:** Create a materialized view `mv_merchant_revenue_daily` that pre-aggregates daily revenue by merchant and source (organic/marketing). Refresh nightly via cron.
- Expected improvement: sub-200ms response times for all merchants.

## Campaign Attribution Coverage

_Discovered: 2026-03-10, by Priya Patel_

- 94% of marketing orders have a `campaign_id` in the `campaign_attribution` table.
- The remaining 6% are from legacy campaigns (pre-2025) that used a different attribution system.
- **Decision:** Show these as "Other marketing" in the breakdown rather than hiding them. Merchants should see all marketing revenue, even if we can't attribute it to a specific campaign.

## Mobile Viewport Constraints

_Discovered: 2026-03-12, by Alex Rivera (Design)_

- On mobile (375px width), the stacked bar chart becomes unreadable with more than 30 data points on the x-axis.
- **Solution:** On mobile, 90-day view uses weekly buckets (13 bars) instead of daily buckets (90 bars). 30-day and 7-day views remain daily.
