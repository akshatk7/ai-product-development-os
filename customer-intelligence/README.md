# Customer Intelligence

> Owner: PM
> Last updated: YYYY-MM-DD

All customer voice, feedback, and behavioral analysis lives here. This folder captures signal from multiple sources — support channels, sales calls, feedback surveys, Slack escalations — and synthesizes it into actionable product insights.

---

## Sources

| Source | Type | Frequency | Skill |
|--------|------|-----------|-------|
| Slack feedback channels | Text (customer-reported issues, internal escalations) | Continuous | `/customer-feedback-scan` |
| Call transcripts | Audio/text (sales calls, support calls, renewal calls) | Weekly scan | `/call-transcript-scan` |
| Ad-hoc deep dives | Manual analysis of specific customer behavior | On demand | — |

---

## Folder Structure

```
customer-intelligence/
├── README.md                     # This file — sources, conventions, skill triggers
├── diagnosis-workflow.md         # Step-by-step playbook for diagnosing a customer issue
├── slack-scans/                  # Dated Slack feedback scan outputs
│   └── YYYY-MM-DD-scan.md       # One file per scan run
├── call-scans/                   # Dated call transcript analysis outputs
│   └── YYYY-MM-DD-topic.md      # One file per scan run
└── deep-dives/                   # Ad-hoc customer-specific analyses
    ├── README.md                 # Index of all deep dives
    └── YYYY-MM-DD-customer.md   # One file per investigation
```

---

## How to Use

**Periodic scans:** Run `/customer-feedback-scan` to sweep Slack channels for product signal. Run `/call-transcript-scan` on call recordings for qualitative patterns.

**Ad-hoc investigations:** When a specific customer issue comes up, use `diagnosis-workflow.md` as the playbook.

**Signal → Strategy:** Patterns from scans should flow into `strategy/beliefs.md` (confirming or challenging hypotheses) and `strategy/open-questions.md` (surfacing new uncertainties).

**Signal → Projects:** When feedback directly informs an active project, cross-reference it in the project's `CONTEXT.md` or `updates.md`.

---

## Slack Channels to Scan

_Register channels here with their IDs and what signal they carry._

| Channel | ID | Signal Type |
|---------|----|-------------|
| _#product-feedback_ | _C0000000000_ | _Customer-reported bugs, feature requests_ |
| _#support-escalations_ | _C0000000000_ | _Escalated support tickets_ |
| _#sales-intel_ | _C0000000000_ | _Sales team observations from prospect/customer calls_ |
