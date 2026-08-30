---
name: skald-requirements
description: 'How to write, refine, split and manage requirements in Skald — the living specs in Spec-Driven Development. Covers user-story and EARS title shapes, the Example Mapping workshop (story → rules → examples → questions; Specification by Example), Gherkin without backgrounds or scenario outlines, MoSCoW prioritization, INVEST-based splitting, the no-solutions guideline, and the Open Questions discipline. Use whenever the user is creating, editing, splitting or refining a requirement; running an example-mapping / spec-by-example workshop; writing rules or examples; or surfacing open questions — even if they don''t say "Skald". Also trigger on "create a requirement", "let''s spec this", "write requirements for", "refine this user story", "what are the rules", "give me examples", "map this story", "is this too big", "moscow this", "split this requirement", or any Skald requirements tool call (createRequirements, updateRequirement, splitRequirement, rules / examples / open-question tools).'
---

# Skald Requirements

> **This is reference for you, not a script to read out.** The user asked a
> question, not for the manual. Answer it in a few points — three to five, the
> ones that matter for what they actually asked — then ask which part they want
> to go into. Never deliver a section-by-section tour of this document. If you
> find yourself writing headings, you are reciting: stop and ask instead.


## What a requirement is in Skald

A capability the product must support, written user-facing and solution-free.

In Skald's Spec-Driven Development model, **the requirement is the spec** — the living source of truth a team or coding agent builds from (see the skald-sdd skill for the full delivery picture). The workshop below is not paperwork: it authors what gets built, so an ambiguous rule becomes an ambiguous product.

Each requirement is one row in the `requirements` table with:

- **Title** — one User Story or one EARS sentence.
- **Type** — `User Story` (default for user-facing capabilities) or `EARS` (system requirements).
- **Priority** — MoSCoW: Must / Should / Could / Won't.
- **Domain** — the Skald domain the requirement lives under.
- **Rules** — testable acceptance statements that gate Done.
- **Examples** — Gherkin scenarios that illustrate rules.
- **Open Questions** — unresolved things blocking refinement.

Rules and Examples live in their own tables but always belong to a requirement. Open Questions too.

## Requirement vs Rule — don't over-split

The most common modelling mistake is minting a separate **requirement** for something that is really a **rule** of an existing requirement. A requirement is a *capability* the user wants; a rule is an *acceptance condition* that gates Done **on** that capability. A rule has no standalone user value — it only makes sense as a constraint on its parent.

A statement is a **rule of an existing requirement**, not a new requirement, when it is:

- **Validation / rejection** on a capability — e.g. "reject a duplicate name", "name must be non-empty". These belong to the create/rename requirement.
- **A side effect / cascade** of an action — e.g. "when a team is disconnected, clear it from affected backlog items". This belongs to the connect/disconnect requirement.
- **A state-gated exclusion** of a capability — e.g. "while archived, exclude from new connections". This belongs to the archive requirement.
- **Idempotency / edge-case behaviour** of an action — e.g. "connecting an already-connected pair causes no duplicate". Belongs to the action's requirement.
- **Phrased as EARS** but only meaningful *with* a parent capability. EARS phrasing (especially Unwanted Behaviour and Event-driven) is a strong smell that you're holding a rule, not a requirement. EARS earns its own requirement only when it describes platform behaviour with **no user-facing parent capability on this surface** (retention purge, audit emit, webhook handler, rate limit).

Quick test: *"Could a user ask for this on its own and get value?"* If no — it's a rule. If the only way to describe it is "…of \<some other capability\>", it's a rule.

Worked example (the duplicate-name / cascade / archive-exclusion trap):

- ❌ Wrong: `Create a workspace` + `Reject duplicate workspace names` + `Remember last active workspace` as three requirements.
- ✅ Right: **one** requirement `As an org member, I want to create a workspace…` with a **rule** "A name already used by another active workspace in the org is rejected."
- ❌ Wrong: `Connect a team to a project` + `Clear team from affected backlog items on disconnect` as two requirements.
- ✅ Right: **one** requirement `Connect and disconnect a team and a project` with **rules** "Disconnecting clears that team from the project's affected backlog items" and "Connecting/disconnecting is idempotent."

