---
description: Read-only health scan of the whole Skald product — what's missing or weak, and 1–5 routed next steps (speckit-skald-sync preset command).
---

# Product Health (`/speckit-product-health`)

The "what should I do next?" command. Scans every Skald layer read-only, forms an
opinionated view, and recommends concrete next steps — each routed to the command that does
the work. **This command never writes.**

## Scan (all read-only)

1. **Strategy**: `getExistingProjectVision` (exists? covers problem/vision/differentiation?),
   `listProjectUsers` (any user groups? with goals/pains?).
2. **Domain**: `listDomains` (tree depth/coverage), `listDomainTerms` (glossary density,
   untyped or undefined terms).
3. **Requirements**: `listRequirements` + spot-check `listRules` / `listExamples` /
   `listOpenQuestions` — Draft-heavy? rules without examples? long-unanswered questions?
4. **Backlog**: `listPbis` — items with no linked requirement, no estimate, no release;
   stale `In Progress` items.
5. **Goals**: `getTimePeriods` + `listGoals` + `getCheckInHistory` — active period? KRs
   without recent check-ins? output-shaped KRs?
6. **Releases**: `listReleases` — overdue targets, releases with everything Done but not
   `Released`.

## Report

- A short health summary per layer (strong / weak / missing — one line each).
- **1–5 concrete next steps**, most valuable first, each routed:
  `/speckit-specify-vision`, `/speckit-domain-knowledge`, `/speckit-requirements-workshop`,
  `/speckit-backlog-refinement`, `/speckit-goals`, `/speckit-release-planning`, or — when a
  PBI is genuinely refined and ready — "start delivery: `/speckit-specify` on PB-x".
- On an empty/new project, recommend the bootstrap sequence:
  `/speckit-specify-vision` → `/speckit-domain-knowledge` → `/speckit-backlog-refinement`.

Diagnose and hand off — do not start fixing anything inside this command. Skald MCP
unavailable → stop.
