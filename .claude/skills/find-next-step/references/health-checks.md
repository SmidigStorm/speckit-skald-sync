# Skald product health checks

The detailed per-layer inspection the health check runs. For each layer: what to
read, what "healthy" looks like, the findings to watch for, and which
doing-skill fixes it. The health check is read-only — these are diagnoses, not
fixes.

## Layer 1 — Strategy (Vision + Users)

**Read**: `getExistingProjectVision`, `listProjectUsers`.

Healthy:
- A Vision exists and reads like an elevator pitch — Problem, Vision,
  Differentiation (or a coherent shape the PM chose).
- At least the core user types exist, most with goals and pains, not
  just bare names.

Findings → **skald-strategy**:
- No Vision at all → recommend writing one first (a bare product's
  natural starting point).
- Vision is a vague mission statement ("empower world-class teams") with
  no stated Problem → recommend sharpening.
- Zero Users, or users that are just names with no goals/pains → the
  product doesn't know who it's for.
- A User named "User" → too generic to be useful.

## Layer 2 — Domains + Glossary

**Read**: `listDomains`, `listDomainTerms`.

Healthy:
- A coherent domain tree (≤3 levels), neither empty nor a flat dump of
  40 unrelated nodes.
- A glossary that exists — it can be **minimal**, but not zero. Terms
  are typed (Entity / Process / Term), not all defaulted to "Term".

Findings → **skald-domain-management**:
- No domains → nothing to hang requirements or terms on.
- One flat list of many top-level domains that clearly want grouping, or
  a tree straining against the 3-level cap → restructure (UI for
  reparenting; the health check flags it).
- Zero glossary terms → no ubiquitous language; recommend capturing the
  core nouns/processes even if minimal.
- Everything typed "Term" → classification was skipped; the glossary
  isn't modelling Entities vs Processes.

Note: a *minimal* glossary is acceptable. Don't flag "only 6 terms" as a
problem if those 6 are the right core terms. Flag *zero*, or
mis-typed/inconsistent.

## Layer 3 — Requirements (+ rules, examples, open questions)

**Read**: `listRequirements`; then `listRules`, `listExamples`,
`listOpenQuestions` on each (or a representative sample if there are
many).

Healthy:
- Requirements have rules (the acceptance bar).
- Rules have at least one example each (Specification by Example).
- Titles are User Story or EARS shaped, solution-free.
- Open questions are tracked, not lost.

Findings → **skald-requirements**:
- A requirement with **no rules** → not testable. Common, high-value
  finding.
- A rule with **no example** → not grounded. The single most common real
  gap in most products. Recommend the SbE workshop.
- Solution-leaning titles ("Add a dropdown…") → coach toward
  capability framing.
- A requirement that looks oversized (many rules, several of which could
  stand alone) → recommend splitting (INVEST).
- Unresolved open questions blocking a requirement → surface them.

## Layer 4 — Planning (PBIs)

**Read**: `listPbis`, `getExistingBacklogItems`, `getTeams`.

Healthy:
- Feature PBIs link to requirements; Bugs/Spikes need not.
- PBIs in "Ready for Planning" have an estimate.
- No PBI is sitting XL without a split.
- Backlog has a sensible shape (not 200 unrefined items).

Findings → **skald-planning**:
- A Feature with **no linked requirements** → either it's unrefined or the
  requirement is missing.
- A PBI in Ready for Planning with **no estimate** → not actually ready.
- A PBI **In Progress while all its linked requirements are still Draft**
  → the statuses aren't telling the truth (statuses are gate verdicts —
  see skald-sdd); either the requirements were refined and never
  approved, or the build started without a settled spec.
- An **XL** PBI → recommend splitting along user-perspective increments.
- A large pile in "Refining Requirements" that never advances → backlog
  refinement needed.

## Layer 5 — Goals (OKRs)

**Read**: `getTimePeriods`, `listGoals`.

Healthy:
- A current Time Period exists with a small number of Objectives.
- Objectives are qualitative (no baked-in metric).
- Key Results are outcomes, measurable, ≤5 per Objective.
- Recent check-ins exist (goals are alive, not set-and-forgotten).

