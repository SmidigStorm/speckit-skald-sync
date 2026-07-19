# PBI types: Feature / Bug / Refactor / Spike

Each PBI in Skald has one of four types. The type changes how the
title reads, what status flow makes sense, whether linkage to a
requirement is expected, and how (or whether) the PBI splits.

The schema stores exactly these four (`Feature`, `Bug`, `Refactor`,
`Spike`) and defaults to `Feature`. `Story` is **not** a valid type —
never propose it.

The skill should **evaluate** which type a piece of work is and offer
that as the default during creation. The PM can override.

## Feature (default)

A new capability or increment. The team will deliver something a user
can use. This is the default type.

**Title shape**: the capability or feature name, written as a noun
phrase or short verb phrase. Not a Gherkin sentence — that's for
requirements.

Good Feature titles:
- Backlog inline edit
- Persistent agent panel
- Vision editor
- Project switcher
- Workspace administration page
- PBI archive

Anti-patterns:
- *"As a PM I want…"* — that's a requirement title, not a PBI title.
  Use it for the linked requirement, not the PBI.
- *"Implement backend for backlog inline edit"* — a vertical-slice
  Feature includes whatever it takes to make the capability work.
  Backend-only is a refinement smell.
- *"Misc UX improvements"* — too vague. Split.

**Linkage**: Features usually link to one or more requirements.
Refinement should surface or create those requirements. Linkage is
still optional at the database level — early-stage Features often start
without it.

**Splitting**: along the user-perspective heuristic — each split must
be a valuable increment from the user's point of view.

**Status flow**: full lifecycle — Refining Requirements → Ready for
Planning → In Progress → Done.

## Bug

A fix to existing behaviour. Something the product already does but
does wrong.

**Title shape**: name the bug or the symptom directly. Imperative or
declarative — both are fine.

Good Bug titles:
- Sort order lost on reload
- Tooltip clipped on narrow viewport
- Domain reparent silently fails on depth-2 nodes
- Audit log misses `domain.write` category

Anti-patterns:
- *"Fix the bug"* — which bug? Be specific.
- *"Improve UX"* — that's a Feature, not a Bug. A Bug is regression of
  expected behaviour, not a desired improvement.

**Linkage**: Bugs usually do **not** link to a requirement.
Occasionally a Bug exposes a missing rule on an existing requirement —
in that case the right move is usually to add the rule (via the
skald-requirements skill) and let the Bug PBI track the fix.

**Splitting**: rarely splits. If a Bug feels splittable, you're
probably looking at two Bugs — file them separately rather than
splitting one.

**Status flow**: typically skips Refining Requirements. Bugs that are
clear enough to file are usually clear enough to plan. Sometimes a Bug
needs a Spike first to localise the cause; in that case file the Spike
as its own PBI, leave the Bug at Refining Requirements until the Spike
finishes.

## Refactor

Internal restructuring, cleanup, or hardening with **no user-facing
behaviour change**. The product does the same thing afterwards; the
code, schema, or infrastructure is in better shape.

**Title shape**: name what is being restructured and, where it helps,
the goal. Noun phrase or short verb phrase.

Good Refactor titles:
- Extract release-core from planning actions
- Replace the three-marker denylist with a size budget
- Harden the production database migration pipeline
- De-duplicate the faceted-filter logic across list views

Anti-patterns:
- *"Improve the code"* — too vague. Name the target and the shape you
  want.
- *"Refactor and add pagination"* — if it changes what the user sees,
  the user-facing part is a Feature. Split the observable change out.
- Using Refactor to smuggle in behaviour changes — a Refactor that
  alters behaviour is mistyped.

**Linkage**: Refactors usually do **not** link to a requirement — there
is no new capability to specify. If the restructuring is in service of a
requirement (e.g. it unblocks one), note that in the description rather
than forcing a link.

**Splitting**: along seams in the code or the rollout. A large refactor
often splits on a reversibility boundary (e.g. expand in one PBI,
contract in another).

**Status flow**: typically skips Refining Requirements — there are no
requirements to refine. Goes Ready for Planning → In Progress → Done.

## Spike

Research, prototype, or learning. The team needs to know something
before they can plan a Feature or fix a Bug.

**Title shape**: prefix with `Spike:` and name the question being
investigated.

Good Spike titles:
- Spike: Mastra evaluation framework
- Spike: pg_trgm performance on a 10M-row search index
- Spike: Clerk OAuth refresh-token configuration for MCP
- Spike: feasibility of per-call orgId on agentic surfaces

Anti-patterns:
- *"Spike: clean up the codebase"* — too open. A Spike has a question
  and an exit criterion.
- *"Spike: implement X"* — that's a Feature. A Spike's output is
  learning, not shipped code.

**Linkage**: Spikes do not link to requirements. The thing a Spike
unblocks (a Feature, a Bug, a future Spike) usually exists as its own
PBI.

**Splitting**: along the questions being investigated. If a Spike
covers three distinct unknowns, make it three Spikes.

**Status flow**: full lifecycle. The Done state means "the question is
answered" or "the timebox expired" — not "the underlying capability
shipped".

## When to ask vs infer

The skill should infer the type from the work description and propose
it. Ask the PM only when:

- The work is genuinely ambiguous between Feature and Bug (e.g. a
  capability that the team intended to ship but didn't fully).
- The work mixes types ("we need to add X but also fix Y, and clean up
  Z") — in that case offer to file separate PBIs and ask which is which.

Otherwise, propose the type along with the title in the confirmation
summary, and let the PM correct if needed.
