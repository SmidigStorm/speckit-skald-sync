<!-- Appended by the `speckit-skald-sync` preset (strategy: append). -->

## Additional step (speckit-skald-sync preset): sync clarifications into Skald

After the clarification session ends and answers are encoded into `spec.md`, sync via the
Skald MCP tools. Read the `skald:` link from `spec.md` frontmatter; if absent, recommend
re-running `/speckit-specify` and skip.

1. **Open questions**: `listOpenQuestions` for the linked requirements. For each question
   this session answered → propose `answerOpenQuestion`. Genuinely new unresolved questions
   → propose `createOpenQuestions`.
2. **Rules & examples**: where an answer changes an agreed behaviour, propose the matching
   `updateRule` / `updateExample` (or `createRules` / `createExamples` for newly surfaced
   behaviour) so the requirement, not just the spec, reflects the decision.
3. **Approval transition**: if the spec now has ZERO `[NEEDS CLARIFICATION]` markers and the
   PM confirms the spec is approved, propose: each linked requirement `Draft` → `Approved`
   (via `updateRequirement`), then the PBI `Refining Requirements` → `Ready for Planning`
   (via `updateBacklogItem`).
4. Update the spec's "Skald traceability" line if links changed.
5. **Recommendation** (say, don't do): if the feature has a real user-facing surface beyond
   trivial reuse of existing screens, recommend `/speckit-design-brief` before
   `/speckit-plan`.

Discipline: read before write, ONE confirmation per write, no deletes. Skald unreachable →
skip with a note + `TODO(skald-sync)` marker in `spec.md`; never block clarify.