Findings → **skald-goals**:
- No Time Period / no OKRs → recommend setting goals if the PM wants
  outcome tracking (not every product needs OKRs — ask).
- An Objective with a number in it → that number is a KR.
- A Key Result that's an **output** ("ship the feature", "run 5
  interviews") → recommend reframing to the outcome.
- More than 5 KRs on an Objective → focus is diluted.
- KRs with no check-ins for a long stretch → goals have gone stale.

## Layer 6 — Research (only when the project has any)

**Read**: `listInsights`, `listInsights({ staleOnly: true })`,
`listRecommendations({ unactionedOnly: true })`.

**Silence is the correct output for a project with no research.** If
`listInsights` and `listRecommendations` both come back empty, this layer
produces no findings, no section and no "0 insights" line — and it never
recommends starting to do UX research. A product that has never recorded
research is not thereby unhealthy.

Healthy (when research exists):
- Claims have been re-read recently — `stale` is false. Staleness is
  derived on every read (nobody has re-read the claim in 90 days), never
  stored.
- Recommendations have produced work — `actioned` is true, meaning the
  recommendation points at a requirement or a backlog item.
- Contested claims exist and are fine. A claim with refuting evidence is
  a signal to go and look, **not a finding to clear**.

Findings → **skald-research**:
- **Claims overdue for review** (`staleOnly: true`) → name them by
  `IN-n`, most overdue first. The fix is for a human to re-read the claim
  and its evidence, then `markInsightReviewed`. **Never suggest marking
  several reviewed at once** — a claim marked reviewed that nobody read
  is worse than one openly overdue, because it looks checked.
- **Un-actioned recommendations** (`unactionedOnly: true`) → research was
  done and then nothing happened. Either turn it into work
  (`createWorkFromRecommendation` → **requirements** / **planning**) or
  archive it deliberately.
- **Requirements with no research backing** are *not* a finding. Most
  requirements come from somewhere other than a study, and manufacturing
  that finding would push people to attach evidence that doesn't exist.

## Cross-layer findings

Some of the most useful findings span layers:

- **A refined, sized, linked PBI sitting in Ready for Planning** → it's
  ready to build. Route to **Delivery** (the PM's dev tooling via the
  speckit-skald-sync Spec Kit preset).
- **A Key Result with no linked PBIs** → the outcome has no work behind
  it; either link existing PBIs or the work doesn't exist yet.
- **Requirements that no PBI delivers** → specced but never planned.
- **A populated Vision but empty everything else** → the product has
  direction but no substance yet; recommend Domains + first
  Requirements.

## Tools (read-only)

The union of read tools the health check uses. None of these write.

| Tool | Tells you |
|------|-----------|
| `listProjects` | which products exist (pick one to assess) |
| `getExistingProjectVision` | is there a Vision, and what it says |
| `listProjectUsers` | the user types, with goals/pains |
| `listDomains` | the domain tree (parentId, depth) |
| `listDomainTerms` | glossary terms, their types and homes |
| `listRequirements` | all requirements (type, priority, status, domain) |
| `listRules` | rules under a requirement |
| `listExamples` | examples under a rule |
| `listOpenQuestions` | open questions under a requirement |
| `listPbis` / `getExistingBacklogItems` | the backlog (status, estimate, links) |
| `getTeams` | teams and their workload |
| `getTimePeriods` | OKR time periods |
| `listGoals` / `getExistingGoals` | objectives + key results |
| `getCheckInHistory` | progress trail on a KR (staleness) |
| `getDomainImpact` | blast radius of a domain (for restructure findings) |
| `listInsights` | research claims, with derived `contested` / `stale`; `staleOnly: true` narrows to the overdue ones |
| `listRecommendations` | research recommendations with derived `actioned`; `unactionedOnly: true` narrows to those that produced no work |

If the scan is large, sample rather than reading every rule/example
individually — read enough to judge the pattern, and say in the output
that you sampled.
