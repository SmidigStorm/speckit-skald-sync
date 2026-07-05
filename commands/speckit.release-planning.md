---
description: Discuss and manage Skald releases — contents, dates, status transitions, moving PBIs (speckit-skald-sync preset command).
---

# Release Planning (`/speckit-release-planning`)

Project-level, on demand. The release lifecycle the feature flow only touches at assignment
time (`/speckit-specify`). Also the recommended follow-up when `/speckit-implement` closes
the last open PBI in a release.

## Flow

1. **Read first**: `mcp__skald__listReleases` (status, target date, item/done counts) and
   `listPbis` to see each release's contents and the unassigned pool.
2. **Review**: walk the releases with the PM — what's in each, what's done, what's overdue
   (target date passed with open items), what's unassigned but should be bundled.
3. **Manage** (each individually confirmed):
   - `createReleases` — new release (name + optional target date).
   - `updateRelease` — rename, retarget, and status transitions `Planned` → `In Progress` →
     `Released`. Propose `Released` only when every item in it is Done; state the exception
     explicitly if the PM wants to release with open items (they move out first).
   - `updateBacklogItem` `releaseId` — move a PBI between releases or out of one. One PBI
     per confirmation.
4. Archive a release (`updateRelease` archive / status per the tool's contract) only on
   explicit request, never as a side effect.

## Discipline

Read before write; ONE confirmation per write; no deletes. The PM decides what ships when —
this command organises and proposes, it never reprioritises on its own. Skald MCP
unavailable → stop.
