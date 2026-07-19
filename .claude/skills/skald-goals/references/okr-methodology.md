# OKR methodology (Perdoo)

Skald's Goals module follows the OKR framework as codified in Perdoo's
*Ultimate OKR Guide*. OKR = Objectives and Key Results — a goal-setting
framework distilled from ~75 years of goal management practice, popularised
at Intel (Andy Grove) and Google (John Doerr), and systematised by Perdoo.

## Objectives

An Objective describes a desired future achievement that sets direction.
Good Objectives are:

- **Directional & inspiring** — motivational language; something the
  team wants to rally behind.
- **Ambitious yet realistic** — pushes beyond the comfort zone without
  being fantasy.
- **Understandable** — clear, concise, no jargon. Memorable enough to
  repeat from memory.
- **Without metrics** — measurement belongs to the Key Results.
- **Aligned** — supports the product strategy / Vision or a
  higher-level OKR.
- **Time-bound** — tied to a Time Period (quarterly or annual).

Example: *"Expand into three new regional markets."* (No number — the
number lives in the KRs.)

## Key Results

A Key Result is a measurable outcome that indicates whether the Objective
was met. Each KR does two jobs: it *specifies* the qualitative Objective
(makes it concrete) and it *measures* progress toward it.

Good Key Results are:

- **Outcome-focused** — measures what was achieved, not the activity
  done. (See the outcome-vs-output section.)
- **Measurable** — a clear metric with a start value and a target
  value. In Skald: `startValue`, `targetValue`, and a free-text `unit`.
- **Relevant** — specific to its Objective.
- **Ambitious yet realistic** — within the team's circle of influence.
- **Balanced** — no more than 5 per Objective.

Example: *"Sell 10,000 grills in Germany"* (start 0, target 10000,
unit "grills").

## Outcome vs output — the critical distinction

Perdoo's sharpest rule:

> "An output is something you do, such as a task or a project. For
> example, *Make 50 sales calls* would be an output. An outcome is a
> result of what you do. For example, *I closed 10 new customers* —
> that's an outcome."

Key Results must be outcomes. Outputs — the projects and tasks that
drive outcomes — are **Initiatives** in Perdoo's vocabulary. In Skald,
Initiatives map to **PBIs** linked to the Key Result. The clean
separation is the whole point:

- Key Results answer *"What did we achieve?"* (outcomes)
- Linked PBIs answer *"What did we do?"* (outputs)

## How many

> "When it comes to OKR, less is more."

- **≤ 5 Key Results per Objective.**
- A small number of Objectives per Time Period at any one level. OKRs
  are the vital few transformation goals, not an exhaustive task list.

## Stretch goals and attainment

Perdoo frames ambition through stretch goals:

- "If you're consistently reaching 100% on your OKRs, they're not
  challenging enough." A stretch OKR landing around ~70% is a healthy
  result.
- If an organisation deliberately works *without* stretch goals, then
  "always aim to hit 100% progress on your OKRs" — these are committed
  goals and 100% is the bar.

Decide per Objective which mode applies. It changes how a check-in value
is interpreted: 70% on a stretch KR is a win; 70% on a committed KR is a
miss.

## Cadence and check-ins

OKRs work best on a **combination of cadences**:

- **Long cadence** — annual OKRs, typically at company level.
- **Short cadence** — quarterly OKRs, typically at team level.
- **Check-in frequency** — weekly updates on progress.

A check-in is a point-in-time progress entry against a Key Result. In
Perdoo's full method a check-in also carries a *confidence level* (how
likely the team is to hit the target). **Skald's Check-in does not store
confidence** — only the measured value and an optional note. Don't
fabricate a confidence field; if the PM wants to note confidence, put it
in the note text.

## OKRs vs KPIs

Don't confuse them:

- **KPIs** "keep the lights on" — they monitor the health of ongoing
  processes (e.g. uptime, churn rate, weekly active users as a steady
  metric).
- **OKRs** "provide the missing link between ambition and reality" —
  they're transformation goals that move the business from where it is
  to where it wants to be.

Car analogy: KPIs are the dashboard gauges (engine health); OKRs are the
roadmap to the destination. Skald's Goals module is for OKRs. A metric
the team just wants to *watch* (not transform) is a KPI and probably
doesn't belong as a Key Result.

## Common mistakes

Perdoo's list — watch for these and coach against them:

1. Not aligning OKRs with the product strategy / Vision.
2. Confusing OKRs with KPIs.
3. Prioritising "nice-to-haves" instead of focusing on the vital few.
4. Writing Key Results as outputs instead of outcomes.
5. Setting too many OKRs.

## Source

- Perdoo, *The Ultimate OKR Guide* —
  `https://www.perdoo.com/resources/okr-guide/`.
