<!-- Appended by the `speckit-skald-sync` preset (strategy: append). -->

## Additional step (speckit-skald-sync preset): Skald drift report (read-only)

As part of the analysis report, include a **two-way drift table** between the spec artefacts
and Skald. Read the `skald:` link from `spec.md` frontmatter; if absent, note it and skip.

1. Read the Skald side: `getBacklogItemDetail` (PBI + linked requirements), plus `listRules`
   / `listExamples` / `listOpenQuestions` for those requirements.
2. Compare against the spec's user stories, functional requirements, and acceptance
   scenarios. Report:
   - **In Skald, missing or contradicted in the spec** — the spec is incomplete or stale.
   - **In the spec, not in Skald** — Skald is stale (requirement/rule/example/question never
     synced).
   - **Status mismatches** — e.g. PBI `Refining Requirements` with a fully clarified spec, or
     `Approved` requirements the spec has since changed.
3. **This step writes NOTHING.** For each drift row, name the fix: re-run the
   `/speckit-specify` or `/speckit-clarify` sync, or take the noted manual action.

Skald unreachable → note it in the report and skip. Never block analyze.
