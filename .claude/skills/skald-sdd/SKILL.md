---
name: skald-sdd
description: 'Spec-Driven Development (SDD) in Skald — requirements as the living specs that drive delivery. The requirement is the spec; one PBI is one change; per-feature spec files are derived at delivery time and disposable. Covers the two-layer truth model, status lifecycles as gate verdicts (Draft → Approved → Delivered; Done means live in production), the write-back rule, advisory vs hard gates, the code-repo constitution, and delivery via the speckit-skald-sync preset. Use whenever the user asks how requirements are used, what happens when a PBI gets built, what a status means, why something isn''t Done yet, how Skald connects to Spec Kit or coding agents, or anything about spec-driven development — even if they don''t say "SDD". Trigger on "what is SDD", "spec-driven", "the spec", "how does delivery work", "what does Approved mean", "write back", "one PBI one change", "hand this to the team".'
---

# Spec-Driven Development in Skald

> **This is reference for you, not a script to read out.** The user asked a
> question, not for the manual. Answer it in a few points — three to five, the
> ones that matter for what they actually asked — then ask which part they want
> to go into. Never deliver a section-by-section tour of this document. If you
> find yourself writing headings, you are reciting: stop and ask instead.


**The requirement is the spec. One PBI is one change.** Everything else
in this skill unpacks those two sentences.

That is also why Skald's **main flow** runs **backlog item → requirements →
delivery**: you pick the change, its requirements say what "done" means, and the
specification is what a team or a coding agent builds from. Strategy, domains and
research feed that line without sitting in it.

Spec-Driven Development is the practice of treating the *specification*
— not the code — as the source of truth. When the software is wrong, you
fix the spec and re-derive, rather than patching code until the spec is
fiction. With AI agents doing more of the building, this stops being
philosophy and becomes mechanics: the spec is what the agent builds
from, so spec quality *is* delivery quality.

## The two-layer truth model

Skald splits SDD's two jobs across two artefacts, and keeping them
straight is the core of this skill:

| Layer | Artefact | Lives | Answers |
|---|---|---|---|
| **Living truth** | the Requirement — title + rules + Gherkin examples + open questions | in Skald, permanently | *"What is the current intended behaviour?"* |
| **Change unit** | the PBI (backlog item) | in Skald, one per delivery | *"What are we changing this time?"* |

Per-feature spec files (`spec.md`, `plan.md`, `tasks.md` in the code
repo) are **derived, disposable snapshots** — generated from the PBI's
linked requirements when delivery starts, useful during the build,
authoritative never. If a spec file and a Skald requirement disagree,
the requirement wins; fix the requirement, re-derive the file.

This split is what prevents **spec rot** — the known failure mode of
keeping per-feature spec folders as truth, where after thirty features
"what does the product do?" requires archaeology. Skald's answer is
always one read away: the requirement.

## The delivery lifecycle

One PBI maps to one delivery flow (today: one Spec Kit feature,
conducted from a developer's machine via the `speckit-skald-sync`
preset — the in-app chat explains and prepares this flow; it does not
run it). Status changes are **gate verdicts**, not bookkeeping:

1. **Refining Requirements** (PBI) — the change is being shaped. Its
   requirements are drafted, workshopped (see skald-requirements),
   linked.
2. **Approved** (requirement) — the PM's verdict that the spec is right
   and refined enough to build against: clear rules, examples showing
   them, nothing blocking. Approval is the *spec gate*.
3. **Ready for Planning → In Progress** (PBI) — delivery starts; the
   spec files are derived from the linked requirements.
4. **Delivered** (requirement) — that capability is built and verified,
   flipped per green story checkpoint during the build.
5. **Done** (PBI) — **only when the change is live in production.** Not
   merged, not demo-ready, not "gate should pass" — live. This is
   Skald's honesty rule: a status claims a verdict someone can check,
   and no gate verdict is ever self-certified on red. If a mandatory
   check failed or was skipped, report the true state instead.

## The write-back rule (the learning loop)

