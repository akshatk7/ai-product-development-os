# Merchant Dashboard Redesign — Designs

> This is a fictional example demonstrating how designs.md captures UI flows in text alongside Figma links.

## Figma

- [Merchant Dashboard Redesign](https://figma.com/design/[YOUR_FILE_KEY]/Merchant-Dashboard-Redesign?node-id=1-2)

## Key Screens / Flows

### Flow 1: Revenue Breakdown View (Default)

1. Merchant opens the Analytics tab in the portal
2. Dashboard loads with 90-day stacked bar chart (organic = blue, marketing = orange)
3. Time range selector shows: 7d / 30d / **90d** (active) / Custom
4. Hover on any bar shows tooltip: date, organic revenue, marketing revenue, total
5. Below the chart: summary row with totals for the selected period

### Flow 2: Campaign Drill-Down

1. Merchant clicks on the "marketing" portion of a bar
2. Panel slides in from the right showing campaign breakdown for that day
3. Each campaign listed with: name, revenue attributed, number of orders
4. "View Campaign" link navigates to the campaign detail page

### Flow 3: Mobile View

1. Same layout as desktop but chart uses weekly buckets (13 bars for 90d) instead of daily
2. Drill-down panel opens full-screen on mobile instead of side panel
3. Time range selector is a dropdown instead of tabs

## Design Notes

- Uses existing card component pattern from the design system
- Chart colors: organic = `#4A90D9` (blue), marketing = `#F5A623` (orange)
- Chart library: Recharts (already used in merchant portal)
- No new components needed — this is a composition of existing patterns

## Edge Cases

- **Merchant with zero marketing spend:** Empty orange section. Show a CTA: "Start a campaign to see marketing-driven revenue here."
- **Merchant with no orders in selected range:** Empty state with "No data for this time period" message.
- **Single-day range selected:** Show a single bar with breakdown, not a chart. Switch to table view.
- **Legacy orders without attribution:** Grouped as "Other marketing" in the breakdown with an info tooltip explaining why.
