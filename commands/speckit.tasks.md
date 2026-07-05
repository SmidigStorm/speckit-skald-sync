<!-- Appended by the `speckit-skald-sync` preset (strategy: append). -->

## Additional step (speckit-skald-sync preset): annotate tasks with Skald traceability

The feature already has ONE PBI (from `/speckit-specify`). Task generation does NOT create
PBIs and does NOT change any Skald status. After `tasks.md` is created:

1. Resolve the PB-x id from the spec's `skald:` frontmatter / "Skald traceability" line. If
   missing, recommend `/speckit-specify` and skip — do not invent a PBI here.
2. **Annotate `tasks.md`**: write the PB-x id near the top, and map each user-story phase to
   its Skald requirement id (from the traceability line). This map is what lets
   `/speckit-implement` flip exactly the right requirement to `Delivered` per story — keep it
   accurate.
3. No writes to Skald in this step. If the task volume suggests the PBI is too big
   (e.g. >40 tasks or >6 phases), recommend `/speckit-backlog-refinement` to discuss a split
   — don't split anything yourself.

Skald unreachable → nothing to do (this step is local annotation). Never block tasks.
