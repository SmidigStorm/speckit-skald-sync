<!-- Appended by the `speckit-skald-sync` preset (strategy: append). -->

## Additional step (speckit-skald-sync preset): sync the plan into Skald

After `plan.md` (and its design artifacts) are produced, sync via the Skald MCP tools. Read
the `skald:` link from `spec.md` frontmatter; if absent, recommend `/speckit-specify` and skip.

1. **Catch-up guard**: if the PBI is still `Refining Requirements` but the spec has no
   unresolved `[NEEDS CLARIFICATION]` markers, first propose the skipped approval
   transitions: linked requirements `Draft` → `Approved`, PBI → `Ready for Planning`.
2. **Open questions**: `listOpenQuestions`; propose `answerOpenQuestion` for questions
   planning resolved, `createOpenQuestions` for new product decisions that surfaced.
3. **Blocked requirements**: if an unanswered question genuinely prevents planning part of
   the feature, propose the affected requirement(s) → `Blocked` (and back to `Approved` once
   answered). The PBI itself stays `Ready for Planning`.
4. **Plan summary on the PBI**: propose an `updateBacklogItem` that appends a short summary
   to the END of the PBI description under a `## Plan (specs/NNN-name)` heading — 3–6 lines
   max: the approach in one sentence, key artefacts/modules touched, notable constraints or
   decisions. On a re-run, REPLACE the existing `## Plan (specs/NNN-name)` section instead of
   stacking a duplicate.
5. If `specs/NNN/design/` contains a handoff that was never intaken, recommend
   `/speckit-design-read` before implementation.

Discipline: read before write, ONE confirmation per write, no deletes. Skald unreachable →
skip with a note + `TODO(skald-sync)` marker in `plan.md`; never block plan.
