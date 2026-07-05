---
description: Standalone Spec-by-Example workshop on one Skald requirement — rules, Gherkin examples, open questions, MoSCoW, split check (speckit-skald-sync preset command).
---

# Requirements Workshop (`/speckit-requirements-workshop`)

Mature ONE requirement outside any feature run (in-feature requirement work belongs to the
`/speckit-specify` and `/speckit-clarify` syncs). Methodology: follow the
`skald-requirements` skill — Specification by Example; user-story or EARS title shapes;
Gherkin scenarios without backgrounds; no solutions in requirements; only the PM decides
which open questions get recorded.

## Flow

1. **Pick the requirement**: `mcp__skald__listRequirements` / `getExistingRequirements`; or
   create it (`createRequirements`, status `Draft`, title in user-story or EARS shape).
2. **Rules**: elicit the agreed behaviours — short, testable statements. `createRules` /
   `updateRule`.
3. **Examples**: for each rule worth illustrating, concrete Gherkin scenarios
   (Given/When/Then, no backgrounds, one behaviour each). `createExamples` / `updateExample`.
4. **Open questions**: things the workshop can't settle — propose recording them; the PM
   decides what's worth keeping. `createOpenQuestions`; resolve stale ones with
   `answerOpenQuestion` / `dismissOpenQuestion` as the discussion lands.
5. **Priority**: MoSCoW check — propose `updateRequirement` priority where discussion moved
   it.
6. **Too big?** INVEST check; if it fails, `getRequirementForSplit` → propose
   `splitRequirement` along user-visible value.
7. **Wrap**: state the requirement's readiness (Draft with open questions vs ready to
   `Approved` — status flip proposed only if the PM agrees) and, if it's linked to a PBI,
   whether that PBI is now closer to `/speckit-specify`.

## Discipline

Read before write; ONE confirmation per write; no deletes. Skald MCP unavailable → stop.
