Generate an internal launch email from scratch to announce a shipped product.

The user will provide product context as $ARGUMENTS or in a follow-up message.

---

## Best Practices Reference

These guidelines are distilled from analyzing high-performing internal launch emails.

### Subject Line

Format: `[Product Launch] Product Name: One-line impact statement`

Examples:
- `[Product Launch] Smart Substitutions: Reducing missing items by 23%`
- `[Product Launch] Arrival ETA: 12% improvement in on-time deliveries`

Keep it under 80 characters. Lead with the product name, follow with the single most impressive metric or outcome.

### Recommended Structure

| Section | Required? | Guidance |
|---------|-----------|----------|
| TL;DR | Yes | 3-4 crisp bullets: problem, solution, top-line metric. Under 100 words total. |
| Problem / Why This Matters | Yes | Business context + data. Frame as customer pain, business risk, or strategic gap. |
| Solution / What We Built | Yes | Feature descriptions with bulleted capabilities. Keep it concrete — what does the user see/do? |
| Visuals | Yes | 3-7 captioned screenshots, before/after comparisons, GIFs, or Loom links. |
| Results / Impact | Yes | Quantified metrics. Dollarize and annualize wherever possible. Use tables for comparisons. |
| What's Next | Yes | 3-5 bullets organized by timeframe (this quarter, next half, long-term). |
| Working Team | Yes | Organized by function (PM, Eng, Design, DS, Ops). Use a "Huge thanks to..." framing. |
| Reference Docs | Optional | Links to Brief, PRD, Figma, Ship Review deck. |
| Contact | Optional | Slack channel or alias for questions. |

### Tone & Style

- **First person plural**: "We shipped..." / "Our team built..." — not "I" or passive voice.
- **Direct and confident**: State outcomes plainly. Avoid hedging ("we believe this might help...").
- **Internal-audience jargon is fine**: No need to over-explain domain-specific terms your audience knows.
- **Celebratory but grounded**: Highlight wins with real numbers. Avoid hyperbole without evidence.

### Length

Aim for **700-900 words** (excluding team list and reference links). Shorter emails get read; longer emails get skimmed. If you're over 1,000 words, cut or move detail to linked docs.

### Metrics Best Practices

- **Dollarize**: Convert percentage improvements into dollar impact. "3% reduction in refunds" is good; "3% reduction in refunds, saving ~$4.2M annualized" is better.
- **Annualize**: Present metrics on an annual basis for bigger-picture impact.
- **Use tables**: When comparing before/after, experiment vs. control, or multiple metrics.
- **Include stat sig and experiment details**: Mention experiment duration, population size, and statistical significance when available.
- **Lead with the biggest number**: Open the results section with your most impressive metric.

### Visuals Guidance

- Place visuals in the **Solution** section to show what was built, and in the **Results** section for dashboards or charts.
- Every visual needs a **caption** explaining what the reader is looking at.
- **Before/after** comparisons are highly effective for UI changes.
- **GIFs or Loom videos** work well for interactive flows.
- Aim for 3-7 visuals total.
- Describe each visual with a placeholder like `[Screenshot: description of what to capture]`.

### Common Mistakes

- **No TL;DR**: Readers skim. If the first thing they see is paragraphs of background, they'll bounce.
- **Metrics without context**: "15% improvement" means nothing without a baseline or dollar amount.
- **Wall of text, no visuals**: Screenshots make the product real. Always include them.
- **Burying the impact**: Lead with results if they're strong. Don't make the reader scroll past three sections of background.
- **Forgetting the team**: Recognizing contributors is a core part of launch emails.
- **Scope creep in What's Next**: Keep it to 3-5 bullets. This is a teaser, not a roadmap doc.
- **Passive or vague language**: "Improvements were made to the checkout experience" — by whom? What improvements? Be specific.

---

## Instructions

### Step 1: Gather Context

Ask the user for the following (skip any they've already provided):

- **Product name**: What's the feature/product called?
- **Problem**: What problem does this solve? Any data on the pain point?
- **Solution**: What did you build? Key capabilities?
- **Results**: What metrics moved? Experiment results? Dollar impact?
- **Visuals**: Do you have screenshots, GIFs, or Loom links to include?
- **What's next**: What's on the roadmap after this launch?
- **Team**: Who worked on this? (PM, Eng, Design, DS, Ops, etc.)
- **Reference docs**: Links to PRD, Figma, Ship Review, etc.?

If the user provides partial context, generate the email with what you have and use `[TODO: ...]` placeholders for missing information.

### Step 2: Generate the Email

Write the complete email following the structure and best practices above. Include:

1. A subject line in the correct format
2. All required sections in order
3. Visual placeholders with descriptions of what to screenshot
4. Properly formatted metrics (dollarized, annualized, tabled where appropriate)
5. A team section organized by function

### Step 3: Present and Iterate

After generating, ask the user:
- Does the framing of the problem resonate?
- Are the metrics presented correctly?
- Anyone missing from the team list?
- Any sections you want to expand or cut?
