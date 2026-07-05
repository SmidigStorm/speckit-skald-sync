---
description: Generate a paste-ready Claude Design brief for the current feature (speckit-skald-sync preset command).
---

# Design Brief (`/speckit-design-brief`)

For design-heavy features: generate a brief the PM pastes into Claude Design
(claude.ai/design). Position: after `/speckit-clarify` (requirements settled), before
`/speckit-plan`. OPTIONAL — skip for features with no meaningful UI surface.

## Flow

1. Resolve the active feature directory and read `spec.md` (+ clarifications).
2. Compose the brief into `specs/NNN-name/design/design-brief.md`:
   - The feature's user stories and the screens/states they imply (include empty, loading,
     and error states).
   - Component inventory hints: what the repo's design system already provides vs what the
     design must define.
   - Token constraints: the design must use the repo's design tokens / color system — name
     the token families, never raw hex.
   - Explicit out-of-scope list, so the returned handoff doesn't invent features.
3. **Open questions**: unresolved UX decisions the brief exposes (flows the spec doesn't
   settle, conflicting states) → propose `mcp__skald__createOpenQuestions` on the owning
   requirement, one per confirmation.
4. Tell the PM: paste the brief into Claude Design; drop the returned files into
   `specs/NNN-name/design/`; then run `/speckit-design-read` before `/speckit-plan`.

No other Skald writes in this command. Skald MCP unavailable → still generate the brief;
note the unsynced questions with a `TODO(skald-sync)` line in the brief.