You learn by building — SDD's most serious critique is that up-front
specs deny this. Skald's answer: **learnings write back, in flight.**
When delivery surfaces a clarification, a missing rule, a wrong
example, or a new question, the change lands **in the Skald
requirement** — via the normal update tools, under the normal
confirmation discipline — not as an untracked edit to a derived spec
file. The living truth stays true *while* the build teaches you things.

(Today write-backs are in-place edits. A future model may stage them as
reviewable per-PBI deltas — like a pull request for requirements — but
that is not built; don't describe it as if it exists.)

## Gates: advisory in the agent, hard in CI

Skald deliberately runs a two-tier gate model:

- **Advisory gates (agent-side)**: readiness to build is a *judgment*
  you help the PM make — read the PBI with its requirements and assess
  coverage, refinement, clarity, coherence (the full method is in
  skald-planning, "Is a backlog item ready to build?"). You advise; the
  PM may knowingly proceed anyway.
- **Hard gates (delivery-side)**: tests, typechecks, and CI in the code
  repo. These are not advisory and not yours to waive. The honesty rule
  above binds every status you propose to them.

The code repo also carries a **constitution** — the engineering
principles the delivery flow enforces (testing standards, quality
gates). That is the code side's counterpart to Skald's requirements;
know it exists, don't restate it.

## Doing SDD with your developers

When the PM wants their team to build from Skald's specs, two pieces
connect a developer's coding agent (Claude Code, Cursor, any MCP-aware
tool) to Skald — point the PM at the **Coding agents** page in the app
(under Manage), which documents both:

1. **The Skald MCP server** — gives the developer's agent the same
   project-scoped tools this surface has (read requirements and rules,
   raise open questions, propose status changes), under the same
   confirmation discipline. This is how the derived spec stays connected
   to the living one during a build.
2. **The `speckit-skald-sync` preset** — a public GitHub repo
   (`SmidigStorm/speckit-skald-sync`) the developer installs into their
   code repo's Spec Kit setup. It wires the whole loop taught above:
   specs derived from the PBI's linked requirements, clarifications
   written back, statuses flipped on gate verdicts. It ships with these
   same `skald-*` methodology skills bundled, so their agent and this
   one work from one playbook.

A developer set up with both is a full SDD participant: they receive
work through the PBI, build against the requirement, and their
questions and learnings flow back into the living spec.

## What this means for how you work

- **Spec quality is delivery quality.** The workshop discipline in
  skald-requirements (rules, examples, open questions) isn't paperwork
  — it is literally authoring what gets built. An ambiguous rule
  becomes an ambiguous product.
- **Open questions are the collaboration seam.** Team members receiving
  work through Skald participate here: a developer or tester who hits
  an ambiguity raises an open question on the requirement; answering it
  updates the spec for everyone, forever.
- **Ceremony scales to risk.** A one-line bug fix does not need the
  full workshop; a capability with money or data on the line needs all
  of it. Propose the depth that fits; never inflate process for its own
  sake.
- **Route, don't absorb.** The crafts live in their own skills:
  authoring specs → skald-requirements; sizing, splitting, readiness →
  skald-planning; what to work on next → skald-health-check. This skill
  is the map of how they connect to delivery.

## Anti-patterns, by name

- **Spec-folder archaeology** — treating `specs/NNN-*` files as truth.
  They are exhaust. The requirement is the spec.
- **Patch-and-drift** — fixing behaviour in code (or in a derived file)
  without writing it back to the requirement. The spec becomes fiction.
- **Self-certified Done** — flipping a status without its verdict
  ("tests will probably pass"). Statuses are gate verdicts.
- **Uniform ceremony** — forcing every change through every step.
  Ceremony scales to risk.
- **BDUF in disguise** — trying to perfect the spec before any building
  starts. The write-back rule exists precisely so specs can start good
  and get right.

## Draw it

In the app's agent workspace you have a canvas.

- Asked how a spec becomes code, what a status actually means, or why the
  requirement outranks a spec file — call `showCanvasVisual` with
  `visual: "sdd-delivery"`.
- Asked how Skald fits together as a whole, or where something sits in the bigger
  picture — call it with `visual: "product-development-flow"`, optionally with `subject` set to
  the stage under discussion.

Not this visual: what a backlog item *is* and how it's sized is `backlog-model`.
