<!-- Appended by the `speckit-skald-sync` preset (strategy: append). -->

## Additional step (speckit-skald-sync preset): track delivery in Skald

Skald tracks reality. Three trigger points during implementation, all via the Skald MCP
tools, all resolved from the PB-x / requirement map in `tasks.md` and the spec's `skald:`
frontmatter. If the link is missing, skip with a note — never block.

1. **Implementation starts** (first task begins): propose `updateBacklogItem` status →
   `In Progress`.
2. **A user-story checkpoint passes** — all its tasks done AND the local quality gates for it
   are green: propose `updateRequirement` status → `Delivered` for the requirement(s) mapped
   to that story in `tasks.md`. The PBI stays `In Progress`.
3. **The feature completes** — every story checkpoint passed and the full local verification
   gate (the final phase of `tasks.md`, e.g. `scripts/gate.sh`) is green: propose
   `updateBacklogItem` status → `Done` with a short outcome note (what shipped, deviations,
   gate results), and flip any remaining in-scope linked requirements to `Delivered`.
   Out-of-scope linked requirements are listed but left alone.
4. **After Done**: `listReleases` + `listPbis` — if this was the last open PBI in its
   release, recommend `/speckit-release-planning` to close the release out (don't flip the
   release yourself).

**Honesty rule**: NEVER propose `Done`/`Delivered` if any mandatory gate was skipped or is
failing — report the true state instead. The write-back is a consequence of a green gate,
not a substitute for one.

Discipline: read before write (`getBacklogItemDetail`), ONE confirmation per write, no
deletes. Skald unreachable → leave a `TODO(skald-sync)` marker in `tasks.md` so the sync
happens next session.
