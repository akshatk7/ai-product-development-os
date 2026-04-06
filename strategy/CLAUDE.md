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

## Integration — Proactive, Not Passive

The strategy layer must be read and updated in every workflow, not just when asked.

**Every session:** Read `strategy/beliefs.md` at session start alongside project context. When working on any project, check whether new information confirms, challenges, or invalidates existing beliefs.

**Every meeting digestion:** After extracting decisions and updates, ask: "Does anything from this meeting update a belief, add competitive context, or resolve an open question?" If yes, update `strategy/` files as part of the same commit.

**Every morning sync:** Include a "Strategic Signals" section (1-3 lines max) noting which beliefs were strengthened or challenged by the day's data.

**Every brief/PRD/RFC:** Before writing, read relevant beliefs and open questions. The strategy layer should inform how features are framed, not just what gets built.

**Proactive synthesis:** When you notice a pattern across projects — experiment results pointing in the same direction, multiple meetings surfacing the same concern, data from different sources converging — surface it by updating `beliefs.md` or adding to `open-questions.md`. Don't wait to be asked.
