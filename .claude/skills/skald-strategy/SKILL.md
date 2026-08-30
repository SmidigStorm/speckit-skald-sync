---
name: skald-strategy
description: 'How to draft and maintain a product''s Vision and User list in Skald, using Roman Pichler''s Product Vision Board as the interview — Target Group → Needs → Product → Business Goals, distilling the Vision statement last. The stored Vision is short Markdown (Problem, Vision, Differentiation, optionally up to five capabilities); Users capture who the product is for (name, description, goals, pains); Business Goals live in the Goals/OKR layer, not in strategy text. Use whenever the user is writing, editing or reviewing a Vision, or working on the project''s Users (personas, user types, segments) — even if they don''t say "Skald". Trigger on "create a vision", "draft a vision", "let''s write the vision", "vision board", "elevator pitch", "what is this product about", "who is this for", "add a user", or any Skald strategy tool call (setProjectVision, getExistingProjectVision, addProjectUser, updateProjectUser, archiveProjectUser, listProjectUsers).'
---

# Skald Strategy

> **This is reference for you, not a script to read out.** The user asked a
> question, not for the manual. Answer it in a few points — three to five, the
> ones that matter for what they actually asked — then ask which part they want
> to go into. Never deliver a section-by-section tour of this document. If you
> find yourself writing headings, you are reciting: stop and ask instead.


The Strategy module in Skald is small and focused: a product has one
**Vision** and a list of **Users**. Both are PM-owned. The agent's job
is to help the PM articulate them clearly — by conducting the interview
below — not to invent them, and never to update them without explicit
confirmation.

## The method: Product Vision Board

Skald's vision work follows **Roman Pichler's Product Vision Board** —
five boxes, each landing in an existing Skald home:

| Board box | Lands in Skald as |
|---|---|
| **Vision** | the Vision statement inside the Vision body |
| **Target Group** | the project's User rows |
| **Needs** | each User's goals and pains |
| **Product** | an optional "Product" list in the Vision body — **at most 5 items** (more is a roadmap, not a vision) |
| **Business Goals** | links into the Goals/OKR layer — offer to continue there; never stored as strategy text |

Skald deliberately uses only the **simple** board. The *Extended*
Product Vision Board (competitors, revenue streams, cost factors,
channels) is out of scope by design — that is business modelling, not
product vision. Decline it by name if asked, and say why.

## Conducting the vision interview

When the PM wants a vision (or wants to sharpen one), conduct the board
as a conversation — one box at a time, drafting as you go, concrete
before abstract. **Distil the Vision statement last**: it writes itself
once the other four boxes have answers.

1. **Target Group** — *Who is this product for? Who benefits most?*
   Capture each answer as a User row (name + description). Surface the
   existing list first.
2. **Needs** — *What problem does it solve for them? What do they gain?*
   Land these as each User's goals and pains. Push past features to the
   underlying struggle.
3. **Product** — *What is it, and what 3–5 things make it stand out?*
   A short list inside the Vision body. **Enforce the cap**: if the PM
   lists more than five, that's the roadmap talking — ask which five
   define the product.
4. **Business Goals** — *How does this product benefit the company?*
   (growth, revenue, retention, brand, learning…). Don't store these as
   prose — offer to make them measurable in the Goals/OKR layer (load
   the skald-goals skill / point the PM at Goals). A vision whose
   business goals never become OKRs is a poster, not a plan.
5. **Vision** — now distil: *in one or two sentences, what future are
   all of the above building toward?* Draft it from the answers already
   on the table, then let the PM reshape the words.

Steps can be skipped or reordered when the PM already has answers —
the board is a coverage map, not a script. Existing content counts:
read `getExistingProjectVision` and `listProjectUsers` first and slot
what's already there into the boxes before asking anything.

## The exit test

Before proposing the final body, check the draft against Pichler's
vision qualities and say which fail:

- **Shared** — would the whole team recognise it as *their* goal?
- **Inspiring** — does it describe a change worth making, or just
  restate the product?
- **Concise** — can the PM say it aloud from memory?
- **A vision, not a roadmap** — does it describe a destination, or a
  feature list with dates?

Any "no" routes back to the weak box. Run the test, don't lecture it.

## Vision (the stored artifact)

One Markdown body per product, stored on the project. Idempotent —
every write replaces the previous body in full. The agent never edits
the Vision on its own initiative.

The stored shape is Skald's three-part elevator pitch, plus the
optional Product list from the interview:

- **Problem** — the pain the product exists to relieve (the Needs box,
  summarised).
- **Vision** — the long-term direction (the Vision box).
- **Differentiation** — why this product, and not some other approach.
- **Product** *(optional)* — what it is, in at most five standout
  capabilities (the Product box).

This shape is a **suggestion**, not a contract. If the PM has a Vision
that doesn't fit, help them write what they have rather than forcing
the sections. See
[`references/vision-template.md`](references/vision-template.md) for
the template prose, drafting tips, and a worked example.

### When to write or update

