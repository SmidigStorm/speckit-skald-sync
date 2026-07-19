# Tool catalogue for the Skald Planning skill

The tools you'll touch when working on PBIs, grouped by purpose. Every
project-scoped tool takes a `projectId` argument — call `listProjects`
first if you don't have one. Names are identical on both agentic
surfaces (in-app Skald Agent and Skald MCP server) per Constitution V
(Agent–MCP parity).

## Read tools (no confirmation needed)

### Primary

- **`listProjects()`** — every project in the org. Always the first
  call when you don't know which project to act in.
- **`listPbis({ projectId })`** — every PBI in the project with id,
  displayId, title, status, and teamId. The lean list view.
- **`getExistingBacklogItems({ projectId })`** — richer per-item shape:
  id, displayId, title, status, estimate, teamName, `releaseName` (the
  assigned first-class release, or null), and requirementCount. Use when
  reviewing the backlog for gaps/duplicates or to see which release an
  item is in.
- **`getBacklogItemDetail({ projectId, backlogItemId })`** — full
  single-PBI read incl. description, linked requirements, and the
  assigned release (`releaseId` + `releaseName`). Use to confirm a
  release assignment after `updateBacklogItem({ releaseId })`.
- **`getTeams({ projectId })`** — every team that can own a PBI in
  this project.
- **`listReleases({ projectId, includeArchived? })`** — every release
  in the project with `id`, `name`, `status`
  (Planned/In Progress/Released), `targetDate`, `archived`, and how many
  backlog items it holds (`itemCount`) and how many are done
  (`doneCount`). Pass `includeArchived: true` to also see archived
  releases — needed to find an archived release's `id` before restoring
  it.

### Adjacent reads worth doing during refinement

- **`listRequirements({ projectId })`** — refinement context for
  Features, candidate linkages for new PBIs.
- **`listRules({ requirementId })`** and **`listExamples({ requirementId })`**
  — read the acceptance bar of a linked requirement before estimating
  or moving status.
- **`listOpenQuestions({ requirementId })`** — surface unresolved
  questions on linked requirements; a PBI usually shouldn't move to
  Ready for Planning while open questions are unanswered.
- **`listDomains({ projectId })`** — context for what part of the
  product the PBI sits in.
- **`listDomainTerms({ projectId })`** — use the project's existing
  vocabulary so the PBI title reads consistently.
- **`listGoals({ projectId })`** — surface Objectives and Key Results
  the PBI might support. The PM may want to link via
  `linkKeyResultToBacklogItem`.

## Write tools (confirmation discipline applies)

Every call here is preceded by a plain-language summary (a proposal) and
the PM's go-ahead. One confirmation covers the whole proposal it answers.
If the conversation has moved on between the summary and the call, or the
write differs from what was summarised, re-confirm.

### The PBI itself

- **`createBacklogItems({ projectId, items: [...] })`** — create one or
  more PBIs. Each item carries `title`, optional `type`
  (Feature/Bug/Refactor/Spike, defaults to Feature), optional
  `description`, optional `estimate`, optional `teamId`.
  Default `status` is Refining Requirements.
- **`updateBacklogItem({ projectId, id, ... })`** — change title,
  description, status, estimate, `type` (Feature/Bug/Refactor/Spike),
  team, `sortOrder`, or `releaseId`. Status and type changes
  are write operations: the agent proposes, the PM confirms, the agent
  calls the tool. **`releaseId`** is the first-class release assignment
  (resolve the UUID via `listReleases`): a UUID assigns/moves the item
  to that release (single-assignment, replaces any prior one); `null`
  removes it from its release. The release and item must be in the same
  project (the tool verifies this). It is the **only** way to set a
  PBI's release — the legacy free-text `release` label was retired
  (spec 045).

### Releases

- **`createReleases({ projectId, releases: [...] })`** — create one or
  more releases. Each carries `name`, optional `targetDate`
  (YYYY-MM-DD), and optional `status` (defaults to Planned). Names must
  be unique within the project (active releases) and within the batch;
  a duplicate rejects the whole batch — nothing is partially created.
- **`updateRelease({ projectId, id, ... })`** — rename a release, change
  its `targetDate` or `status`, or archive/restore it. `archived: true`
  archives the release **and unassigns all its backlog items**;
  `archived: false` restores it (items are not re-added). There is no
  delete — archive is the destructive path.

### Linkage

- **`linkRequirementToBacklogItem({ projectId, requirementId, backlogItemId })`**
  — attach a requirement to a PBI. Many-to-many; one PBI can implement
  several requirements and one requirement can be delivered across
  several PBIs.
- **`unlinkRequirementFromBacklogItem({ projectId, requirementId, backlogItemId })`**
  — remove the trace.

### Goal-linkage adjacency

- **`linkKeyResultToBacklogItem({ projectId, keyResultId, backlogItemId })`**
  — surface during refinement when the PBI plausibly moves a KR.
- **`unlinkKeyResultFromBacklogItem(...)`** — the reverse.

These two live in the Goals skill's tool catalogue too. Mentioned here
because they're commonly used during PBI refinement.

## What you will NOT find in this catalogue

By design, Skald has no delete tools. Destructive operations go through
soft-archive instead, which preserves history. Don't simulate deletion
via `update*` — that breaks the audit trail.

Status backwards-flow (e.g. Done → In Progress) is supported by
`updateBacklogItem` but is uncommon. If you propose a backwards move,
say why in the confirmation summary — the PM will usually want to know
the reason (a regression discovered, a rollback, etc.).
