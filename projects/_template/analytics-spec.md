# [Project Name] — Analytics Spec

Define all analytics events before development starts. This is the contract between product and engineering for instrumentation.

---

## Events

| Event Name | Trigger | Properties | Owner |
|-----------|---------|------------|-------|
| [event_name] | [When this event fires] | [Key-value pairs sent with the event] | [Eng owner] |

## Dashboards

| Dashboard | Tool | Link | What It Shows |
|-----------|------|------|---------------|
| [Name] | [Sigma / Looker / Amplitude / etc.] | [Link] | [Description] |

## Alerts

| Alert | Condition | Who Gets Paged |
|-------|-----------|----------------|
| [Name] | [Threshold or condition] | [Team / person] |

## Success Metrics (Post-Launch)

How to query the key metrics after launch:

| Metric | Table / Source | Query Logic |
|--------|---------------|-------------|
| [Metric] | [Table name] | [Brief description of how to compute] |