- **Write a Vision** when the project doesn't have one and the PM
  wants to capture direction. Read with `getExistingProjectVision`
  first — if a Vision already exists, surface it before proposing
  changes.
- **Update a Vision** only when the PM asks. Big shifts (a pivot, a
  new differentiator, a clearer Problem statement) are real reasons to
  update; cosmetic edits usually aren't worth the write.
- **Never propose updating a Vision on your own initiative.** Don't
  read the Vision and then say "this seems stale, want me to refresh
  it?". That's the PM's call.

### Confirmation discipline for Vision

Vision writes replace the entire body. Before calling `setProjectVision`:

1. Show the full new Markdown body in the proposal — not just a
   description of the change. The PM is replacing existing content, so
   they need to see what's going in.
2. If updating, show the old body alongside the new body so the PM
   can spot anything they want to preserve.
3. Wait for the PM's go-ahead, then write.

## Users

A list of the **kinds of users** the product is built for. Each row is
one user type (sometimes called a persona, user segment, or just
"user"). Skald uses one table for all of these — there's no separate
Segment table. In board terms, the User rows *are* the *Target Group*
box, and their goals and pains *are* the *Needs* box.

Each row has:

- **Name** (required) — short, plain English. *"Product Manager"*,
  *"Tech Lead"*, *"External stakeholder reviewing a share link"*.
- **Description** (optional, Markdown) — who this user is in 1–3
  sentences.
- **Goals** (optional, Markdown) — what this user is trying to achieve
  when they touch the product. Bullet list usually works best.
- **Pains** (optional, Markdown) — current frustrations or friction
  this user faces today. Also usually a bullet list.

See [`references/users-guide.md`](references/users-guide.md) for what
makes a good entry, the relationship to Requirements (the `<role>` in
a User Story title should usually be a Project User), and worked
examples.

### When to add or edit

- **Add a User** when the PM identifies a new kind of person the
  product serves. Surface the existing list first via
  `listProjectUsers` so the PM can decide whether the new entry is
  genuinely new or a sub-flavour of an existing entry.
- **Edit a User** when the PM wants to refine the description, goals,
  or pains. Pull the existing row first, propose the patch, confirm.
- **Archive a User** when a user type is no longer relevant.
  Archived rows stay in the database for history.
- **Never invent users.** Goals and pains in particular are easy to
  fabricate; don't. Ask the PM to describe the user in their own words
  and capture that.

## Check for duplicates before creating

Before `addProjectUser`, read `listProjectUsers` first and surface the
existing list — a new entry that's really a sub-flavour of an existing user
is a duplicate. Ask before adding. **Never add blind.** (Vision is a single
idempotent body, so it can't duplicate — but always `getExistingProjectVision`
before a write so you don't clobber content you haven't seen.)

## Tool catalogue

All Strategy tools are project-scoped — they take a `projectId`. Names
are identical on both agentic surfaces per Constitution V (Agent–MCP
parity).

**Reads** (no confirmation needed):
- `getExistingProjectVision({ projectId })` — current Vision Markdown,
  or null if none.
- `listProjectUsers({ projectId })` — every user row, alphabetical by
  name.

**Writes** (confirmation discipline applies):
- `setProjectVision({ projectId, markdownBody })` — idempotent UPSERT.
  Pass an empty string to clear.
- `clearProjectVision({ projectId })` — explicit clear; equivalent to
  `setProjectVision` with an empty body.
- `addProjectUser({ projectId, name, descriptionMarkdown?, goalsMarkdown?, painsMarkdown? })`
  — add a user row.
- `updateProjectUser({ projectId, userId, patch })` — partial update.
  Only the fields in `patch` change.
- `archiveProjectUser({ projectId, userId })` — soft-archive. No hard
  delete by design.

**Adjacent** worth doing during Strategy work:
- `listDomainTerms({ projectId })` — use the project's ubiquitous
  language consistently. The Strategy domain's glossary
  (Vision, User, User Segment) is the canonical reference.
- `getTimePeriods` / `listGoals` — when Business Goals surface in the
  interview, read what already exists in the Goals layer before
  offering to continue there.
- `listProjects()` — when you don't already have a `projectId`.

## Confirmation discipline (restated)

Every write tool here is preceded by a plain-language summary (a
proposal) and the PM's go-ahead — a plain "yes" to the proposal
suffices. One confirmation covers the whole proposal it answers.
Re-confirm if the conversation has moved on or the write differs from
what was summarised.

For `setProjectVision` specifically: show the full proposed body in the
summary, not just a description. The PM is replacing content; they need
to see what's going in.

## Draw it

In the app's agent workspace you have a canvas. When the PM wants to see where the
vision stands — after a board interview, or when they ask what they decided — call
`showCanvasVisual` with `visual: "vision-board"`. It reads their real project, so
it is a mirror rather than a lesson: don't narrate the boxes back to them, talk
about what's thin or missing.

Empty boxes are not failures. A PM may decide a box does not apply.
