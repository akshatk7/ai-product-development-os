# Merchant Dashboard Redesign — Backend RFC

> This is a fictional example demonstrating the expected depth level for a backend RFC.

## Summary

Add a revenue breakdown endpoint to `dashboard-service` that returns daily organic vs marketing-driven revenue for a given merchant and time range. Pre-compute daily aggregates in a materialized view for performance.

## Repo(s)

- `dashboard-service` — new endpoint + materialized view migration

## Approach

Create a materialized view `mv_merchant_revenue_daily` that joins `orders`, `campaign_attribution`, and `merchant_revenue` tables to pre-aggregate daily revenue by source. Expose this via a new REST endpoint. Refresh the materialized view nightly via an Airflow DAG.

## API Changes

### New Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/v1/merchants/{merchant_id}/revenue-breakdown` | Returns daily revenue breakdown for a merchant |

### Request

```
GET /api/v1/merchants/12345/revenue-breakdown?start_date=2025-12-15&end_date=2026-03-15&granularity=daily
```

Query params:
- `start_date` (required): ISO date
- `end_date` (required): ISO date
- `granularity` (optional): `daily` (default) or `weekly`

### Response

```json
{
  "merchant_id": 12345,
  "start_date": "2025-12-15",
  "end_date": "2026-03-15",
  "granularity": "daily",
  "data": [
    {
      "date": "2025-12-15",
      "organic_revenue": 1250.00,
      "marketing_revenue": 450.00,
      "total_revenue": 1700.00,
      "campaigns": [
        {"campaign_id": 101, "campaign_name": "Holiday Sale", "revenue": 300.00},
        {"campaign_id": 102, "campaign_name": "New Customer Promo", "revenue": 150.00}
      ]
    }
  ]
}
```

## Data Model Changes

### New materialized view: `mv_merchant_revenue_daily`

| Column | Type | Description |
|--------|------|-------------|
| merchant_id | BIGINT | Merchant identifier |
| date | DATE | Day (store local time) |
| organic_revenue | DECIMAL(12,2) | Revenue from non-marketing orders |
| marketing_revenue | DECIMAL(12,2) | Revenue from marketing-attributed orders (deduplicated) |
| campaign_id | BIGINT | Campaign that drove the marketing revenue (nullable) |
| campaign_name | VARCHAR | Campaign name at time of order |

Refresh: nightly at 02:00 UTC via Airflow DAG `refresh_merchant_revenue_mv`.

## Dependencies

- `campaign_attribution` table — owned by the marketing platform team. Must be populated before the MV refresh runs.
- Airflow scheduler — needs a new DAG slot.

## Rollout Plan

1. Deploy MV migration and backfill (90 days of historical data)
2. Deploy endpoint behind feature flag `revenue_breakdown_v1`
3. Enable for 5% of merchants (internal test accounts first)
4. Monitor query latency, data accuracy, and error rates
5. Ramp to 100% over 2 weeks

## Open Questions

| Question | Answer | Date |
|----------|--------|------|
| Can we reuse the existing Airflow DAG for the MV refresh? | No — need a new DAG due to different dependencies | 2026-03-10 |
| How do we handle the 6% of orders without campaign attribution? | Show as "Other marketing" — see truths.md | 2026-03-10 |
