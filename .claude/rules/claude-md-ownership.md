---
paths: ["CLAUDE.md", "**/CLAUDE.md"]
---

# CLAUDE.md Ownership

CLAUDE.md is **orientation only** -- max 150 lines, PM-owned. It answers: what is this repo, where do I find things, what are the key workflows. That's it.

The pre-commit hook enforces the 150-line limit. If CLAUDE.md is approaching the cap, move content to the appropriate location below.

## Content Routing Table

| Content Type | Belongs In | NOT in CLAUDE.md |
|---|---|---|
| Document conventions (file boundaries, status freshness, cross-refs) | `.claude/rules/document-conventions.md` | |
| Meeting routing and digest conventions | `.claude/rules/meetings.md` | |
| DS analysis conventions | `.claude/rules/data-science.md` | |
| Project file conventions (CONTEXT.md, decisions.md, etc.) | `.claude/rules/project-context.md` | |
| Eng setup, codebase guide, prototyping | `engineering/CLAUDE.md` (engineering add-on) | |
| Skill definitions and instructions | `.claude/skills/<name>/SKILL.md` | |
| Strategy guidance (beliefs schema, open questions process) | `strategy/CLAUDE.md` | |
| Model selection guidance | Do not include anywhere -- always use the best available model at max effort | |

## When CLAUDE.md Grows

If you need to add content to CLAUDE.md and it would exceed 150 lines:

1. Identify which category the content belongs to (see table above).
2. Move or create it in the appropriate file.
3. If needed, add a one-line pointer in CLAUDE.md to the new location.
4. CLAUDE.md should never contain the full rules -- only pointers to where rules live.
