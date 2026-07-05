---
description: Discuss and record the domain's entities, processes, and terms in Skald — feature mode (from the active spec) or standalone domain discovery (speckit-skald-sync preset command).
---

# Domain Knowledge (`/speckit-domain-knowledge`)

Build and maintain the project's domain tree and ubiquitous language in Skald. Methodology:
follow the `skald-domain-management` skill — lightly DDD-inspired, deliberately simpler.
Hard rules: max 3 levels of domain nesting; every glossary term is classified as **Entity**,
**Process**, or **Term**; per-domain title uniqueness; run `getDomainImpact` before any
rename or restructure proposal.

## Mode selection

- **Feature mode** — an active spec exists (resolve the feature directory the same way the
  core commands do): work the spec's language. Position: after `/speckit-specify`, before
  `/speckit-clarify`, so clarification happens in settled terms.
- **Standalone mode** — no active spec or the PM asks for general domain work: open-ended
  discovery across the project.

## Flow

1. **Read first**: `mcp__skald__listDomains` (the tree) and `listDomainTerms` per relevant
   domain.
2. **Feature mode**: extract candidate entities, processes, and terms from the spec's nouns
   and verbs. Diff against the glossary. Discuss with the PM: which are genuinely part of the
   ubiquitous language, which domain owns each, is any existing term's definition contradicted
   by the spec?
3. **Standalone mode**: walk the tree with the PM — pick an area, discuss its entities /
   processes / terms, identify missing sub-domains (nest only when a domain genuinely owns
   distinct language; otherwise keep flat).
4. **Write** (each individually confirmed): `createDomain` / `updateDomain` for structure,
   `createTerm` / `updateTerm` for glossary entries (title, Entity/Process/Term type,
   definition).
5. **Feature mode close**: where the discussion settled a better word, update the spec's
   terminology to match the glossary — the spec follows the glossary, not the other way
   around. Then continue to `/speckit-clarify`.

## Discipline

Read before write; ONE confirmation per write; no deletes (restructures are
create-then-move proposals, checked with `getDomainImpact` first). Skald MCP unavailable →
in feature mode leave a `TODO(skald-sync)` note in the spec and continue the flow; in
standalone mode stop.
