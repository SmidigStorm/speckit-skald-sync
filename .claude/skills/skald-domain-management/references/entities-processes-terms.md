# Classifying glossary terms: Entity / Process / Term

Every glossary term in Skald has one of three types. The type is what
turns a glossary from a flat word list into a model of the product. This
file is the detailed guidance for picking the right one.

## The three types

### Entity — a thing with identity

An Entity is something the product treats as a distinct, identifiable
thing. The tells:

- You can have **several** of them, each its own row, each nameable.
- The product (or the team) **stores** them, lists them, points at one.
- It makes sense to say "this particular X" or "all the X's".

Skald Entities: Requirement, Rule, Example, Open Question, Backlog Item
(PBI), Objective, Key Result, Check-in, Time Period, Domain, Glossary
Term, Vision, User, User Segment, Team, Member, Role, Organization,
Workspace, Product, Audit Event, Command Palette, Search Index.

### Process — an activity that happens

A Process is something done over time — a flow, a procedure, a recurring
activity. Often a verb turned into a noun ("refine" → "Refinement").
The tells:

- It has steps, or a before/after.
- It's something the team or the system **runs** or **performs**.
- You'd describe it with verbs: gather, order, replace, repair, split.

Skald Processes: Backlog Refinement, Prioritization, Requirement
Splitting, Pseudonymization, Retention Purge, Data Export (the act),
Reconciliation.

### Term — a concept, framework, or rule

A Term is everything else worth defining: a concept, a principle, an
acronym, a methodology, a rule. The tells:

- You can't point at one instance or run it; it's an **idea** about how
  things work.
- It's a framework (EARS, MoSCoW, OKR), a principle (Ubiquitous
  Language, Bounded Context, Agent–MCP Parity), or a named rule
  (Last-Write-Wins, Confirmation Discipline).

Skald Terms: EARS, Specification by Example, MoSCoW, Gherkin, OKR,
Acceptance Criteria, Ubiquitous Language, Bounded Context, Domain
Hierarchy, Estimate, PBI Status, PBI Type, Mastra, MCP, Tool Inventory,
Agent–MCP Parity, Confirmation Discipline.

## The decision procedure

Apply in order; stop at the first yes:

1. **Can you have several, each with its own identity, stored as rows?**
   → **Entity.** ("three Requirements", "two Teams", "this Objective".)
2. **Is it something the product or team *does* over time — a flow with
   steps or a recurring activity?** → **Process.**
3. **Otherwise** — a concept, principle, framework, acronym, or rule →
   **Term.**

## Tie-breakers

The hard calls are Entity-vs-Term and Entity-vs-Process.

### Entity vs Term

Ask: **does the product store rows of these?**

- *Estimate* — feels concrete, but it's the *concept* of relative
  sizing (S/M/L/XL), not a stored row you create independently. It's a
  field on a PBI. → **Term.**
- *Key Result* — the product stores Key Result rows, each with its own
  values and check-ins. → **Entity.**
- *PBI Status* — the set of lifecycle states is a concept; you don't
  create "a Status". → **Term.**

Rule of thumb: if it's a **field, attribute, or classification** of
some Entity, it's usually a Term. If it's the **row itself**, it's an
Entity.

### Entity vs Process

Ask: **is it the thing, or the act on the thing?**

- *Data Export* — if you mean the downloadable artefact, that leans
  Entity; if you mean the act of exporting, Process. Pick based on how
  the product talks about it. (In Skald it's been treated as the act →
  Process; confirm with the PM.)
- *Check-in* — a stored point-in-time entry with a value. The act of
  checking in produces one, but the glossary term names the stored
  thing. → **Entity.**
- *Reconciliation* — the cron *runs*; there's no "a Reconciliation" row
  the PM manages. → **Process.**

When genuinely ambiguous, ask the PM which framing they use day-to-day.
The right answer is the one that matches how the team talks.

## Worked classification set

| Term | Type | Why |
|------|------|-----|
| Requirement | Entity | Stored rows, each with identity |
| Backlog Refinement | Process | Ongoing activity with steps |
| MoSCoW | Term | A prioritization scheme (concept) |
| Key Result | Entity | Stored rows with values |
| Estimate | Term | A field/concept on a PBI, not a row |
| Requirement Splitting | Process | An act performed on a requirement |
| Bounded Context | Term | A DDD concept |
| Team | Entity | Stored rows, nameable |
| Reconciliation | Process | A cron activity |
| Agent–MCP Parity | Term | A governing rule |

## Why this matters

A glossary where types are assigned consistently lets everyone — humans
and the AI agent — reason about the product: Entities are the nouns the
product manipulates, Processes are the verbs, Terms are the shared
concepts that keep the language precise. Sloppy typing (everything a
"Term") collapses that signal. Spend the extra moment to classify well.
