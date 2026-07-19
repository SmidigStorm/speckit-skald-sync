# Tool catalogue for the Skald Goals skill

The tools you'll touch when working on OKRs, grouped by purpose. Every
project-scoped tool takes a `projectId` — call `listProjects` first if
you don't have one. Names are identical on both agentic surfaces per
Constitution V (Agent–MCP parity).

## Read tools (no confirmation needed)

- **`getTimePeriods({ projectId })`** — every time period in the
  project. Read before creating one so you don't duplicate (e.g. two
  "Q3 2026" rows).
- **`listGoals({ projectId })`** / **`getExistingGoals({ projectId })`**
  — the Objective + Key Result tree for the project. Read before adding
  an Objective or KR to avoid duplicates and to understand context.
- **`getCheckInHistory({ projectId, keyResultId })`** — the progress
  trail for one Key Result. Read before recording a new check-in so you
  can see the trajectory.

## Write tools (confirmation discipline applies)

Every call is preceded by a plain-language summary (a proposal) and the
PM's go-ahead. One confirmation covers the whole proposal it answers.

### Time periods

- **`createTimePeriod({ projectId, name, startDate, endDate })`** —
  `startDate` strictly before `endDate`, both `YYYY-MM-DD`. Name is
  unique per organization (e.g. "Q3 2026").
- **`updateTimePeriod({ projectId, ... })`** — rename or adjust dates.

### Objectives

- **`createObjective({ projectId, timePeriodId, title, description? })`**
  — the parent time period must already exist. Title is the qualitative
  statement; keep metrics out of it.
- **`updateObjective({ projectId, ... })`** — refine title or
  description. Use the description to record stretch-vs-committed
  framing if it matters.

### Key Results

- **`createKeyResult({ projectId, objectiveId, title, startValue, targetValue, unit? })`**
  — the parent objective must exist. `startValue` and `targetValue` are
  **strings** and must differ (the DB rejects equal values). **State
  all three numbers explicitly in the confirmation summary** before
  calling — start, target, unit.
- **`updateKeyResult({ projectId, ... })`** — adjust title, values, or
  unit.

### Check-ins

- **`createCheckIn({ projectId, keyResultId, value, note? })`** — record
  a point-in-time measured value (string, captured **exactly** as the
  PM states it — never round) plus an optional note. **No confidence
  field** — confidence, if wanted, goes in the note.

### Linking KRs to PBIs

- **`linkKeyResultToBacklogItem({ projectId, keyResultId, backlogItemId })`**
  — connect the outcome (KR) to the output (PBI). **Call
  `getExistingBacklogItems` first** to verify the PBI exists in the
  project. Idempotent if the link already exists.
- **`unlinkKeyResultFromBacklogItem({ projectId, keyResultId, backlogItemId })`**
  — remove the link.

## Adjacent reads

- **`getExistingBacklogItems({ projectId })`** — required before
  `linkKeyResultToBacklogItem`. Also useful when coaching the PM toward
  the outputs that would move a KR.
- **`listProjects()`** — when you don't already have a `projectId`.
- **`getExistingProjectVision({ projectId })`** — check that Objectives
  align with the product Vision (Perdoo common mistake #1: OKRs not
  aligned with strategy).

## What you will NOT find in this catalogue

No delete tools — Skald has none by design. There's no archive tool for
goals exposed via MCP either; if the PM wants to retire an Objective or
KR, that's a UI operation. Don't simulate deletion via `update*`.
