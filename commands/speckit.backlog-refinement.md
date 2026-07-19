---
description: Refine the Skald backlog outside a delivery — create, split, size, and gap-check PBIs until one is ready for /speckit-specify (speckit-skald-sync preset command).
---

# Backlog Refinement (`/speckit-backlog-refinement`)

Project-level, before delivery: the pre-flow work that produces a PBI ready for
`/speckit-specify`. Methodology: follow the `skald-planning` skill — PBI types
(Feature/Bug/Refactor/Spike) with per-type title shapes, S/M/L/XL relative sizing
calibrated for AI-assisted delivery, the user-perspective splitting heuristic, and the
PM-only prioritisation rule.

## Flow

1. **Read first**: `mcp__skald__listPbis` (the ordered backlog), and for items under
   discussion `getBacklogItemDetail` + the linked requirements' rules/examples/questions
   (`listRules`, `listExamples`, `listOpenQuestions`).
2. **Create**: new PBIs via `createBacklogItems` — right type, title shape per type, status
   `Refining Requirements`.
3. **Refine an item**: read its linked requirements; surface gaps (no linked requirement, a
   requirement with no rules/examples — recommend `/speckit-requirements-workshop` for
   maturing it); propose an S/M/L/XL estimate via `updateBacklogItem`.
4. **Split**: when an item is too big (estimate XL, or its requirement fails INVEST), use
   `getRequirementForSplit` / `splitRequirement` for the requirement side and propose
   follow-up PBIs for the remainder — split along user-visible value, never technical layers.
5. **Housekeeping**: propose archiving superseded items (`archiveBacklogItem`) one at a
   time, each with explicit confirmation.
6. **Hand off**: end by naming what's ready — "PB-x is refined; start delivery with
   `/speckit-specify`".

## Discipline

**The agent proposes; the PM prioritises.** This command NEVER reorders the backlog on its
own. Read before write; ONE confirmation per write; no deletes (archive only, explicit).
Skald MCP unavailable → stop.
