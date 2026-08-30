---
name: skald-planning
description: 'How to create, refine, split, estimate, prioritise and link Product Backlog Items (PBIs) in Skald. Covers PBI types (Feature / Bug / Refactor / Spike) and title shapes, S/M/L/XL sizing calibrated for AI-assisted delivery, the variable PBI↔requirement relationship, backlog refinement, the user-perspective splitting heuristic, release bundling, status transitions (the agent proposes; the PM moves), and the PM-only prioritisation rule. Use whenever the user is creating, editing, splitting, sizing, prioritising, releasing or refining a PBI — even if they don''t say "Skald". Also trigger on "create a backlog item", "add to the backlog", "refine this PBI", "what should we work on next", "size this", "estimate this", "link this PBI to a requirement", "what release does this belong in", "split this", or any Skald planning tool call (createBacklogItems, updateBacklogItem, listPbis, getTeams).'
---

# Skald Planning

> **This is reference for you, not a script to read out.** The user asked a
> question, not for the manual. Answer it in a few points — three to five, the
> ones that matter for what they actually asked — then ask which part they want
> to go into. Never deliver a section-by-section tour of this document. If you
> find yourself writing headings, you are reciting: stop and ask instead.


## What a PBI is in Skald

A Product Backlog Item (PBI) is a **planned unit of work**. PBIs group a set
of requirements (or pieces of work without an explicit requirement) into
something a team will deliver together.

In Skald's Spec-Driven Development model, the PBI is the **change unit**:
**one PBI is one change** — one delivery flow, one derived spec, one set of
status flips when gates go green. The linked requirements are the living
specs it builds against. Full picture: the skald-sdd skill.

Each PBI is one row in the `backlog_items` table with:

- **Title** — describes the work or the feature. Shape depends on the
  PBI type (see below).
- **Type** — Feature (default), Bug, Refactor, or Spike.
- **Status** — Refining Requirements / Ready for Planning / In Progress
  / Done.
- **Estimate** — S / M / L / XL. Optional during early refinement.
- **Team** — the team assigned to deliver it. Optional until planned.
- **Release** — a first-class, project-scoped artefact a PBI is
  assigned to via its `releaseId` (a stakeholder-facing bundle, e.g. a
  version or a theme). Optional. Resolve names → UUIDs with
  `listReleases`. (A legacy free-text `release` label still exists but
  is deprecated — do not use it for assignment.)
- **Linked Requirements** — zero or more. Linkage is genuinely
  optional; many PBIs are non-feature work.

## PBI types and what their titles look like

The PBI type drives the title shape because the kind of work is
different. See [`references/pbi-types.md`](references/pbi-types.md) for
the full guidance, including when to ask the PM vs infer the type from
context.

In short:

- **Feature** (default) — a new capability or increment. Title is the
  feature or capability: `Backlog inline edit`, `Persistent agent panel`,
  `Vision editor`.
- **Bug** — a fix to existing behaviour. Title names the bug or the
  symptom: `Sort order lost on reload`, `Tooltip clipped on narrow
  viewport`.
- **Refactor** — internal restructuring or hardening with no user-facing
  behaviour change. Title names what is being restructured: `Extract
  release-core from planning actions`, `Harden the migration pipeline`.
- **Spike** — research, prototype, or learning. Title prefixes with
  `Spike:` and names the question: `Spike: Mastra evaluation framework`,
  `Spike: pg_trgm performance on 10M-row search index`.

The skill should **evaluate** which type a piece of work is and offer
that as the default. The PM can override.

## PBI ↔ Requirement: both flows are valid

Skald deliberately allows two orderings:

- **Requirements first, PBI later.** The team has agreed on a capability
  but hasn't yet decided when to deliver it. The PBI gets created later
  during planning and links to the requirement(s).
- **PBI first, requirements later.** The team knows they want to work on
  something (often a Spike, a Bug, or a roughly-shaped Feature) but
  hasn't fully refined it yet. Requirements get attached during
  refinement, or never (for Bugs, Refactors, and Spikes).

