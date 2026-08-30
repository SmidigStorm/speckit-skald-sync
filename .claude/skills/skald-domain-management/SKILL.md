---
name: skald-domain-management
description: 'How to model a product''s domain in Skald — the domain hierarchy, and the ubiquitous language as Entities, Processes and Terms. DDD-inspired but simpler: a clear tree of domains, each holding the glossary it owns. Covers the 3-level depth cap, when to nest vs keep flat, Entity/Process/Term classification, term aliases, per-domain title uniqueness, reparenting, and domain-impact checks before renames or moves. Use whenever the user is creating or organising domains, building a glossary, classifying a term, deciding where a term or requirement belongs, or restructuring the domain tree — even if they don''t say "Skald". Also trigger on "create a domain", "model my domains", "add a domain", "model this area", "what''s the ubiquitous language", "is this an entity or a process", "where does this term go", "move this domain", "rename this bounded context", or any Skald domain tool call (createDomain, updateDomain, listDomains, getDomainImpact, term tools).'
---

# Skald Domain Management

> **This is reference for you, not a script to read out.** The user asked a
> question, not for the manual. Answer it in a few points — three to five, the
> ones that matter for what they actually asked — then ask which part they want
> to go into. Never deliver a section-by-section tour of this document. If you
> find yourself writing headings, you are reciting: stop and ask instead.


Skald's Domain module captures **what the product is about** as a tree of
domains, each owning a glossary of Entities, Processes, and Terms. It's
lightly inspired by Domain-Driven Design — domains play the role of
bounded contexts — but it's deliberately simpler. Don't drag in the full
DDD apparatus (aggregates, repositories, value objects). The job is: a
clear hierarchy, and the ubiquitous language captured under it.

Vocabulary note: in descriptions, refer to the **product**, not the
"project". A Skald workspace holds one product; the domain tree models
that product. In the app this whole surface is the **"Domains &
Glossary"** page — use that name when pointing the PM at it.

## The domain hierarchy

Domains form a tree. Each row has a `parentId`, a `depth`, and a
`description`.

- **Depth is capped at 3 levels** — depth 0 (top-level), depth 1, and
  depth 2. You cannot create a child under a depth-2 domain; the core
  rejects it. Plan the tree with that ceiling in mind.
- **Top-level domains** are the major areas of the product
  (e.g. Requirements, Planning, Goals, Architecture).
- **Sub-domains** narrow a top-level area (e.g. under Requirements:
  Rules, Examples, Open Questions).

### When to nest vs keep flat

- **Nest** when a sub-area has its own distinct vocabulary and a handful
  of terms or requirements that clearly belong only to it. Sub-domains
  earn their place by giving terms a more specific home.
- **Keep flat** when you'd be creating a sub-domain with one term and no
  distinct language — that's over-modelling. A term can live directly on
  a top-level domain.
- **Watch the 3-level cap.** If you find yourself wanting a 4th level,
  the tree is too deep — flatten a layer or move the distinction into
  the term's description instead.

### Reparenting is chat-only — and moves the whole subtree

`updateDomain` can rename, re-describe, and **reparent** (PB-26): pass
`parentId` with a domain UUID to move under it, or `null` to move to
top level; omit it to keep the current parent. A reparent moves the
domain **and its entire subtree** — depths are recomputed, the 3-level
cap is enforced across the whole moved subtree, and cycles are rejected
(a domain can never move under itself or its own descendant). The UI
deliberately has no reparent control — this is an agent/MCP capability.

Before proposing a reparent, run `getDomainImpact` and include the
counts in the proposal. **Archival is still UI-only** — decline and
point the PM at the Domains & Glossary page; never simulate an archive
or fake a move by recreating the domain (that orphans its terms and
requirements).

## Entities, Processes, and Terms

Every glossary term has one of three types. Getting the type right is the
core skill here — it's what makes the glossary a real model rather than a
word list.

- **Entity** — a *thing with identity* that exists in the product. You
  can point at one, list them, give one a name. Requirement, Backlog
  Item, Objective, Team, User, Vision.
- **Process** — an *activity or flow that happens*. A verb made noun.
  Backlog Refinement, Prioritization, Pseudonymization, Reconciliation,
  Requirement Splitting.
- **Term** — a *concept, framework, or rule* that isn't a thing you
  point at or an activity you run. EARS, MoSCoW, Ubiquitous Language,
  Bounded Context, Agent–MCP Parity.

Full classification guidance with a decision procedure and many worked
examples: [`references/entities-processes-terms.md`](references/entities-processes-terms.md).

Quick decision procedure:

1. **Can you have several of them, each with its own identity?** →
   Entity. ("three Requirements", "two Teams".)
2. **Is it something the product or team *does* over time?** → Process.
3. **Otherwise** — a concept, principle, acronym, or rule → Term.

When unsure between Entity and Term, ask: *does the product store rows
of these?* If yes, Entity. If it's an idea about how things work, Term.

