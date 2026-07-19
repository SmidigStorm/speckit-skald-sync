# The Vision template

In Skald a product's Vision is a single Markdown body. The agent helps
the PM draft it — by conducting the Product Vision Board interview in
`SKILL.md` — but the PM owns the words. The agent never enforces a
template; when the PM is starting from a blank page, suggest the
**elevator pitch** shape below as the landing place for the interview's
answers.

## The template

The full template, in suggested order:

```markdown
## Problem

<1–3 sentences naming the pain the product exists to relieve. Concrete
beats abstract. Name the user, the situation, and what's broken or
missing today. This is the interview's Needs box, summarised.>

## Vision

<1–3 sentences describing the long-term direction the product is heading
in. Future-facing. Optimistic but not vague. Distilled LAST in the
interview, from everything else.>

## Differentiation

<1–3 sentences explaining why this product (and not some other
approach) solves the problem. What's the angle, the opinionated take,
the bet?>

## Product   <!-- optional -->

<What the product is, in AT MOST five standout capabilities. This is
the interview's Product box. More than five = the roadmap talking.>
```

Tight sections; the whole pitch should fit on one screen — that's the
elevator. How each section maps to the Product Vision Board is in
`SKILL.md` (Target Group and Needs live on the User rows, Business
Goals live in the Goals/OKR layer — neither is stored in this body).

## Drafting tips

- **Problem comes first for a reason.** A Vision without a stated
  Problem floats. Help the PM name the pain before the cure.
- **Avoid product-management jargon.** Words like "synergy",
  "stakeholder alignment", "best-in-class" empty out fast. Push the
  PM to say what they actually mean.
- **Name the user where it matters.** "Product Managers struggling
  with multi-team product delivery" lands harder than "users".
- **Differentiation isn't a feature list.** The bet — the opinionated
  thing this product does that others don't — is what belongs here.
  Not the feature roadmap.
- **The Product list is capped at five.** If the PM keeps adding,
  ask which five *define* the product; park the rest for the backlog.
- **Date-stamp big changes outside the Vision body.** Skald doesn't
  diff Visions automatically; if the PM cares about when a pivot
  happened, they can mention it in the commit/PR or in a notes
  doc — don't put a changelog inside the Vision.

## When the PM has a Vision that doesn't fit

The template is a suggestion. If the PM walks in with a Vision shape
that works for them — a single paragraph, a list of bullets, a
mission/vision/values structure — help them write what they have.
Don't force the sections.

When you're not sure, ask: *"Want me to keep your structure or
suggest the template shape?"*.

## Worked example: Skald itself

Skald's own production Vision (lightly abridged) shows the shape:

```markdown
## Problem

Managing a product is tough. A product manager needs an overview of
*everything* — vision, users, domain language, requirements, backlog,
releases, and goals — but it lives scattered across Jira, Confluence,
Figma, and people's heads. Nobody can see the whole product in one
place, so gaps and contradictions go unnoticed. And getting it *right*
— testable requirements, outcome-based OKRs, well-split backlog items —
is a skill most PMs have no coach for while doing the work.

## Vision

Skald is an AI-native, opinionated product-management tool that holds
the whole product as **one connected model** and puts an **agent at
your elbow** to conduct the work. The agent does the heavy lifting;
humans keep the decisions.

## Differentiation

Where Jira, Confluence, and Figma are unopinionated and make *bad*
process possible, Skald guides you toward good practice —
Specification by Example, DDD-lite domain modelling, a single shared
backlog, and outcome-based OKRs — and does it with you rather than
just storing what you type.
```

Note what makes it work: the Problem names the person and the struggle,
the Vision fits in a breath, the Differentiation is a bet ("opinionated
guidance"), not a feature list.

## Anti-patterns

Watch for and rewrite:

- **Mission statement that says nothing.** *"Skald empowers world-class
  product teams to deliver outsized value."* — empowers what? deliver
  what? Rewrite.
- **Differentiation as a feature list.** *"We have OKRs, requirements,
  PBIs, and agents."* — the *what*, not the *bet*. Rewrite.
- **Problem that's actually a solution.** *"PMs don't have a great
  AI-native PM tool."* — that's restating the product, not the
  underlying user pain. Rewrite to name the actual struggle.
- **A Product list that's a roadmap.** Eight bullets with dates and
  version numbers is planning, not vision. Cut to the five that define
  the product; the rest belongs in the backlog.

## Updating a Vision

Visions don't get touched often. When the PM asks for an update,
read the existing body first and present both old and new in the
proposal so the PM can spot anything they want to preserve.

If the PM wants to clear the Vision entirely, use
`clearProjectVision` (or pass an empty `markdownBody` to
`setProjectVision`). Confirm explicitly — clearing is a real action,
not a typo path.
