---
name: skald-goals
description: How to set and track OKRs (Objectives and Key Results) in Skald, following the Perdoo methodology. Covers the Time Period → Objective → Key Result → Check-in hierarchy, the qualitative-Objective vs measurable-Key-Result split, the outcome-not-output rule for Key Results, the ≤5 KRs per Objective limit, stretch-goal calibration, the weekly check-in cadence (value + note; no confidence field in Skald), and linking Key Results to Product Backlog Items so delivery rolls up to outcomes. Use this skill whenever the user is setting goals, writing or refining objectives or key results, creating a quarter/time period, checking in on progress, or connecting backlog work to outcomes — even if they don't say "Skald". Also trigger on phrases like "set our OKRs", "what should our objective be", "is this a good key result", "that's an output not an outcome", "check in on the KR", "create Q3", "link this PBI to a key result", or any call to a Skald goals tool (objective, key-result, check-in, time-period, and KR-link tools).
---

# Skald Goals (OKRs)

Skald's Goals module follows the **OKR** framework as codified by Perdoo.
The agent helps the PM write good OKRs and record progress; the PM owns
the goals.

Full methodology reference: [`references/okr-methodology.md`](references/okr-methodology.md).
How to write strong Objectives and KRs: [`references/writing-objectives-and-krs.md`](references/writing-objectives-and-krs.md).

## The hierarchy

Skald enforces a four-level structure. You can't create one level without
its parent:

```
Time Period   (e.g. "Q3 2026", a date-bounded window)
  └─ Objective    (qualitative, inspiring, no metrics)
       └─ Key Result   (quantitative: start → target, a unit)
            └─ Check-in   (a point-in-time measured value + note)
```

- A **Time Period** must exist before you can create an Objective.
- An **Objective** must exist before you can create a Key Result.
- A **Key Result** must exist before you can record a Check-in.

So a from-scratch OKR setup is: `createTimePeriod` →
`createObjective` → `createKeyResult` (×N) → later, `createCheckIn`.

## Where Objectives come from: the Vision

Before proposing or refining Objectives, read
`getExistingProjectVision` (and `listProjectUsers` if user-facing
outcomes are in play). Objectives are how the vision's **Business
Goals** become measurable — the skald-strategy skill's vision interview
deliberately hands off here, and an Objective that serves no part of
the vision deserves the question *"what is this in service of?"*. If
the project has no vision yet, say so and offer the strategy work
first; don't invent direction from a blank page.

## Objective vs Key Result — the core split

This is the distinction the whole framework rests on:

- **Objective** — *what we want to achieve*. Qualitative, directional,
  inspiring. **No metrics.** Example: *"Make Skald the obvious choice
  for multi-team product management."*
- **Key Result** — *how we'll know we got there*. Quantitative,
  measurable, with a start value, a target value, and a unit. Example:
  *"Active weekly PMs: 5 → 50."*

If a proposed Objective has a number in it, it's probably a Key Result
wearing an Objective's clothes. If a proposed Key Result has no number,
it's not measurable yet — push for the metric.

## The outcome-not-output rule

The single most common OKR mistake, per Perdoo: writing Key Results
that measure **activity** instead of **results**.

- **Output** (avoid): *"Ship 50 sales calls"*, *"Release the share-link
  feature"*, *"Write 10 blog posts"* — things you *do*.
- **Outcome** (aim for): *"Close 10 new customers"*, *"Lift weekly
  active PMs from 5 to 50"*, *"Cut time-to-first-requirement from 2
  days to 2 hours"* — results of what you do.

When a PM proposes an output-shaped KR, name it and offer the outcome
behind it: *"'Ship the share-link feature' is an output — what outcome
are we hoping it drives? Maybe 'external stakeholder comments per week:
0 → 20'?"*. The output itself usually belongs in a **PBI** linked to the
KR (see linkage below), not in the KR text.

## Less is more

Perdoo's headline guidance: keep it tight.

- **≤ 5 Key Results per Objective.** More than that and focus
  dissolves. If the PM wants more, ask which ones actually matter.
- **A handful of Objectives per Time Period**, not dozens. OKRs are the
  vital few transformation goals, not a catalogue of everything the
  team does.

