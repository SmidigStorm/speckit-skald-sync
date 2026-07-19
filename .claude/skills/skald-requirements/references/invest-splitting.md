# INVEST and requirement splitting

## INVEST in one paragraph

INVEST is a sanity check for a single requirement, coined by Bill Wake. A
well-formed requirement is **I**ndependent (can be delivered on its own),
**N**egotiable (scope can flex; not over-specified), **V**aluable
(delivers something a user or system meaningfully receives), **E**stimable
(the team can size it), **S**mall (fits in a normal delivery rhythm), and
**T**estable (the rules and examples make Done unambiguous). When any of
the six start to fail, that's the signal a requirement might need to be
split, reframed, or de-scoped.

In Skald, the requirement type drives a lot of this: a User Story should
be Valuable to the persona named in the title; an EARS requirement should
be Testable through its rules and examples.

## The Skald splitting heuristic

Before reaching for a pattern, apply the Skald-specific decision rule:

- **Splittable** — the requirement has multiple rules, and at least one
  rule could plausibly stand alone as its own User Story (i.e. that rule
  alone delivers some user-visible or system-meaningful value). Suggest
  splitting.
- **Not splittable** — all rules must apply together for any of them to
  deliver value (e.g. a checkout flow where partial steps make no sense
  to ship). Leave it as one requirement.

This is the lens to apply first. If the heuristic says "splittable", the
Lawrence patterns below tell you *how* to split.

## The nine Lawrence patterns (Humanizing Work guide)

Adapted from Richard Lawrence's *Patterns for Splitting User Stories* and
its successor flowchart, maintained at humanizingwork.com. Listed in the
order Lawrence recommends trying them. The earlier patterns tend to
produce better vertical slices than the later ones.

### 1. Workflow Steps

Split a multi-step process into one requirement per step. Build the
basic end-to-end happy path first, then add intermediate steps and edge
cases as their own requirements.

Skald example: "As a PM, I want to share a backlog item with stakeholders
so that they can comment" → split into "generate a share link", "render
the read-only view", "expire a share link".

### 2. Operations (CRUD)

A requirement using a vague verb like "manage" usually hides multiple
operations. Separate Create, Read, Update, Delete (and Archive) into
their own requirements.

Skald example: "Manage a glossary term" → "create a glossary term",
"update a glossary term", "archive a glossary term".

### 3. Business Rule Variations

If a requirement names multiple business rules with different logic,
each rule (or rule cluster) can become its own requirement.

Skald example: "Filter the backlog" → one requirement per filter kind
(status, team, estimate, release).

### 4. Data Variations

If a requirement handles many data shapes, split by data shape. Start
with the simplest shape and add the others later.

Skald example: "Render any artefact in the share view" → start with
backlog items, then requirements, then goals.

### 5. Data Entry Methods

If a requirement bundles "the capability" with "the polished input UI",
ship the simplest input first and the polished input later.

Skald example: "Pick a date for a check-in" → "type the date into a
text field" first; "use the calendar picker" later.

### 6. Major Effort

If most of the work concentrates upfront on one variation (e.g.
infrastructure for one card type), do that variation first and let the
others become small follow-ups.

Skald example: "Ingest webhooks from any Clerk event" → "handle
`user.deleted` end to end" first; subsequent event types become small
additions.

### 7. Simple/Complex

Ask "what's the simplest version of this that's still valuable?". Ship
that. Park the complications as follow-up requirements.

Skald example: "Search across all entity types" → "fuzzy-match titles"
first; "rank by recency", "boost active artefacts", "include archived
on toggle" come later.

### 8. Defer Performance

Ship correctness first; pursue performance separately. Performance
becomes its own requirement once a real baseline exists.

Skald example: "Reconcile the search index" → "correct reconciliation,
even if slow" first; "reconcile within 30s on a 100k-row table" later.

### 9. Break Out a Spike

If a requirement is poorly understood, timebox a research spike as a
separate (non-user-facing) work item to remove the uncertainty. Use
sparingly — only after the other eight patterns fail.

Skald example: a poorly understood third-party integration where the
team needs to read the docs and prototype before any requirement can be
made Testable.

## The meta-pattern

Lawrence's umbrella advice across all nine patterns:

1. **Find the central complexity.** What's the thing that makes this
   requirement big — many user types? many data shapes? many edge cases?
   an unfamiliar integration?
2. **List the variations.** Enumerate the differences along that axis.
3. **Reduce variations to one path through the complexity.** That's
   your first requirement. The remaining variations become follow-ups.

## Evaluation rules

When you've drafted candidate splits, check:

- **Drop value**: can you imagine the PM choosing not to ship some of
  the smaller requirements? A good split exposes low-value pieces so
  they can be deprioritised or discarded.
- **Balance**: prefer splits that produce roughly equally-sized
  requirements. A 5+3 split is usually better than 8+0 (where one piece
  is trivial and the other carries all the work).
- **Vertical slices**: each split should still deliver some user-visible
  or system-meaningful value. "Build the database table" is not a
  valid split.

## In Skald, mechanically

Once the PM agrees to split a requirement:

1. Call `getRequirementForSplit({ id })` to read the existing rules,
   examples, and open questions.
2. Propose the split out loud — list the new requirement titles and how
   the existing rules / examples / open questions map to them.
3. Confirm with the PM.
4. Call `splitRequirement` with the agreed map. Skald preserves
   traceability from the original to the new requirements.

## Sources

- *The Humanizing Work Guide to Splitting User Stories*, Richard
  Lawrence et al. — `https://www.humanizingwork.com/the-humanizing-work-guide-to-splitting-user-stories/`.
- *Patterns for Splitting User Stories*, Richard Lawrence, 2009.
- INVEST, Bill Wake, *Extreme Programming Explored*, 2003.