## Building a glossary

A discovery session is Skald's **Map your requirements into domains**
journey. This skill is how to do the modelling well; how to conduct that
journey — announcing its shape, drawing the sketch, and why it has no
progress tracker — is in **skald-journeys**. Load it when one starts.

For such a session ("let's model the domain"), seed it before asking
anything: read `getExistingProjectVision` and
`listProjectUsers` first — the top-level areas of the tree usually fall
straight out of the vision's problem space and the users' goals. Walk
the product area by area from there.

Terms also arrive **from requirement sessions**: a workshop or mapping
session (skald-requirements) that coins new language, or finds no
domain for a requirement, hands off here. Capture such terms the same
way — a batch surfaced mid-session can go in one proposal.

The workflow when capturing domain knowledge:

1. **Find the home domain.** `listDomains` to see the tree. The term
   belongs under the most specific domain that fits. If no domain fits,
   the missing domain is the real first step — propose creating it.
2. **Check for duplicates.** `listDomainTerms` and filter to the target
   domain. Term titles are unique per `(org, product, domain)` — the
   same word can exist in two different domains, but not twice in one.
3. **Classify.** Entity / Process / Term per the procedure above.
   Propose the type; let the PM correct.
4. **Ask for an alias.** *"Does the team have another name for this?"*
   A term can carry one `alias` ("also known as" — e.g. SKU → "Stock
   Keeping Unit"). It's searchable in the glossary and not subject to
   per-domain title uniqueness. Capture it when it exists; don't invent
   one.
5. **Write a solution-free description.** One or two sentences about
   what the concept *is* in this product, not how it's implemented.
   Reuse existing glossary vocabulary so the model stays consistent.
6. **Confirm, then `createTerm`.**

## Before renaming or restructuring

Domain edits ripple. A rename changes how the domain reads everywhere it
appears; a restructure can strand terms and requirements.

**Always call `getDomainImpact` before proposing a rename or
restructure.** It reports how many child domains and how many
requirements hang off the domain. Surface that count to the PM so they
make the call with eyes open: *"Architecture has 2 sub-domains and 6
requirements linked — renaming it will touch all of those. Proceed?"*.

## Check for duplicates before creating

Before `createDomain` or `createTerm`, read the existing tree first —
`listDomains` for domains, `listDomainTerms` for glossary (the Building a
glossary step above already calls this out for terms). A domain whose
name/scope already exists, or a term already present in the target domain,
is a duplicate. Surface it to the PM and ask whether to reuse or extend
rather than adding a second. **Never create blind.**

## Confirmation discipline

Every write tool here is preceded by a plain-language summary (a
proposal) and the PM's go-ahead — a plain "yes" to the proposal
suffices. One confirmation covers the whole proposal it answers.
Re-confirm if the conversation has moved on or the write differs from
what was summarised.

For domain renames / restructures specifically: run `getDomainImpact`
first and include the impact counts in the summary.

This matches the discipline both agentic surfaces enforce. Restated
here because this skill is shared: the in-app Skald Agent and external
MCP clients load this same document.

## Tool catalogue

Full list with when-to-use: [`references/tool-catalogue.md`](references/tool-catalogue.md).

Quick reference:

**Reads** (no confirmation needed):
- `listDomains({ projectId })` — the full domain tree (flat array with
  parentId + depth).
- `listDomainTerms({ projectId })` — every glossary term with its type
  and home domain.
- `getDomainImpact({ projectId, domainId })` — child + requirement
  counts; run before any rename/restructure.

**Writes** (confirmation discipline applies):
- `createDomain({ projectId, name, parentId?, description? })` — omit
  `parentId` for a top-level domain. Rejects a child under a depth-2
  parent.
- `updateDomain({ projectId, id, name?, description?, parentId? })` —
  rename, re-describe, or reparent. `parentId` is tri-state: uuid moves
  the domain + subtree under that parent, `null` moves it to top level,
  omitted keeps the current parent. No archive (UI-only).
- `createTerm({ projectId, domainId, title, type, alias?, description? })`
  — type is Entity / Process / Term (defaults to Term); `alias` is the
  optional "also known as" name.
- `updateTerm({ projectId, id, title, type, domainId, alias?, description? })`
  — can also move a term to a different home domain, change its type,
  or set/clear the alias (tri-state: string sets, `null` clears,
  omitted keeps).

**Adjacent reads**:
- `listRequirements({ projectId })` — see what's linked to a domain
  before restructuring.
- `listProjects()` — when you don't already have a `projectId`.

## Draw it

In the app's agent workspace you have a canvas. When you explain how domains nest,
or what an **Entity**, a **Process** and a **Term** each are, call
`showCanvasVisual` with `visual: "domain-tree"`.

Not this visual: the shape of the whole product — vision through to a buildable
item — is `product-development-flow`.
