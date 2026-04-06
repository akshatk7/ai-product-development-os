---
paths:
  - "projects/**"
---
# How to Use Project Context

When a user asks about a project:

1. Read that project's folder to understand the full context
2. Use the **product brief / CONTEXT.md** to answer "what" and "why" questions
3. Use **updates.md** to answer "what happened recently" and "what's the latest" questions
4. Use the **RFCs** to answer "how" questions about implementation
5. Use **designs.md** to answer questions about UI flows and screens
6. Use **decisions.md** to answer "why was it done this way" questions
7. Use **truths.md** to answer questions about data tables, systems, and architecture
8. Use **scratchpad.md** for exploratory work and artifacts that informed decisions

When a user asks a cross-project question, look across multiple project folders.

When updating files written by teammates, always **append or merge** — never overwrite.

---

# Cross-Project Knowledge Retrieval

When the user is **creating new work** — a new project, product brief, RFC, or design spec — scan past projects for relevant context **before writing**. This applies when:
- Creating a new project folder
- Writing or reviewing a product brief
- Drafting or reviewing an RFC
- Starting a design spec

## How to scan

1. Read `projects/INDEX.md` to identify which past projects touched the same systems or domains.
2. For each related project, read its `truths.md`, `decisions.md`, and `CONTEXT.md`.
3. Extract what's relevant:
   - **System constraints** (data quirks, pipeline latency, API limitations)
   - **Precedent decisions** (patterns to follow or consciously break)
   - **Dependencies** (blocked by or blocking another project?)
   - **Shared infrastructure** (same tables, caches, services?)
4. Present a brief **context summary** before the user starts writing. Top 3-5 findings only.

## When to flag proactively

Even outside explicit "new project" requests, flag cross-project knowledge when:
- A discussion in one project contradicts a decision in another
- A truth discovered in one project affects another active project
- A project's timeline shift creates a dependency risk for another
