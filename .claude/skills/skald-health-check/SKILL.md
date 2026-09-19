---
name: skald-health-check
description: 'Your Skald health check — what to do next in a product. Read-only scan across every layer (Vision, Users, Domains, Glossary, Requirements with rules and examples, Backlog Items with linkage and sizing, Goals/OKRs, UX research), an opinionated view of what is missing or weak, and one to five concrete next steps routed to strategy, domains & glossary, requirements, planning, goals or delivery. Never writes — diagnoses and hands off. Use whenever the user asks what to do next, where to start, what is missing, or how healthy the product is — even if they don''t say "Skald". Also trigger on "what''s next", "where do I start", "what should I work on", "what''s missing", "is this product in good shape", "give me a health check", "I don''t know what to do", or at the start of working in a product when direction is unclear.'
---

# Skald Health Check

> **This is reference for you, not a script to read out.** The user asked a
> question, not for the manual. Answer it in a few points — three to five, the
> ones that matter for what they actually asked — then ask which part they want
> to go into. Never deliver a section-by-section tour of this document. If you
> find yourself writing headings, you are reciting: stop and ask instead.


The health check answers one question: **what should I do next in this product?**

It does that by scanning the whole product's state in Skald, forming an
opinionated view of what's missing or weak, and recommending 1–5 concrete
next steps — each one routed to the skill that does the work.

**The health check is read-only.** It diagnoses and points; it never writes. Every
recommendation ends with a hand-off to the domain where the work happens
(strategy, domain modelling, requirements, planning, goals, delivery),
and the doing-work there carries its own Writing rules.

## How the health check works

1. **Scan** every layer with read tools (full list and what "healthy"
   looks like: [`references/health-checks.md`](references/health-checks.md)).
2. **Judge** the state against best practice — opinionated, not neutral.
   A requirement with no examples is a finding, not just data.
3. **Recommend** 1–5 next steps, most valuable first, each routed to a
   skill.

There is **no canonical order** — it doesn't force "Vision before
anything". It reads the actual state and recommends what would most
improve the product right now. That said, an empty product has a natural
starting point (Vision + Users), and it says so when the product is
bare.

## The scan

Read across all layers (don't write):

- **Strategy** — `getExistingProjectVision`, `listProjectUsers`.
- **Domains + Glossary** — `listDomains`, `listDomainTerms`.
- **Requirements** — `listRequirements`, then `listRules` /
  `listExamples` / `listOpenQuestions` on a sample (or all, if few).
- **Planning** — `listPbis`, `getExistingBacklogItems`, `getTeams`.
- **Goals** — `getTimePeriods`, `listGoals`.
- **Research** (only when the project has any) — `listInsights`, then
  `listInsights({ staleOnly: true })` and
  `listRecommendations({ unactionedOnly: true })`.

**A project with no research contributes nothing to the report** — no
Research section, no "0 insights", no recommendation to start doing UX
research. `listInsights` and `listRecommendations` both returning empty
means this layer is silent.

The same silence is correct when the research tools are **absent from your
catalogue entirely**: UX research is an optional module, so an organization
that has not switched it on gets no research tools at all. Skip the layer
without comment — do not report the module as missing, and never suggest
enabling it. From the report's point of view "no records" and "no module"
are the same answer: nothing to say. A product that has never recorded research is
not thereby unhealthy, and a report padded with empty sections teaches
the PM to skim the ones that matter.

When the active project isn't already known (in the in-app chat it is
auto-filled from the URL), start with `listProjects` and ask the PM
which product to assess if more than one could be meant.

## What the health check looks for (opinionated)

The health check holds a point of view about what a healthy product looks like.
The detailed checks per layer are in
[`references/health-checks.md`](references/health-checks.md); the
headline opinions:

- **Vision exists** and reads like an elevator pitch (Problem / Vision /
  Differentiation). A product with no Vision gets that recommended first.
- **Users exist** — at least the core user types, with goals and pains,
  not just names.