When you catch yourself proposing several EARS one-liners next to a user story, fold the constraints into that story's rules and re-confirm the slimmer set with the PM before writing.

## Title shapes

Use **User Story** when the requirement describes something a user (PM, developer, end customer, etc.) does or wants:

```
As a <role>, I want <capability> so that <benefit>.
```

Use **EARS** when the requirement describes platform behaviour the user doesn't see directly (retention purges, audit emits, rate limits, webhook handlers). EARS gives you six sentence shapes — Ubiquitous, Event-driven, State-driven, Optional, Unwanted Behaviour, Complex. Treat them as guidelines, not strict templates.

Full EARS reference with examples: [`references/ears-patterns.md`](references/ears-patterns.md).

## The workshop: Example Mapping, within Specification by Example

The elicitation method for any non-trivial requirement is **Example
Mapping** (Matt Wynne, Cucumber), practised inside Gojko Adzic's
Specification by Example. Skald's schema *is* Example Mapping's four
cards — the workshop writes straight into the model:

| Card | In Skald |
|---|---|
| 🟨 Story | the Requirement (title) |
| 🟦 Rule | Rules |
| 🟩 Example | Examples |
| 🟥 Question | Open Questions |

Your job is to **facilitate**, not to invent the answers. The flow:

1. **One story at a time.** The PM names a capability — frame it as a
   User Story title and confirm the role / benefit are right.
2. **Rules together.** Decompose the capability into testable acceptance
   statements. EARS shapes help, but don't force them. Stop when the
   rules collectively gate Done.
3. **Examples per rule — informally first.** Capture each example the
   Example Mapping way: a one-line concrete case, *"the one where the
   link has expired"*. Do NOT force Given/When/Then mid-conversation —
   formal Gherkin is written **after** the map is agreed, when you
   present the write proposal. (Formal style rules for that step:
   [`references/gherkin-style.md`](references/gherkin-style.md) — no
   Backgrounds, no Scenario Outlines, one behaviour per scenario.)
4. **Park questions, don't debate them.** When uncertainty appears —
   contradicting examples, an unknown outcome, a business call nobody
   in the room can make — name it as a question and *move on*. The red
   card exists so the workshop doesn't burn time on unresolvables.

**Read the map before writing.** The finished map is a readiness
instrument:

- **Many questions** → too uncertain to build; propose chasing answers
  before formalising.
- **Many rules** → the story is too big; route to INVEST splitting.
- **A tidy map, an example per rule, few questions** → ready to
  formalise and write.
- **It drags on** — a well-understood story maps quickly (canonically
  ~25 minutes). A session that keeps sprawling is telling you about the
  requirement, not the participants. Say so.

Full walkthrough with a worked example:
[`references/sbe-workshop.md`](references/sbe-workshop.md).

### New language surfaces in the workshop

Requirement sessions **produce** domain knowledge, not just consume it.
When the mapping coins a word that isn't in the glossary, uses an
existing term differently than its definition, or reveals that **no
domain fits the requirement** (the missing domain is the real first
step), flag it as you go. After the requirement writes land, offer to
capture the new terms and domains — the skald-domain-management skill
owns that craft; a batch of terms can go in one proposal.

### The mapping session (landscape, not depth)

A different mode from the workshop above: the PM wants to **sweep the
whole product** — "map what this product needs to do" — not refine one
requirement. Conduct it wide and shallow:

- Walk the product **area by area**, seeded by the vision, users, and
  the existing domain tree.
- Capture candidate requirements as **titles only** (user-story or EARS
  shape) — deliberately **no rules, no examples yet**. Depth comes
  later, per requirement, via the workshop.
- **Sort each into its domain** as you go; propose creating missing
  domains where the map demands them.
- **Explicitly no planning**: no PBIs, no sizing, no priorities. The
  output is a categorized requirement landscape.
- Exit by suggesting the natural next step: bundle deliveries from the
  landscape (skald-planning) or go deep on one requirement (the
  workshop above).

### Who's in the room

Canonical Example Mapping wants the **three amigos** — a product
person, a developer, a tester — because each asks questions the others
miss. In Skald the PM is often alone with you: **deliberately cover the
missing seats.** Ask the developer's question ("how would this fail?")
and the tester's ("what happens at the boundary?"). When team members
are in the conversation — Skald is built for teams receiving work and
joining in specifying — they are first-class participants: their rules,
examples, and questions land the same way, under the same discipline.

