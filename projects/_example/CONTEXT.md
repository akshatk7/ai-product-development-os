# Merchant Dashboard Redesign — Project Context

> This is a fictional example demonstrating how CONTEXT.md works. It's the hub that connects all project files and links to source documents.

**Last updated:** March 15, 2026
**Status:** RFC phase. Brief and designs complete. Backend RFC in review.
**Phase:** RFC

---

## Source Documents

> **Note:** This file is a point-in-time snapshot. The Google Docs linked below are the living sources of truth — they may contain comments, updates, and discussions not reflected here. Always check the Google Doc for the latest.

| Source | Location | Date | Notes |
|--------|----------|------|-------|
| **Product Brief (latest)** | [Google Doc](https://docs.google.com/document/d/[YOUR_DOC_ID]/edit) | Feb 10, 2026 | Living source of truth |
| Design Specs | [Figma](https://figma.com/design/[YOUR_FILE_KEY]/Merchant-Dashboard-Redesign) | Mar 1, 2026 | Eng handoff complete |
| Backend RFC | `rfc-backend.md` | Mar 12, 2026 | In review |
| User Research | [Google Doc](https://docs.google.com/document/d/[YOUR_DOC_ID]/edit) | Jan 20, 2026 | 12 merchant interviews |

---

## Problem

Merchants can't understand their revenue breakdown. The current dashboard shows a single total without distinguishing organic orders from marketing-driven orders. When merchants see high marketing spend, they can't tell if it's working — leading to support calls, campaign cancellations, and churn.

## Solution

Add a revenue breakdown view to the merchant analytics dashboard:
- Stacked bar chart showing organic vs marketing-driven revenue over time
- Drill-down by clicking a segment to see which campaigns contributed
- 90-day default view with options for 30d, 7d, and custom range
- Campaign attribution column showing which campaign drove each marketing order

## Key Decisions

See `decisions.md` for full decision log. Summary:
- Stacked bar chart (not pie chart) for revenue visualization
- 90-day default view (not 30-day)
- Backend materialized view for performance (not real-time aggregation)

## Pending

- [ ] Frontend RFC not started — waiting for backend API to stabilize
- [ ] Load testing for merchants with >1000 orders/day
