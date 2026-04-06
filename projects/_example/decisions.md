# Merchant Dashboard Redesign — Decisions

> This is a fictional example demonstrating the decision log pattern, including the supersession pattern.

Track key decisions made during the project. When a newer decision supersedes an older one, strike through only the title using `~~title~~`, update the body to reflect the current state, and add: "**Superseded by:** [date + title of newer decision]"

## Decision Log

### ~~[2026-02-15] Use pie chart for revenue breakdown~~

**Superseded by:** [2026-02-22] Use stacked bar chart for revenue breakdown

- **Context**: Needed a visualization for organic vs marketing revenue split.
- **Decision**: Originally chose pie chart for simplicity.
- **Alternatives considered**: Stacked bar chart, table view, donut chart.
- **Impact**: Design started mockups with pie chart.

### [2026-02-22] Use stacked bar chart for revenue breakdown

- **Context**: Design review feedback: pie charts make it hard to compare values across time periods. A merchant can't tell if marketing revenue grew from last month — they can only see the current split.
- **Decision**: Switch to stacked bar chart with time on the x-axis. Each bar shows organic (bottom) and marketing-driven (top) revenue.
- **Alternatives considered**: Keeping pie chart with a separate trend line — rejected as too cluttered. Area chart — rejected because it implies continuous data when we have daily buckets.
- **Impact**: Design rework (1 day). Backend API response format unchanged — frontend handles the visualization.

### [2026-03-05] Default to 90-day view, not 30-day

- **Context**: User research showed merchants think about marketing ROI in quarterly terms, not monthly. A 30-day view often doesn't capture the full cycle of a campaign launch → ramp → steady state.
- **Decision**: Default time range is 90 days. Options for 30d, 7d, and custom range are available via selector.
- **Alternatives considered**: 30-day default with a "see more" link to 90 days — rejected because the default shapes merchant perception. We want them thinking in terms of campaign lifecycle, not recent activity.
- **Impact**: Backend needs to support 90-day aggregation efficiently. Led to the materialized view decision (see truths.md).