Linkage is **optional**. A PBI without any linked requirements is fine —
that's the right shape for most Bugs and Spikes, and for early-stage
Features before refinement starts.

When linkage *does* make sense (typically for Features), use
`linkRequirementToBacklogItem` after confirming with the PM. The
relationship is many-to-many; a PBI can implement several requirements,
and a requirement can be delivered across several PBIs.

## Estimation: S/M/L/XL calibrated to AI-assisted delivery

Skald uses the four-bucket t-shirt scheme: **S / M / L / XL**. The PM
sizes; the agent surfaces comparable past PBIs as anchors.

**Crucial framing**: in this product the delivery effort is no longer
"engineer coding time". It's the AI-augmented loop — prompting,
verifying, integrating, reviewing. Size with that in mind, not with
old "story points = effort in person-days" muscle memory.

See [`references/estimation.md`](references/estimation.md) for the
calibration approach, the AI-augmented framing, and how to use past
PBIs as reference anchors.

Estimate is optional during early refinement. Don't push for a number
before the PBI has enough context. A PBI without an estimate is fine
during Refining Requirements; before it moves to Ready for Planning it
should usually be sized.

## Prioritisation: PM only

The order of the backlog is the PM's call. The agent never reorders
without an explicit request.

You may surface things that look out of order ("this PBI is below this
one, but it's a Must-have requirement; want to reconsider?"), but you
don't move them. `updateBacklogItem` with a new `sortOrder` is a write
that requires the same plain-language summary + explicit confirmation
as any other write — and the PM is the one who decides.

## Releases: stakeholder-facing bundles

A Release is a **first-class, project-scoped artefact** that groups PBIs
into something that makes sense to **communicate to stakeholders**.
That's the test: would you put this on a release note, in a sales pitch,
or in an internal update? If yes, it's a release.

Two common shapes:

- **Version-style**: `v1.0`, `v1.1`. Calendar-bounded delivery cuts.
- **Theme-style**: `Basic Security`, `Multi-org Support`,
  `Pre-launch Polish`. Capability-bounded.

**How to assign a PBI to a release** (the only correct path):

1. `listReleases({ projectId })` — see the project's existing releases
   and their UUIDs. Offer the PM an existing release before creating a
   new one, so you don't fragment the set.
2. If none fits, `createReleases({ projectId, releases: [...] })` first.
3. `updateBacklogItem({ projectId, id, releaseId })` — assign by the
   release **UUID** (pass `releaseId: null` to remove the item from its
   release). This writes the first-class `release_id` FK that the
   Release Planning page groups by.
4. To confirm, re-read the item with `getBacklogItemDetail` and check
   its `releaseName` / `releaseId` (or see `releaseName` on
   `getExistingBacklogItems`).

The agent doesn't assign releases on its own — ask the PM and confirm
before the write.

**Conducting a release-planning session** ("bundle features into
releases"): read the backlog with its requirement links, then

1. **Propose coherent bundles** — groups of PBIs that make one
   stakeholder-communicable story, with a suggested target date each.
2. **Surface the leftovers honestly** — items that aren't ready (see
   the readiness judgment below) or genuinely fit no bundle; say why.
3. **Suggest an order, never decide it** — sequencing across releases
   is prioritisation, and prioritisation is PM-only.
4. One confirmation can cover a whole proposed bundle (create the
   release + assign its items), per the confirmation discipline.

> **Do not** set the legacy free-text `release` field to assign a
> release. It is deprecated, is not the `release_id` FK, and does **not**
> show up on the Release Planning page — using it is the classic
> "I assigned it but nothing happened" failure. Always use `releaseId`.

## Status transitions: agent proposes, PM moves

The four PBI statuses are:

- **Refining Requirements** — the PBI exists but the work isn't fully
  understood yet. May lack linked requirements, may lack an estimate,
  may lack a clear acceptance bar.
- **Ready for Planning** — the work is understood enough to be picked
  up. Typically: estimate set, a clear definition of Done, dependencies
  identified.
- **In Progress** — the team is actively working on it.
- **Done** — the change is **live in production**. Not merged, not
  demo-ready — live. Statuses are **gate verdicts** (see skald-sdd):
  never propose Done while a mandatory gate is red or unrun; report the
  true state instead.

The agent **may propose** a status change ("this PBI now has a sized
estimate and three linked requirements — want me to move it to Ready
for Planning?") but never moves status without confirmation. The PM is
the source of truth for status flow.

## Is a backlog item ready to build?

"Ready to build" is a **judgment you make**, not a checklist you tick.
When the PM asks — or at the end of planning a feature — read the backlog
item **together with its linked requirements** (`getBacklogItemDetail`
returns both in one call) and evaluate whether someone could pick it up
and build it without guessing. Read it the way a good reviewer reads a
spec:

- **Coverage** — do the linked requirements actually cover what this item
  is about, or are there obvious holes (an unhandled case, a missing rule,
  no error / edge behaviour)?
- **Refinement** — are the requirements real (rules and examples, not a
  bare title) and settled (past Draft, no blocking open question — see
  requirement status in the skald-requirements skill)?
- **Clarity** — is the intent unambiguous? Would two developers read it
  the same way?
- **Coherence** — anything contradictory, or clearly out of scope for
  this item?

Give the PM a plain verdict, then — the important part — **name what is
missing or thin and offer to fix it**: draft the missing requirement, add
rules / examples, resolve an open question, or move a settled requirement
past Draft. Do **not** reduce this to "it has a requirement and an
estimate, so it's ready" — that counts fields instead of understanding
the work.

Estimate, team, and release are **planning logistics** you also help set
during `delegate-and-plan`, and the item's detail page shows a quick
readiness indicator — treat that as a glance. Neither replaces your
judgment about whether the work is actually understood.

## Refinement workflow

During refinement the agent's job is to:

1. **Read what's already there.** Use `listRequirements` filtered to
   the PBI's linked requirements, `listRules` for each, `listExamples`
   for each. Surface what's known.
2. **Spot the gaps.** Missing rules, missing examples, missing
   acceptance bar, no linked requirements when the PBI is clearly a
   Feature.
3. **Suggest splits.** If the PBI is too big, propose splits along the
   user-perspective heuristic (see below). Frame each split as its own
   PBI title.
4. **Offer linkage.** If the PBI doesn't yet have linked requirements
   and is a Feature, surface candidate requirements via
   `listRequirements` and offer to link.
5. **Capture follow-ups.** If the PM raises something that should
   become its own PBI later (a Bug surfaced during a Feature
   refinement, a Spike that should precede the Feature), propose creating
   it — don't try to bundle everything into the current PBI.

See [`references/refinement.md`](references/refinement.md) for the full
flow plus the splitting heuristic and worked examples.

## Splitting heuristic: valuable increments from the user's view

PBI splitting is different from Requirement splitting. Requirements use
INVEST + Lawrence's nine patterns (see the skald-requirements skill).
For PBIs, the lens is simpler:

> **Each split must be a valuable increment from the user's
> perspective.**

A team should be able to deliver a set of PBIs within a short time
period and have each PBI be a real step forward for the user. A split
that produces "build the backend" + "build the frontend" fails this
test — neither half is useful to the user alone. A split that produces
"share-link generation" + "public read view" + "expiration" can pass —
each one moves the user-visible capability forward.

For Spikes and Bugs, the heuristic loosens. A Bug usually doesn't
split. A Spike splits along the questions being investigated.

Details and examples: [`references/refinement.md`](references/refinement.md)
§ Splitting.

## Check for duplicates before creating

Before creating a PBI, read the existing backlog — `getExistingBacklogItems`
/ `listPbis`. A PBI with a near-identical title, or covering the same work,
is a duplicate. Surface the match to the PM and ask whether to extend the
existing item or genuinely add a new one. **Never create blind.** When a
duplicate slips through, reconcile with the repo's merge convention: rename
the superseded item `[MERGED → PB-NN]` and move it to Done, keeping the
linked/specced item as the survivor (see PB-28/29/30 for the shape).

## Confirmation discipline

This applies to every write tool you call:

1. Summarise the intended change in plain language (a proposal).
   Include the PBI title, type, status, and any other touched fields
   (estimate, team, release, linked requirements).
2. Wait for the PM's go-ahead — a plain "yes" to the proposal suffices.
3. Write everything the confirmed proposal covers — no fresh "yes"
   per item.
4. Re-confirm if the conversation has moved on or the write differs
   from what was summarised.

This matches the discipline both agentic surfaces enforce. Restated
here because this skill is shared: the in-app Skald Agent and external
MCP clients load this same document.

## Tool catalogue

Full list with when-to-use, primary and adjacent: [`references/tool-catalogue.md`](references/tool-catalogue.md).

Quick reference — the tools you'll reach for most:

**Reads** (no confirmation needed):
- `listPbis({ projectId })` — every PBI in the project. Pass
  `includeArchived: true` to also see archived items (each row carries an
  `archived` flag) when you need to find one to restore.
- `getExistingBacklogItems({ projectId })` — same data, slightly richer
  shape (incl. `releaseName`) for triaging gaps (also takes `includeArchived`).
- `getBacklogItemDetail({ projectId, backlogItemId })` — full single-PBI
  read; reports `releaseId` + `releaseName` so you can confirm a release
  assignment.
- `listRequirements({ projectId })` — for linkage candidates and
  refinement context.
- `getTeams({ projectId })` — team assignment options.
- `listReleases({ projectId })` — releases + their UUIDs; resolve a
  release name before assigning a PBI to it.

**Writes** (confirmation discipline applies):
- `createBacklogItems` — create one or more PBIs.
- `updateBacklogItem` — title, description, status, estimate, team,
  `releaseId` (first-class release assignment), sortOrder.
- `createReleases`, `updateRelease` — create / rename / retarget /
  archive a release.
- `archiveBacklogItem`, `restoreBacklogItem` — archive is the destructive
  verb (there is **no delete**); archiving removes the PBI from the active
  backlog, board, roadmap, and search, and **clears its team, planned
  dates, and release** (requirement links are preserved). `restoreBacklogItem`
  brings it back **unassigned** — the cleared associations are not
  reinstated. Resolve an archived PBI via `listPbis({ includeArchived: true })`.
- `linkRequirementToBacklogItem`, `unlinkRequirementFromBacklogItem` —
  manage the many-to-many trace.

**Adjacent reads** worth doing before writing a PBI:
- `listDomains` — context for what part of the product the PBI sits
  in.
- `listDomainTerms` — use the project's existing vocabulary so the PBI
  title reads consistently.
- `listGoals` — surface the Key Result this PBI might support; the PM
  may want to link via `linkKeyResultToBacklogItem`.

## Draw it

In the app's agent workspace you have a canvas.

- Explaining **what a backlog item is** — its types, how it's sized, that Skald is
  sprint-free — call `showCanvasVisual` with `visual: "backlog-model"`.
- Asked whether one specific item **could be handed to a team or a coding agent
  right now**, call it with `visual: "item-readiness"`. Answer with what would
  stop it, not with how ready it is.
- **Asked to SEE a specific artefact** — "show me PB-12", "open REQ-40", "what's
  in this item" — call it with `visual: "subject-card"` as you fetch the
  details. Draw it, then talk through what you found; do not answer with a
  prose-only summary of something the canvas can show.

Not this visual: *how* a change becomes code, and what a status means as a
verdict, is `sdd-delivery`.
