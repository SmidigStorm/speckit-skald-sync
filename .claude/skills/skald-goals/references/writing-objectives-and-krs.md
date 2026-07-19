# Writing strong Objectives and Key Results

Practical coaching patterns for the agent during an OKR-setting session.
The PM owns the goals; your job is to sharpen them against the Perdoo
rules in `okr-methodology.md`.

## Setting up from scratch

The hierarchy forces an order. A from-zero session goes:

1. **Time Period** — confirm or create the window (e.g. "Q3 2026",
   2026-07-01 → 2026-09-30). `startDate` must be strictly before
   `endDate`.
2. **Objective** — one qualitative statement, tied to that period.
3. **Key Results** — 1–5 measurable outcomes under the Objective.
4. Later, **Check-ins** record progress on each KR.

Always read first: `getTimePeriods` and `listGoals` so you don't
duplicate an existing period or Objective.

## Coaching an Objective

Ask the PM what they want to be true at the end of the period. Then
pressure-test:

- **Has it got a number in it?** → That number is a Key Result. Pull it
  out. The Objective stays qualitative.
- **Is it inspiring / directional?** → If it reads like a task
  ("Refactor the search index"), it's not an Objective. Reframe to the
  why ("Make search feel instant for every PM").
- **Could the whole team repeat it from memory?** → If not, tighten.
- **Does it align with the Vision or a higher OKR?** → If you can't
  trace it upward, ask the PM how it connects.

Good vs weak:

| Weak | Strong |
|------|--------|
| Increase weekly active users to 50 | Make Skald a daily habit for product managers |
| Ship the share-link feature | Let PMs collaborate with stakeholders outside Skald |
| Improve performance | Make every core interaction feel instant |

## Coaching a Key Result

For each KR, get to a metric with a start and a target:

- **Start value** — where the metric is today. If the PM doesn't know,
  that's a signal to measure before committing the KR.
- **Target value** — where they want it by period end. Must differ from
  the start (the tool rejects equal values).
- **Unit** — free text: `%`, `users`, `NOK`, `requirements`, `seconds`.

Then apply the outcome test:

- **Is this something the team *does* or something that *results*?**
  "Run 3 customer interviews" is a doing (output). "Lift activation
  rate from 20% to 40%" is a result (outcome). Push to the outcome; the
  doing becomes a linked PBI.
- **Is it within the team's influence?** A KR the team can't move
  through their own work is a wish, not a key result.

Good vs weak KRs:

| Weak (output) | Strong (outcome) |
|---------------|------------------|
| Publish 10 blog posts | Grow organic signups from 100 to 400 / month |
| Build the onboarding flow | Lift day-1 activation from 30% to 60% |
| Hold 5 user interviews | Cut time-to-first-requirement from 2 days to 2 hours |

## Worked example (Skald)

**Time Period**: Q3 2026.

**Objective**: *"Make Skald the obvious home for a PM's daily work."*

**Key Results**:

1. Weekly active PMs: `5 → 50` (unit: `PMs`).
2. Median time-to-first-requirement: `2 → 0.1` (unit: `days`).
3. Projects with a populated Vision and ≥3 Users: `1 → 15`
   (unit: `projects`).

Note: each KR is an outcome, all are within the team's influence, there
are three of them (well under five), and none contain the *work* —
the work (e.g. "build the onboarding flow") lives in PBIs linked to
KR 2 and KR 3.

## Check-in coaching

When the PM reports progress:

- Capture the **exact value** they state. Never round "about 30ish" to
  30 — ask them to commit to a number.
- Offer to add a **note** for context ("two big customers churned;
  number is soft").
- Skald has **no confidence field** — if the PM wants to express
  confidence ("I think we'll miss this"), it goes in the note.
- Interpret the value against the stretch/committed framing for that
  Objective (see `okr-methodology.md`): 70% on a stretch KR is good; on
  a committed KR it's a miss.

## When a KR is really a KPI

If the PM proposes a metric they just want to *monitor* (uptime, churn,
weekly actives as a steady-state number) rather than *transform*, gently
flag it: that's a KPI, not a Key Result. KPIs keep the lights on; KRs
move the business. Skald's Goals module is for the transformation goals.