- **Domains make sense** — a coherent tree, not a dumping ground, not
  one flat list of 40 things, not empty.
- **Glossary is present** — it can be minimal, but a product with zero
  terms has no ubiquitous language. A few well-typed terms beats none.
- **Requirements have rules, and rules have examples** — a requirement
  with no rules isn't testable; a rule with no example isn't grounded
  (Specification by Example). This is the most common real gap.
- **Backlog items are linked and sized** — Features with no linked
  requirements, or PBIs with no estimate sitting in Ready for Planning,
  are findings.
- **Goals are outcomes, not outputs** — a Key Result that's really an
  output, or an Objective with a metric baked in, is a finding.
- **Research that exists is kept alive** — *only if the project records
  any.* A claim nobody has re-read in 90 days reads as **review due**,
  and a recommendation that produced neither a requirement nor a backlog
  item is **un-actioned**: research that was done and then ignored. Both
  are derived on every read, never stored, so they cannot go quietly out
  of date. Report the specific claims and recommendations, and never
  suggest clearing them in bulk — `markInsightReviewed` means somebody
  re-read the claim, and marking a batch reviewed without reading makes
  the whole signal worthless.
- **Statuses tell the truth** — statuses are gate verdicts (skald-sdd):
  a PBI In Progress whose linked requirements are all still Draft, or a
  requirement Delivered under a PBI that never started, is a finding.

## Output format

Always produce this shape:

```
## Product health: <product name>

<one or two sentences: overall state, the single biggest opportunity>

### Recommended next steps

1. <imperative next step> — <why it matters> → **<domain>**
2. ...
(up to 5, most valuable first)

### Looking healthy
- <layer>: <one line on what's already good>
```

Rules for the output:

- **1–5 recommendations.** If the product is in great shape, it's fine
  to give one or even zero — say so plainly rather than inventing busywork.
- **Most valuable first.** Lead with the step that most improves the
  product, not the easiest.
- **Each step routes to a domain.** Name the domain and, where you can, the
  specific artefact ("REQ-23 has no examples → **requirements** work").
- **Be opinionated but not preachy.** Explain *why* a gap matters in one
  clause; don't lecture.
- **Acknowledge what's good.** The "Looking healthy" section keeps the
  read honest and tells the PM what not to worry about.

## Routing map

Each finding routes to exactly one domain (load the matching domain skill
to do the work):

| Finding | Route to |
|---------|----------|
| No / weak Vision, missing Users | **Strategy** |
| Domain tree messy / empty, glossary thin | **Domains & Glossary** |
| Requirements without rules/examples, untestable reqs, splitting needed | **Requirements** |
| PBIs unlinked / unsized / unrefined, backlog shape | **Planning** |
| No OKRs, output-shaped KRs, no check-ins | **Goals** |
| Claims overdue for review, recommendations that produced no work | **Research** (skald-research) |
| A refined PBI ready to build / ship | **Delivery** — concept in **skald-sdd**; conducted from the PM's dev tooling via the speckit-skald-sync Spec Kit preset (not available in the in-app chat; point the PM there) |
| Statuses that don't match reality, "what does Approved/Done mean?" | **skald-sdd** (the lifecycle + gate-verdict semantics), then Requirements/Planning for the fix |

## What the health check does NOT do

- **No writes.** The health check never creates or edits anything. It hands off to
  a doing-skill, which gets its own confirmation from the PM.
- **No prioritisation of the backlog.** That's PM-only (planning domain).
  The health check can say "your backlog has 12 unsized PBIs" but doesn't reorder.
- **No busywork.** If a layer is genuinely fine, say so. Don't
  manufacture five findings to fill the list.
- **No fixed methodology lecture.** The health check meets the product where it is.

## Tool catalogue

Read tools only — the union of the read tools across all six domains.
Full list with what each tells you: [`references/health-checks.md`](references/health-checks.md)
§ Tools.
