# Writing good Users in Skald

A User row in Skald describes a **kind of user** the product is built
for — a persona, a user segment, a user type. Skald uses one table for
all of these; there's no separate Segment table.

In Product Vision Board terms (see `SKILL.md`), the User rows **are**
the *Target Group* box, and their goals and pains **are** the *Needs*
box — the interview's "who is it for?" and "what do they need?" land
here as durable, reusable rows rather than one-off canvas notes.

The agent's job is to help the PM articulate users clearly. **Don't
invent users.** Goals and pains in particular are easy to fabricate;
ask the PM to describe the user in their own words and capture that.

## The shape of a User row

Each row has:

- **`name`** (required) — short, plain English. One to four words. Use
  the noun the PM uses out loud.
- **`descriptionMarkdown`** (optional) — who this user is in 1–3
  sentences. Context, role, situation.
- **`goalsMarkdown`** (optional) — what this user is trying to achieve
  when they touch the product. Bullets usually beat prose.
- **`painsMarkdown`** (optional) — what's frustrating or broken for
  them today. Also bullets.

All three Markdown fields are optional, but a User with no description,
goals, *or* pains isn't telling anyone anything. Push for at least one
substantive field before calling `addProjectUser`.

## Good User examples

### Example 1 — Product Manager

```
name: Product Manager
```

```markdown
descriptionMarkdown:

A senior IC who is accountable for the direction of one product across
multiple delivery teams. Owns the backlog, runs requirements workshops,
and reports outcomes to leadership.
```

```markdown
goalsMarkdown:

- Keep the backlog refined enough that any team can pick the next PBI.
- Articulate vision and OKRs in a way the whole org understands.
- Stop juggling three tools for goals, requirements, and planning.
```

```markdown
painsMarkdown:

- Tooling that lets process drift; rules and examples scattered across
  Confluence pages.
- Manual translation between PM artefacts and engineering specs.
- Status reporting that requires copy-paste from three sources.
```

### Example 2 — External Stakeholder

```
name: External Stakeholder
```

```markdown
descriptionMarkdown:

Someone outside the organisation who's been shared a read-only link to
a requirement or backlog item. May not have a Skald account; may or may
not know the product context.
```

```markdown
goalsMarkdown:

- Understand the shared artefact in under 60 seconds.
- Leave a focused comment the PM will actually see.
```

```markdown
painsMarkdown:

- Shared links that drop them into a login wall.
- Shared artefacts with no context — they don't know what product or
  team this belongs to.
```

## What makes a User useful

A User row earns its keep when it shows up downstream:

- In **Requirements**, the `<role>` in a User Story title should
  ideally be a Project User name. *"As a Product Manager, I want…"* is
  much more useful than *"As a user, I want…"* because *Product
  Manager* is a known row with goals and pains.
- In **PBI refinement**, you can ask *"which User is this PBI for?"*
  and have a real answer. A capability with no matching User is a
  flag — either the capability doesn't matter, or the User list is
  incomplete.
- In **discussions about scope**, naming a User narrows the
  conversation. "This is a Product Manager feature, not a Stakeholder
  feature" reframes a debate fast.

## When to add a new User vs reuse one

Surface the existing list (`listProjectUsers`) before adding a new
User. Most "new" users are actually a sub-flavour of an existing row
(*"a Product Manager doing their first month with the tool"* is still a
Product Manager — capture the nuance in the description or pains).

Add a new User when:

- The role has different **goals** from existing Users.
- The role has different **pains** from existing Users.
- The role shows up in Requirements with a different `<role>` than any
  existing User.

Don't add a new User when:

- It's the same role with a different job title at a different company.
- It's a temporary state of an existing User ("Product Manager on
  trial week 1") — capture that in the description.

## Anti-patterns

- **"User"** as a name. Too generic. If the row matters, it deserves a
  more specific name. If it doesn't matter, don't add the row.
- **Goals that are the product's features.** *"Use Skald to manage
  requirements"* is not a goal — the user's goal is the underlying
  thing they're trying to do, not the way Skald lets them do it.
- **Pains that are the product's gaps.** *"Skald doesn't have a
  share-link revocation flow yet"* belongs in a PBI, not in a User's
  pains. Pains are the **outside-the-product** struggles.
- **Inventing goals or pains the PM didn't say.** The agent's job is
  to capture, not to imagine. If goals or pains are missing, ask.

## Editing and archiving

- Use `updateProjectUser` to refine a row. Pull the current row first
  so you can show old vs new in the confirmation summary.
- Use `archiveProjectUser` to soft-remove a User that no longer
  applies. Skald has no hard delete by design.
- A User row's `name` is referenced by Requirements (as `<role>`).
  Renaming a User doesn't auto-rewrite the requirements — flag this
  when proposing a name change.
