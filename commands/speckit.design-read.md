---
description: Intake a returned design handoff — drift-check against the repo design system, scope-check against the spec, write questions back to Skald (speckit-skald-sync preset command).
---

# Design Intake (`/speckit-design-read`)

Runs when a Claude Design handoff lands in `specs/NNN-name/design/`, before `/speckit-plan`.

**Authority rule: the repo's design system always wins.** The handoff is reference material,
never production code — prototype code is never copied into the app source tree.

## Flow

1. Read the handoff files in `specs/NNN-name/design/` and the repo's design-system source of
   truth (tokens, primitives).
2. **Drift check**: every colour, font, spacing value, or bespoke component in the handoff
   that deviates from the repo's tokens/primitives is FLAGGED with its repo-side equivalent
   — not adopted. Produce the mapping table (handoff value → repo token/component).
3. **Scope check**: handoff screens/affordances not covered by the spec are surfaced,
   not silently absorbed. For each, the PM decides: into the spec (re-run the
   `/speckit-specify` sync so Skald follows), a follow-up PBI, or dropped.
4. **Question writeback**: product decisions the handoff surfaced (states the design
   invented, flows it changed) → propose `mcp__skald__createOpenQuestions` on the owning
   requirement, one per confirmation.
5. Write the intake summary (mapping table + scope findings + question ids) into
   `specs/NNN-name/design/intake.md` — this is design input for `/speckit-plan`, which keeps
   making the technical decisions.

Discipline: ONE confirmation per Skald write; no deletes. Skald MCP unavailable → complete
the local intake and mark the unsynced questions `TODO(skald-sync)` in `intake.md`.
