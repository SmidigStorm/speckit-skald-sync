# Estimation: S/M/L/XL for AI-assisted delivery

Skald uses a four-bucket t-shirt scheme — **S / M / L / XL** — for PBI
sizing. The PM sizes; the agent's job is to **surface comparable
anchors** from the past so the PM can calibrate fast.

## Why t-shirt sizing, not story points

T-shirt sizes are coarse-grained on purpose. They sidestep the
"this is a 3 but maybe a 5" debate that consumes time without buying
accuracy. With four buckets the team can move fast: pick a size,
discuss only outliers, ship.

This matches the broader industry shift in 2026 toward simpler sizing
(XS–L, or "this is too big to estimate") as AI-augmented delivery
makes traditional story-point velocity volatile and less meaningful.

## The crucial framing for Skald: AI-assisted delivery effort

In this product, delivery effort is **not** "engineer coding time" any
more. The mental model that points to size to person-days breaks down
because:

- AI generates substantial code in one focused session.
- Most effort shifts to prompting, validating output, integrating it,
  and reviewing the final result.
- Setup, ambiguity, integration surface, and review depth dominate
  rather than typing speed.

So when you size a PBI, ask the right question: **how long will the
AI-augmented loop take to deliver this end-to-end, including human
review?** Not "how many person-days would this have been pre-AI?".

## Reference anchors

The fastest way to size is by comparison to a known past PBI. Pick one
S, one M, one L, and one XL from completed work that the team
remembers well — those become the anchors. New PBIs get sized by "is
this more or less than the anchor M?".

Picking good Skald anchors:

- An anchor is a PBI everyone on the team has clear recall of.
- It shipped recently enough that the AI-augmented delivery effort is
  comparable to today.
- It's not an outlier — pick something representative, not the easiest
  or hardest PBI you ever shipped.

When sizing a new PBI, the agent should:

1. Use `listPbis` to find recently-shipped (`status: Done`) PBIs of
   similar type and shape.
2. Surface the anchor PBIs and their sizes to the PM.
3. Let the PM make the call.

Don't claim a size on your own. The team's reference frame is theirs.

## What each bucket means (rough)

These are guidelines, calibrated by the team's anchors:

- **S** — small, well-understood, low-risk. One focused AI-assisted
  session including review. Typical: a small UI tweak, a one-line
  schema change with migration, a copy update, a bug fix with clear
  repro.
- **M** — moderate. Multi-step delivery, but no big unknowns. Typical:
  a new field on a CRUD entity with UI, a new tool added to the agent
  catalogue end-to-end, a small spec-kit story.
- **L** — substantial. Multiple touched areas, several integration
  points, or non-trivial review depth. Typical: a small feature spec
  delivered end-to-end with new schema, route, and UI.
- **XL** — too big. Pause and split. If after splitting the pieces are
  still XL, leave the PBI at XL and timebox a Spike to surface what
  makes it that big.

XL should always feel like a yellow flag. PBIs that ship are usually
S/M/L. If a PBI is genuinely XL, the team should know that's an
exception, not the norm.

## When to push vs leave the estimate empty

Don't push for a size when the PBI lacks enough context. A PBI at
Refining Requirements can sit without an estimate. Before it moves to
Ready for Planning it should usually be sized.

If the PM is unsure, offer:

- An anchor comparison ("the persistent agent panel was an L; how does
  this compare?")
- A split proposal ("this feels XL; can we slice off the share-link
  generation as a separate S/M PBI?")
- A Spike ("can we put a 1-day timebox on a Spike to find out how big
  this really is?")

## Sources

- *T-Shirt Sizing in Agile* (multiple 2026 guides). Asana, Easy Agile,
  KnowledgeHut.
- *AI-Story-Point Estimation in Agile Teams*, Growing Scrum Masters.
- *Mike Hutson on AI-augmented velocity volatility* (Medium, 2026).
- *Story Points vs. T-Shirt Sizing*, Agile Seekers.
- Scrum.org forum discussion on AI dev acceleration and estimation.
