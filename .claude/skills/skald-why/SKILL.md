---
name: skald-why
description: 'Why Skald exists and what it changes for a product manager — use when someone asks what Skald is for, what makes it different from a backlog tool plus a wiki, why they should move their product into it, or says some version of "I don''t see what''s new". Also when a pilot user is unconvinced two weeks in, when writing or reviewing positioning copy, and when an external coding agent needs to understand why the requirements it reads are structured the way they are. Contains the argument, the concrete test that settles it, and the claims that must stay accurate. Not a feature list and not a script.'
---

# Why Skald

This is reference for you, not a script to read out. Take the part that answers what was
actually asked. If they have asked something broad, give the one-line version and then
ask which part they want.

**Lead with the shift, then offer the test.** For a narrow question — "why do I have to
write examples?" — answer just that, from the relevant section below. The shift belongs to
the "why Skald at all" conversation, not to every question.

Never blame the reader. Every failure described here is the tools' failure. A PM
compensating for scattered information is doing skilled work in a bad system.

---

## The shift

**Your developers can build faster than you can specify. That is the new constraint, and
it is yours.**

AI took the slow part out of building. What it cannot do is decide what to build, or know
what "right" means — so the work that decides both moved onto the critical path. The teams
pulling ahead are the ones whose product manager can hand an agent a specification precise
enough to build from. That is **spec-driven development**, and it is becoming how software
gets made.

The gap compounds. A PM who writes ticket descriptions gets rounds of clarification; a PM
who writes rules and examples gets working software. Every delivery makes the next one
faster — or it doesn't.

## The test

Do not argue this. Offer it, and let them check it in ten minutes.

> **Pick a key result. Ask: which requirements does it depend on, and which of those
> aren't built yet?**

In Skald that is two hops. A key result links to backlog items; a backlog item links to
the requirements it realises; a requirement carries its own status. You follow the links
and you have the answer — including the requirement that is fully specified, approved,
and simply not built. Nobody had to remember anything.

With a backlog tool plus a wiki plus an OKR page, the first hop is not a link — it is a
sentence someone wrote. You can find related tickets. You cannot follow them, because
the connection was never stored as data. No amount of discipline creates it later, and
no amount of spend buys it: those tools are not missing the feature, they are missing
the model.

That is the whole argument. Everything below is elaboration.

*(In the app's agent workspace you have a canvas — call `showCanvasVisual` with
`visual: "why-skald"` before explaining, and walk them down it. Not
`product-development-flow`: that one shows what Skald is, this one argues why you would
use it. Elsewhere, the test above works spoken.)*

## What it costs today

Not "your information is scattered" — that is abstract, and they have heard it. Say what
it costs, in the shape they will recognise:

- A developer is unsure whether an edge case is intended. The rule exists — in a comment
  on a ticket that closed in March. They ask the PM.
- A tester writes cases from the ticket description, because the examples were agreed in
  a workshop and never written down anywhere findable.
- A designer asks who this is for. It is answered in a doc from onboarding that nobody
  links to any more.
- A coding agent gets a prompt written by hand, because the requirement it needs is
  prose in three places and none of it is addressable.

Each of those routes through the PM. The PM spends the day being the index, and that
work is invisible — it never looks like a problem, it just looks like a busy PM.

This is a cost of scattered information, not a promise about teams. Skald has no team
membership and no cross-team workflow. It fixes the source, not the org chart.

## Why better habits don't fix it

The tools hold no opinion about product work. A backlog tool knows about tickets; it
does not know what a requirement is, that a rule needs an example, or that a key result
describing an activity is really an output. So the right way is always the effortful
way — every good practice is something you impose, alone, forever, and it decays the
first busy week.

Skald holds the opinion. The structure is typed, so the connections exist by
construction rather than by diligence, and the agent conducts the method rather than
reminding you to.

## What Skald actually does

Four things, each of which the reader can picture:

**Stores the product as one model.** Vision and users, domains and a glossary,
requirements with rules and Gherkin examples and open questions, backlog items,
releases, goals. Typed records with real links — a key result to backlog items, a
backlog item to the requirements it realises, a requirement to its domain.

**Conducts the method with you.** The agent runs an example-mapping workshop on a
requirement, tells you when a backlog item is too big and where it splits, and pushes
back when a key result is an output rather than an outcome. It is a practitioner working
with you, not a box that summarises.

**Makes the spec directly readable by coding agents.** Claude Code and others read the
same requirements, rules and examples over MCP — the record itself, not a prompt you
hand-wrote from it. When the requirement changes, what they read changes.

**Keeps open questions as first-class content.** An unanswered question is a real
record, not a TODO in a paragraph. Being incomplete on purpose is a normal state.

## What you get

You stop being the index. The questions that used to route through you get answered from
the structure, and the time comes back as direction: deciding what should be true, not
transmitting what already is.

## The frame — only for comparative questions

If they ask "why now" or "why didn't this exist before": AI moved the constraint. Writing
code got dramatically faster, so the bottleneck moved upstream to knowing what to build
and being able to say it precisely. Tools built when coding was the constraint optimise
the wrong end — and a coding agent is only as good as the specification it is given,
which makes the quality and the addressability of that specification the new limit.

Do not lead with this. It is true, it is everywhere, and it sounds like everyone else.

## Accuracy guardrails

- Never name a competitor. Categories only: "a backlog tool", "a wiki", "an OKR page".
- Never claim team coordination, permissions or workflow. Skald has none.
- Never promise time saved, velocity or a percentage. Offer the test instead.
- Everything above is true of Skald today. Do not extend it with roadmap.
- If they disagree, do not add claims. Go back to the test — it is checkable, and an
  argument they can check beats one they have to accept.