## Stretch vs committed

Calibrate ambition explicitly with the PM:

- **Stretch goals**: if the team consistently hits 100%, the goals
  aren't ambitious enough. ~70% attainment is a healthy stretch
  outcome.
- **Committed goals**: if the team is *not* using stretch framing,
  then 100% is the expectation and anything less is a miss.

Ask which mode a given Objective is in so check-in interpretation is
honest. Skald doesn't store this flag — capture it in the Objective's
description if it matters.

## Check-ins: cadence and shape

- **Cadence**: weekly progress updates are the Perdoo default. Company
  OKRs run on a long (annual) cadence, team OKRs on a short (quarterly)
  cadence; check-ins keep both alive between boundaries.
- **Shape in Skald**: a Check-in is a measured **value** (string,
  captured exactly as the PM states it — never round) plus an optional
  **note**.
- **No confidence field.** Perdoo's method tracks a confidence level
  alongside the value; Skald's Check-in does **not** have a confidence
  field. Do not fabricate one or try to encode it in the value. If the
  PM wants to record confidence qualitatively, put it in the `note`.

See [`references/okr-methodology.md`](references/okr-methodology.md)
§ Check-ins for the cadence rationale.

## Linking Key Results to PBIs

The bridge between outcomes and outputs. A Key Result is the outcome; the
PBIs linked to it are the outputs (Perdoo calls these "Initiatives")
the team believes will move the metric.

- Before linking, call `getExistingBacklogItems` to confirm the PBI
  exists in the active project.
- Then `linkKeyResultToBacklogItem`. The link is idempotent.
- This is how delivery work rolls up to goals. During PBI refinement
  (see the skald-planning skill) you can offer the reverse: "this PBI
  looks like it moves KR X — want to link them?".
- Close the loop after delivery: when a linked PBI reaches **Done**
  (live in production — see skald-sdd), the next check-in is where the
  bet gets tested. Ask: did the metric move? A KR whose linked PBIs
  keep shipping while the value stays flat is telling you the outputs
  aren't buying the outcome — surface that, don't bury it.

## Check for duplicates before creating

Before creating a Time Period, Objective, or Key Result, read the existing
tree — `getTimePeriods`, `listGoals` / `getExistingGoals`. A near-identical
Objective, a KR already tracking the same metric, or a Time Period that
already exists is a duplicate. Surface it to the PM and ask whether to reuse
or extend rather than adding a second. **Never create blind.**

## Confirmation discipline

Every write tool here is preceded by a plain-language summary (a
proposal) and the PM's go-ahead — a plain "yes" to the proposal
suffices. One confirmation covers the whole proposal it answers.
Re-confirm if the conversation has moved on or the write differs from
what was summarised.

For Key Results specifically: **state all numeric values explicitly** in
the summary — start value, target value, and unit — before calling
`createKeyResult`. The PM confirms the exact numbers. The tool rejects a
`targetValue` equal to `startValue`.

For Check-ins: state the exact value you're about to record and confirm
it's right before calling `createCheckIn`. Never round.

This matches the discipline both agentic surfaces enforce. Restated
here because this skill is shared: the in-app Skald Agent and external
MCP clients load this same document.

## Tool catalogue

Full list with when-to-use: [`references/tool-catalogue.md`](references/tool-catalogue.md).

Quick reference:

**Reads** (no confirmation needed):
- `getTimePeriods({ projectId })` — existing time periods.
- `listGoals({ projectId })` / `getExistingGoals({ projectId })` — the
  Objective + KR tree.
- `getCheckInHistory({ keyResultId })` — progress trail for a KR.

**Writes** (confirmation discipline applies):
- `createTimePeriod`, `updateTimePeriod`.
- `createObjective`, `updateObjective`.
- `createKeyResult`, `updateKeyResult`.
- `createCheckIn`.
- `linkKeyResultToBacklogItem`, `unlinkKeyResultFromBacklogItem`.

**Adjacent reads**:
- `getExistingProjectVision` — read before proposing Objectives; they
  make the vision's business goals measurable.
- `getExistingBacklogItems` — required before linking a KR to a PBI.
- `listProjects` — when you don't already have a `projectId`.