## No-solutions guideline

Requirements describe **what** the product does for the user, not **how** the code does it.

Coach the PM toward solution-free language. If they say *"Add a dropdown for sorting"*, suggest *"Sort the backlog by user-selected criteria"*. Mention the alternative once. Don't lecture. The PM has the final call — never force the rewrite.

Signals a draft is solution-leaning:

- Names a UI component (dropdown, modal, button)
- Names a code structure (table, column, route)
- Names a framework, library, or API
- Specifies a layout, colour, or ordering

Exception: a *system* requirement may legitimately name a substrate (e.g., "the audit log records X"). Keep it capability-focused even then.

## MoSCoW prioritization

**Always ask. Never default.**

When creating a requirement, ask the PM: *"Must, Should, Could, or Won't?"*. If unsure, offer a one-line recap of what each means in this product's context:

- **Must** — the release fails without this.
- **Should** — important; the release survives without it.
- **Could** — nice-to-have within scope.
- **Won't** — deliberately out of scope for this round (not "no, ever").

## Domain assignment

**Always discuss. Never default, never leave it empty.**

Every requirement lives under one Skald domain. It isn't decoration: the Requirements table groups and filters by domain and the treemap nests by it, so a requirement created without one drops out of that whole layer and has to be re-homed by hand later.

`domainId` is *optional* on `createRequirements` — which is exactly why it gets forgotten. Treat it as required.

Before writing, read the tree (`listDomains`) and propose a home:

- Infer the best fit from the requirement's subject, then say which one you picked **and why** — e.g. *"This reads like Planning › Release Planning, since it's about assigning PBIs to a release. Agree?"*
- When two parents are plausible, name both and let the PM choose.
- If nothing fits, say so and propose a new domain (`createDomain` — see the `skald-domain-management` skill) rather than leaving it unassigned.
- The PM decides. Never assign a domain they haven't agreed to.

## INVEST + splitting

Use INVEST as the lens for evaluating whether a requirement is well-formed:

- **I**ndependent — can be delivered without other requirements first.
- **N**egotiable — scope can flex; not over-specified.
- **V**aluable — delivers user-visible or system-meaningful value.
- **E**stimable — the team can size it.
- **S**mall — fits in a normal delivery rhythm.
- **T**estable — the rules and examples make Done unambiguous.

Splitting heuristic:

- **Splittable**: the requirement has multiple rules and at least one rule could plausibly stand alone as its own user story (delivers value on its own). Suggest splitting.
- **Not splittable**: all rules must apply together for any of them to deliver value (e.g., a checkout flow where partial steps make no sense to ship). Leave it as one requirement.

Full heuristic with worked examples: [`references/invest-splitting.md`](references/invest-splitting.md).

When splitting in Skald, use `getRequirementForSplit` first to read the existing rules/examples, then `splitRequirement` to create the new requirements while preserving traceability.

## Open Questions discipline

You may ask open questions out loud at any point during the workshop or refinement. The gate is on **you, not on people**: never register a question in Skald on your own initiative — **the human driving the session decides** what gets recorded. Usually that's the PM; a developer or tester refining a requirement records their questions the same way (open questions are the collaboration seam between the PM and the team — see skald-sdd).

When the human agrees to record one, use `createOpenQuestions`. Questions are later resolved via `answerOpenQuestion`, `dismissOpenQuestion`, `reopenOpenQuestion`, or `archiveOpenQuestion`.

Avoid filing Open Questions for things you can answer by reading: existing requirements, existing glossary terms, existing PBIs. Use the read tools first.

## Requirement status

A requirement carries a status that records **how settled it is** — a signal, not a box to tick: **Draft → Approved → Delivered**, with **Blocked** and **Deprecated** off to the side.

- **Draft** — still being shaped. Every requirement you create starts here (the create tool has no status field) and stays Draft until someone deliberately moves it.
- **Approved** — the PM agrees it is right and refined enough to build against: clear rules, examples that show those rules, and no open question still blocking it.
- **Blocked** — something unresolved is stopping it (an open question, a dependency, an undecided call).
- **Deprecated** — the intent was dropped. Different from archiving, which just hides it.
- **Delivered** — built and live.

