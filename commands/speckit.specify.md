<!-- Appended by the `speckit-skald-sync` preset (strategy: append). -->

## Additional step (speckit-skald-sync preset): sync the spec into Skald

Skald is the source of truth; the spec is a derived view. **One spec folder = exactly ONE
Skald backlog item (PBI).** After `spec.md` is written and the normal completion is reported,
run this sync via the Skald MCP tools (`mcp__skald__*`).

**Establish the Skald link.**

1. Look for a `skald:` block in `spec.md` frontmatter (`projectId`, `pbiId`). If present, this
   is a re-run — update, don't re-create.
2. If absent, resolve the project via `mcp__skald__listProjects` (ask the PM if ambiguous),
   then resolve the PBI: check `getExistingBacklogItems` / `listPbis` for an existing item
   matching this feature (match on a spec-folder reference in the description first, then
   title). Only if none exists, propose creating ONE: `createBacklogItems`, type `Feature`,
   status `Refining Requirements`, title = feature name, description = spec summary + branch.
3. Write the link into `spec.md` frontmatter and a human-readable **"Skald traceability"**
   line (PB-x + requirement ids, kept current by later steps):

   ```yaml
   skald:
     project: "<name>"
     projectId: "<uuid>"
     pbi: "PB-73"
     pbiId: "<uuid>"
   ```

4. Offer to append the feature path (`specs/NNN-name/`) to the PBI description for
   back-navigation.

**Requirements: adopt before creating.**

5. Read the Skald side: `getBacklogItemDetail`, `listRequirements`, `listRules`,
   `listExamples`, `listOpenQuestions`. When the spec covers ground an existing requirement
   already describes, ADOPT it (propose `updateRequirement` if the spec refines its wording)
   — never duplicate. Only create genuinely new ones: `createRequirements`, status `Draft`,
   one per user story / coherent FR cluster, plus `createRules` (from FRs) and
   `createExamples` (Gherkin, from acceptance scenarios) where valuable. Follow the
   `skald-requirements` skill for title shapes and Gherkin conventions.
6. Link every adopted or created requirement to the PBI via `linkRequirementToBacklogItem`.
7. Every `[NEEDS CLARIFICATION]` marker → propose `createOpenQuestions` on the owning
   requirement.
8. Report drift in BOTH directions: Skald content missing/contradicted in the spec is
   surfaced for the PM (never silently edit the spec); spec content missing in Skald becomes
   the create/adopt proposals above.

**Release and goal.**

9. If the PBI has no release: `listReleases`, suggest the best-fitting active one (name,
   target date, what its other PBIs cover); if none fits, propose `createReleases` (name +
   optional target date agreed with the PM); then `updateBacklogItem` `releaseId`.
10. Goal link: `listGoals` for the active time period and ask whether this PBI serves one of
    the key results — if yes, `linkKeyResultToBacklogItem`. If the PM names a goal that
    doesn't exist yet, recommend `/speckit-goals`.

**Wrap up.**

11. Approval shortcut: if the spec has ZERO `[NEEDS CLARIFICATION]` markers and the PM
    explicitly approves it now, additionally propose linked requirements `Draft` → `Approved`
    and the PBI → `Ready for Planning`. Otherwise leave that to `/speckit-clarify`.
12. Recommendations (say, don't do): if the spec introduces ≥3 terms not in the glossary
    (`listDomainTerms`) → recommend `/speckit-domain-knowledge`; if the project has no vision
    (`getExistingProjectVision`) or a user story names an actor missing from
    `listProjectUsers` → recommend `/speckit-specify-vision`.

Discipline for every write: read before write, summarise the change in plain language, ONE
explicit PM confirmation per write (never chain off a single "yes"), no deletes. If the PM
says this spec has no Skald PBI, or Skald MCP is unreachable, skip with a one-line note and
leave a `TODO(skald-sync)` line under "Skald traceability" — never block specify.
