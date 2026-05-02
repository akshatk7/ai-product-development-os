# Quickstart Guide

Get your team's knowledge base running in 5 steps. Each step is independent — you don't need a perfect setup to start getting value.

---

## Step 1: Clone the repo as your team's KB

```bash
git clone https://github.com/akshatk7/ai-product-development-os.git my-team-knowledge-base
cd my-team-knowledge-base
rm -rf .git && git init
```

Then push to your own GitHub repo:

```bash
git remote add origin <your-team-repo-url>
git add -A && git commit -m "Initialize from AI Product Development OS"
git push -u origin main
```

## Step 2: Fill in your team

Edit `team/people.md` with your org chart — who's on the team, their roles, who they report to. This is the file the OS reads to understand who does what.

`team/rituals.md` and `team/channels.md` can stay as-is for now; come back to them when you have time.

## Step 3: Describe the product in one paragraph

Edit `product/overview.md` — a short paragraph (or page) that explains what your product is and how it works. This is the document agents and new team members read first to get oriented.

That's the minimum. `product/terminology.md` and `product/metrics.md` are optional starting out — populate them as terms and metrics come up.

## Step 4: Seed strategy with 1–2 beliefs

In `strategy/beliefs.md`, write 1 or 2 hypotheses your team currently holds about customers, the product, or the market. Use the schema in the file: confidence, evidence for, evidence against, implications, last challenged.

These don't need to be proven. The strategy layer compounds — every meeting digestion, brief, and analysis adds to it.

## Step 5: Create your first project

```bash
cp -r projects/_template projects/your-first-project
```

Fill in the project's `README.md` (phase, team, key links). Add a `CONTEXT.md` with a one-paragraph problem statement and a link to your source brief if you have one. See `projects/_example/` for what a fleshed-out project looks like.

---

## What you can do now

- `/repo-navigator` — interactive tour of what's where in your KB
- `/brief-starter` — gather context from past projects before writing a new brief
- `/digest-meeting` — turn a meeting transcript into routed project updates and decisions
- `/morning-sync` — daily sweep of overnight activity (requires registering channels in `config/morning-sync.md` first)

Full skill and agent inventory in CLAUDE.md.

---

## Expanding from here

When you're ready for more, see:

- **`CUSTOMIZE.md`** — adapt the OS to your team's tools (Slack vs Teams, Jira vs Linear, Figma vs Sketch, etc.)
- **`add-ons/README.md`** — opt-in patterns for engineering, data science, leadership-forum, and more
- **`config/morning-sync.md`** — register your chat channels for the morning sync skill
- **`roadmap/CLAUDE.md`** — connect your roadmap source (spreadsheet, Linear roadmap, etc.)
- **`customer-intelligence/`** — start collecting customer signal in whatever shape fits your product

The repo gets smarter over time. Every decision logged, every finding recorded, every meeting digested, every belief challenged makes the next task faster.

---

> Built on the [AI Product Development OS](https://github.com/akshatk7/ai-product-development-os) by Akshat Khandelwal.
