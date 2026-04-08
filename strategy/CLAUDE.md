# Strategy Layer

The `strategy/` folder is the thinking layer of the repo. It captures what the team believes (and why), what the competitive landscape looks like, and which strategic uncertainties need resolution.

---

## Files

| File | Purpose |
|------|---------|
| `beliefs.md` | Hypotheses about customers, product, and market — with confidence levels, evidence, and challenge dates |
| `competitive.md` | What competitors and analogous products are doing — observations with sources and relevance |
| `open-questions.md` | Strategic uncertainties that frame the work — what we'd need to resolve them |

---

## Conventions

### Beliefs (`beliefs.md`)

Each belief follows this schema:

```
### B[N]: [Belief statement]
- Confidence: High/Medium/Low
- Evidence for: [specific data, experiments, observations]
- Evidence against: [counter-evidence, if any]
- Implications: [how this shapes what we build]
- Last challenged: [date]
```

**Lifecycle:** Beliefs follow a lifecycle: **Active** (being tested) → **Confirmed** (mark `[Confirmed]` with evidence pointer) → or **Disproven/Superseded** (move to `## Retired Beliefs` section with date, evidence, and what replaced it). See `.claude/rules/document-conventions.md` for full rules. The history of wrong beliefs is as valuable as current ones.

**Sections:** Organize by theme (e.g., "Customer Behavior", "Product & Growth", "Experiment Insights", "Market & Competitive").

### Competitive Intelligence (`competitive.md`)

Organized by domain. Each domain has a table:

```
| Observation | Source | Date | Relevance |
```

End with a "Gaps — Need to Research" section of open research questions.

### Open Questions (`open-questions.md`)

Each question follows this schema:

```
### Q[N]: [Question]
[Context paragraph — why it matters, what's at stake]
**What would resolve this:** [Specific data/event/experiment]
```

**Lifecycle:** When resolved, move to a "Resolved" section with a pointer to the answer (e.g., "Resolved by experiment results in `projects/your-project/truths.md`").

---

## When to Consult This Layer

**Pull context as needed — don't front-load everything.** Read the beliefs, competitive observations, and open questions that are relevant to the specific task at hand.

### Morning Sync
If a new finding clearly strengthens or challenges a belief, note it briefly (e.g., "Strengthens B3" or "Challenges B7 — [why]"). Don't force a strategic signal check on every update — only flag when something genuinely moves the needle.

### Meeting Digestion
Extract strategic signals only when they're present — new evidence for/against beliefs, competitive observations mentioned in passing, or questions nobody answered. Not every meeting has strategic content.

### Brainstorming & New Work
Pull relevant beliefs and open questions as starting context. This is where the strategy layer adds the most value — connecting past evidence to new decisions.

### Proactive Synthesis
When you notice a pattern across projects — experiment results pointing in the same direction, multiple meetings surfacing the same concern, data from different sources converging — surface it by updating `beliefs.md` or adding to `open-questions.md`. Don't wait to be asked.
