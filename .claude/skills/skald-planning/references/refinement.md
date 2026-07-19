# Backlog refinement and PBI splitting

Refinement is the conversation that turns a half-formed PBI into
something the team can pick up. In Skald the agent's job during
refinement is to **make the existing state visible** and **surface gaps
and split opportunities** — not to invent answers or move the work
forward without the PM.

## The refinement flow

When the PM says *"let's refine this"* (or you spot a PBI that needs
it), run this loop:

### 1. Read what's already there

Surface everything the PBI is currently anchored to:

- The PBI itself: title, type, status, estimate, team, release.
- Linked requirements: `listRequirements` filtered, then `listRules`
  and `listExamples` per requirement.
- Any open questions on those requirements: `listOpenQuestions` per
  linked requirement.
- Related PBIs: same domain, same release, same team. Use
  `getExistingBacklogItems` (it reports each item's `releaseName` and
  team) and `listReleases` to group by release.

Read this state aloud to the PM in plain language. The act of reading
the current state often surfaces the next question by itself.

### 2. Spot the gaps

Compare what's there against what a refined PBI looks like:

- **Feature PBIs** should ideally have at least one linked requirement
  with rules and at least one example per rule before moving to Ready
  for Planning. If linked requirements are missing or under-specified,
  offer to run the Specification by Example workshop (see the
  skald-requirements skill).
- **Bug PBIs** need clear reproduction steps and an expected vs.
  actual outcome. If those are missing, ask for them — but don't
  invent them.
- **Spike PBIs** need a precise question and an exit criterion.
  *"Investigate caching"* is not specific enough. *"Can the search
  index reconcile a 100k-row drift in under 30 s?"* is.

Surface every gap once and let the PM choose what to address.

### 3. Suggest splits when the PBI is too big

Apply the splitting heuristic (next section). If the PBI looks
splittable, propose the new PBI titles out loud and let the PM
decide. Splits are a write — the agent doesn't create them without
explicit confirmation.

### 4. Offer linkage

If the PBI is a Feature and doesn't yet link to any requirements,
search for plausible candidates via `listRequirements` and surface
them. If the right requirement doesn't exist, offer to create one
via the skald-requirements skill before moving on.

### 5. Capture follow-ups

The refinement conversation often surfaces things that don't belong
on the current PBI:

- A separate Bug noticed mid-refinement → propose a new Bug PBI.
- A Spike that should precede the Feature → propose a new Spike PBI and
  suggest the Feature stays at Refining Requirements until the Spike
  closes.
- A missing requirement that's bigger than the current PBI →
  propose creating it as a new requirement plus a Feature PBI.

Don't bundle everything into the current PBI to "save time". A clean
backlog with several focused PBIs is much easier to plan than one
mega-PBI.

## The splitting heuristic

For PBIs the test is simpler than the INVEST/Lawrence framework used
for requirements:

> **Each split must be a valuable increment from the user's
> perspective.**

A team should be able to deliver a set of PBIs within a short time
period and have each PBI be a real step forward for the user.

### Splittable signals

- The PBI mentions multiple distinct user-facing capabilities.
- The PBI bundles a happy path with edge cases that could ship later.
- The PBI bundles a v1 of a feature with polish that could ship later.
- The PBI is XL — splitting usually finds two L's or an M and an L.

### Not splittable signals

- All pieces of the PBI must be present for any user value to land
  (e.g. a checkout flow where shipping just the cart leaves users
  stuck).
- The PBI is a Bug. Bugs that look splittable are usually two Bugs —
  file separately rather than splitting one.

### Anti-patterns: bad splits

- **Horizontal slices**: *"build the backend" + "build the frontend"*.
  Neither half delivers user value. This is the most common bad
  split — watch for it.
- **Vague follow-ups**: *"v1" + "polish"* with no specifics about what
  "polish" means. The polish PBI never ships because it's not
  scoped.
- **Unrelated bundles**: *"the share link feature" + "a bug we noticed
  while testing it"*. File the bug separately.

### Good split examples (Skald)

Original PBI: *"Public share link for a backlog item (XL)"*.

Good split:

- *"Share-link generation and storage"* (M) — the PM can mint a
  link; nothing renders yet but the substrate is in place.
- *"Public read-only view of a shared PBI"* (M) — the link
  resolves and renders the PBI for stakeholders.
- *"Share-link expiration"* (S) — links auto-expire after the
  configured window.
- *"Share-link revocation"* (S) — the PM can manually invalidate a
  link.

Each piece is a real step forward; the PM can ship 1+2 and have a
useful capability even if 3 and 4 ship later.

Bad split of the same PBI:

- *"Database schema and API for share links"* (M) — no user value.
- *"UI for share links"* (L) — depends entirely on the first; can't
  ship in isolation.

## Mechanically, in Skald

After the PM agrees to a split:

1. Confirm the new PBI titles, types, and what gets linked where.
2. Create the new PBIs (`createBacklogItems`) — usually as Refining
   Requirements unless they're obviously ready.
3. If linked requirements migrate or get re-attributed, use
   `linkRequirementToBacklogItem` / `unlinkRequirementFromBacklogItem`
   to redo the trace.
4. Update the original PBI's status. Common outcomes:
   - The original PBI becomes one of the splits (most common). Update
     its title and rescope.
   - The original PBI is archived because all of its work moved to
     the splits.
