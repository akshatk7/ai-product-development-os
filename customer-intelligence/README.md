# Customer Intelligence

> Owner: PM

This folder is the home for **voice-of-customer artifacts** — anything that captures what your customers (or users, merchants, partners, members — whatever your product calls them) are saying, thinking, or doing.

What goes here is open-ended on purpose. Different teams collect customer signal from different places: support tickets, NPS surveys, sales calls, in-product feedback, social mentions, churn interviews, app store reviews, Slack escalations, user research sessions. The pattern is the same: capture the signal, look for themes, route insights into projects and strategy.

---

## Suggested folder structure

Create subfolders only when there's content for them. A new repo starts with just this README.

```
customer-intelligence/
├── README.md                 # This file
├── deep-dives/               # Ad-hoc investigations of specific customers, segments, issues
│   └── YYYY-MM-DD-topic.md
└── scans/                    # Recurring sweeps of a feedback source
    └── YYYY-MM-DD-source.md
```

If you have a structured diagnosis playbook for individual customer investigations, add a `diagnosis-workflow.md` documenting your data sources, dimensions, and output format.

---

## How signal flows out of this folder

Customer signal is most valuable when it lands somewhere actionable:

- **Confirms or challenges a belief** → update `strategy/beliefs.md` (add evidence to the relevant hypothesis, or move it toward Confirmed/Disproven).
- **Surfaces a new uncertainty** → add to `strategy/open-questions.md`.
- **Maps directly to an active project** → cross-reference in that project's `CONTEXT.md` or `updates.md`.
- **Reveals a system constraint or unexpected behavior** → land it in the relevant project's `truths.md`.

The same finding can land in multiple places. That's expected.

---

## Optional: automated scans

If your team collects feedback in chat tools (Slack, Teams, Discord) or records customer/sales calls, two opt-in skills can sweep those sources on a cadence and write dated scan files into this folder:

- **`add-ons/skills/customer-feedback-scan`** — sweep registered chat channels for product signal, classify by theme/severity.
- **`add-ons/skills/call-transcript-scan`** — speaker-aware analysis of recorded calls with thematic signal detection.

Move either skill into `.claude/skills/` to enable it. Both are tool-pluggable — the channel registry and transcript source are configured per-team.

---

## Bare-bones is fine

Many teams start with just one or two deep-dives and grow this folder organically as patterns emerge. Don't pre-build infrastructure you don't need. The folder exists so customer signal has a home — what shape that takes is up to you.