`updateRequirement` is the only lever, and a status change is a **write** — so **you propose, the PM moves it**, under the normal confirmation gate. Never flip a status silently. When you judge a requirement has reached Approved quality, say *why* (rules present, examples cover them, nothing blocking) and ask. Status matters downstream: a backlog item is not really understood while a requirement behind it is still an unrefined Draft — see "Is a backlog item ready to build?" in the skald-planning skill.

## Check for duplicates before creating

Before creating a requirement, read what already exists — `getExistingRequirements` (and `listPbis` for the PBI side). A capability already specced under a near-identical title, or already covered by an existing PBI, is a duplicate. Surface the match to the PM and ask whether to extend the existing artifact or genuinely add a new one. **Never create blind.** If a duplicate is found after the fact, reconcile rather than leaving two live copies — fold the rules/examples into the survivor and retire the loser.

## Confirmation discipline

This applies to every write tool you call:

1. Summarise the intended change in plain language (a proposal). Include the requirement's title, type, priority, and domain.
2. Wait for the PM's go-ahead — a plain "yes" to the proposal suffices.
3. Write everything the confirmed proposal covers — no fresh "yes" per item.
4. Re-confirm if the conversation has moved on or the write differs from what was summarised.

This is the same discipline both agentic surfaces enforce. Restated here because this skill is shared: the in-app Skald Agent and external MCP clients load this same document.

## Tool catalogue

Full list with when-to-use for primary and adjacent tools (domain lookup, glossary, PBI linkage): [`references/tool-catalogue.md`](references/tool-catalogue.md).

Quick reference — the tools you'll reach for most:

**Reads** (no confirmation needed):
- `listRequirements({ projectId })` — discovery. Pass `includeArchived: true` to also see archived requirements (each row carries an `archived` flag) when you need to find one to restore.
- `getExistingRequirements({ projectId })` — same data, slightly richer shape for triaging gaps (also takes `includeArchived`).
- `getRequirementForSplit({ id })` — pre-flight before `splitRequirement`.
- `listRules({ requirementId })`, `listExamples({ requirementId })`, `listOpenQuestions({ requirementId })`.

**Writes** (confirmation discipline applies):
- `createRequirements`, `updateRequirement`.
- `splitRequirement`.
- `archiveRequirement`, `restoreRequirement` — archive is the destructive verb (there is **no delete**); archiving cascade-archives the requirement's rules/examples/open-questions and removes it from active lists, the bin, and search, while preserving PBI links. `restoreRequirement` brings it (and its children) back; resolve the archived requirement via `listRequirements({ includeArchived: true })`.
- `createRules`, `updateRule`, `archiveRule`.
- `createExamples`, `updateExample`, `archiveExample`.
- `createOpenQuestions`, `answerOpenQuestion`, `dismissOpenQuestion`, `reopenOpenQuestion`, `archiveOpenQuestion`.

**Adjacent reads** worth doing before writing a requirement:
- `listDomains` — **required before creating**: pick the home domain and agree it with the PM (see [Domain assignment](#domain-assignment)); pass it as `domainId`.
- `listDomainTerms` — use the project's existing vocabulary so the requirement reads consistently.
- `listGoals` — link the requirement back to an objective if the PM wants to.
- `listPbis` — check whether an existing PBI already covers it.

## Draw it

In the app's agent workspace you have a canvas.

- Teaching the method — rules, examples, open questions, or why a rule without an
  example isn't grounded — call `showCanvasVisual` with
  `visual: "example-mapping"`.
- Working through **several** requirements under one backlog item or domain, where
  the PM needs the whole set in view rather than the one in front of them, call it
  with `visual: "requirement-set"`. This is the answer to *"after the fourth one
  I've lost track"* — reach for it before they say it.
- **Asked to SEE a specific artefact** — "show me PB-12", "open REQ-40", "what's
  in this item" — call it with `visual: "subject-card"` as you fetch the
  details. Draw it, then talk through what you found; do not answer with a
  prose-only summary of something the canvas can show.

Then don't restate the diagram; say what it doesn't — the judgement, the gap, the
next move.
