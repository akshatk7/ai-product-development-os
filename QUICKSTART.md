# Quickstart Guide

Get your team's knowledge base running in 11 steps.

---

## Step 1: Clone the repo

```bash
git clone [YOUR_REPO_URL]
cd ai-product-development-os
```

## Step 2: Fill in your team

Edit `team/people.md` with your org chart — who's on the team, their roles, and reporting lines.

## Step 3: Register your Slack channels

Edit `config/morning-sync.md` to register your Slack channels with their channel IDs and project mappings. `team/channels.md` will be auto-generated from this config.

## Step 4: Document your meetings

Edit `team/rituals.md` with all recurring meetings — name, cadence, time, attendees, and the doc/tool used.

## Step 5: Describe your product

Write `product/overview.md` — what the product is, how it works, key milestones, current strategy. This is the single document that gives anyone full product context in 5 minutes.

## Step 6: Build your glossary

Edit `product/terminology.md` with every acronym, internal term, and domain concept your team uses. This is the first file new team members and agents should read.

## Step 7: Seed your strategy layer

Fill in the `strategy/` files:
- `strategy/beliefs.md` — Write 3-5 hypotheses the team currently holds about customers, product, or market. Use the schema in the file (confidence level, evidence, implications). These don't need to be proven — they're hypotheses.
- `strategy/competitive.md` — Add 2-3 observations about competitors or analogous products.
- `strategy/open-questions.md` — Write the 2-3 biggest strategic uncertainties facing the team.

The strategy layer gets smarter over time. Start small — every meeting and analysis will add to it.

## Step 8: Create your first project

```bash
cp -r projects/_template projects/your-project-name
```

Fill in `README.md` with the project overview, then add `CONTEXT.md` linking to your source Google Doc. See `projects/_example/` for what a filled-in project looks like.

## Step 9: Set up the morning sync

Edit `config/morning-sync.md` to map your Slack channels to project folders. This powers the `/morning-sync` skill that pulls context from the previous business day.

## Step 10: Register customer feedback channels

Edit `customer-intelligence/README.md` to register your feedback channels (support, sales, product feedback). This powers the `/customer-feedback-scan` skill.

## Step 11: Connect your roadmap

Edit `roadmap/CLAUDE.md` with links to your roadmap spreadsheet and any other tracking tools. Define the update workflow so the agent knows how to propose changes.

---

## You're Done

Your daily workflow now looks like this:

- **Morning:** `/morning-sync` pulls overnight activity from Slack + meetings, proposes repo updates, flags strategic signals
- **During the day:** Decisions, research findings, and design specs get added to project folders as work happens
- **Monday:** "write top of mind for [date]" drafts the weekly meeting doc from all accumulated context
- **Before leadership syncs:** "prep leadership forum for [date]" assembles a strategic briefing
- **After any meeting:** "digest the notes from [meeting]" extracts decisions, updates project files, and checks strategy/beliefs.md
- **Weekly:** "weekly status" pulls a cross-referenced snapshot from all tracking sources
- **Periodically:** "scan for customer feedback" sweeps Slack for product signal

The repo gets smarter over time. Every decision logged, every finding recorded, every meeting digested, every belief challenged makes the next task faster.

---

> Built on the [AI Product Development OS](https://github.com/akshatk7/ai-product-development-os) by Akshat Khandelwal.
