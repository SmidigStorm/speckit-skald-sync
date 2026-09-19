# Tool catalogue for the Skald Domain Management skill

The tools for modelling the domain tree and its glossary. Every
project-scoped tool takes a `projectId` — call `listProjects` first if
you don't have one. Names are identical on both agentic surfaces per
Constitution V (Agent–MCP parity).

## Read tools (no confirmation needed)

- **`listDomains({ projectId })`** — the full domain hierarchy as a flat
  array; each row carries `parentId`, `depth`, and `description`. Your
  first call when placing a term or planning a new domain.
- **`listDomainTerms({ projectId })`** — every glossary term with its
  title, type (Entity/Process/Term), description, and home domain. Read
  before creating a term to avoid duplicates (titles are unique per
  domain) and to reuse existing vocabulary.
- **`getDomainImpact({ projectId, domainId })`** — how many child
  domains and how many requirements are attached to a domain. **Run this
  before proposing any rename or restructure** so the PM sees the blast
  radius.

## Write tools (Writing rules apply)

A write the PM asked for happens straight away, followed by a summary of
what was written. Propose first, and write only after a go-ahead, for
content you drafted rather than the PM, or an archive, restore or
destructive write (the skill's
Writing section).

### Domains

- **`createDomain({ projectId, name, parentId?, description? })`** — omit
  `parentId` for a top-level (depth 0) domain; pass it to nest. The core
  **rejects a child under a depth-2 parent** (3-level cap). Description
  should say what the domain covers, in product vocabulary.
- **`updateDomain({ projectId, id, name?, description?, parentId? })`** —
  rename, re-describe, or **reparent** (PB-26). `parentId` is tri-state:
  a domain UUID (same project) moves this domain **and its whole
  subtree** under that parent; `null` moves it to top level; omitted
  keeps the current parent. Depths are recomputed, the 3-level cap is
  enforced across the moved subtree, and cycles are rejected. It does
  **not** archive — that's UI-only. Don't recreate a domain to fake
  anything; that strands its terms and requirements. Run
  `getDomainImpact` first when renaming or reparenting and put the
  counts in the proposal.

### Glossary terms

- **`createTerm({ projectId, domainId, title, type, alias?, description? })`** —
  `type` is `Entity` / `Process` / `Term` (defaults to `Term` if
  omitted, but propose the right type explicitly). `alias` is the
  optional single "also known as" name (e.g. SKU → "Stock Keeping
  Unit") — searchable, not subject to per-domain title uniqueness.
  Title must be unique within the target domain — `listDomainTerms`
  and filter first.
- **`updateTerm({ projectId, id, title, type, domainId, alias?, description? })`**
  — rename, re-describe, **change the type**, **move the term to a
  different home domain** (pass a different `domainId`), or set/clear
  the alias (tri-state: a string sets it, `null` clears it, omitted
  keeps it). Changing type or home domain is a material edit — surface
  it explicitly in the confirmation summary.

## Adjacent reads

- **`listRequirements({ projectId })`** — see which requirements are
  linked to a domain before restructuring; complements
  `getDomainImpact`.
- **`listProjects()`** — when you don't already have a `projectId`.

## What you will NOT find in this catalogue

- **No domain archive/delete via chat.** UI only (the Domains & Glossary
  page has the delete-with-impact dialog). Skald exposes no hard delete
  via chat by design.
- **No term archive/delete via chat.** If the PM wants to retire a term,
  that's a UI operation; don't simulate it via `updateTerm`.

(Domain **reparenting**, formerly listed here, is now supported via
`updateDomain.parentId` — see above. PB-26.)
