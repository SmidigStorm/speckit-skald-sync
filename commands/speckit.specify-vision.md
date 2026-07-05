---
description: Draft or refine the Skald project Vision and User groups (speckit-skald-sync preset command).
---

# Project Vision & Users (`/speckit-specify-vision`)

Project-level, not per-feature: run once early, revisit when `/speckit-specify` flags a
missing vision or an unlisted actor. Interactive session on the product **Vision** and
**User groups** via the Skald MCP tools. Methodology: follow the `skald-strategy` skill —
the Vision is a short elevator pitch (Problem / Vision / Differentiation); assist the PM in
writing it, never force the template.

## Flow

1. **Read first**: `mcp__skald__listProjects` to resolve the project (ask if ambiguous), then
   `getExistingProjectVision` and `listProjectUsers`.
2. **Vision**: discuss and draft/refine the Markdown body with the PM — problem being solved,
   the change the product makes, what sets it apart. Write via `setProjectVision`. Only clear
   an existing vision (`clearProjectVision`) on explicit request.
3. **User groups**: for each kind of user the product serves, capture name + optional
   description, goals, and pains (Markdown) — `addProjectUser` for new ones,
   `updateProjectUser` to refine. Retire obsolete groups via `archiveProjectUser` only on
   explicit request.
4. **Close the loop**: if this session was triggered from a feature (an actor in a spec's
   user stories was missing), confirm the new user group covers that actor and point back to
   the feature flow.

## Discipline

Read before write; summarise each intended write; ONE explicit PM confirmation per write; no
deletes. Skald MCP unavailable → say so and stop; there is no local artefact to fall back to.
