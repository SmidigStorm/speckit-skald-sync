# The Example Mapping workshop

Skald's requirements workshop is **Example Mapping** — Matt Wynne's
four-card technique — practised inside **Specification by Example**
(Gojko Adzic): requirements get clearer when specified through concrete
examples, and those same examples serve as acceptance tests.

Skald's data model is the four cards, so the workshop writes straight
into the product:

| Card | Meaning | In Skald |
|---|---|---|
| 🟨 Story | the capability under discussion | the Requirement (title) |
| 🟦 Rule | an acceptance criterion that gates Done | a Rule row |
| 🟩 Example | a concrete case illustrating one rule | an Example row |
| 🟥 Question | an unknown nobody in the room can resolve | an Open Question row |

Two Skald twists on the classic technique: the red card gets a
**lifecycle** (answered / dismissed / reopened / archived) instead of
dying with the meeting, and the agent **never records a question on its
own initiative** — the human driving the session decides.

The agent's job is to **facilitate** the workshop, not to invent the
answers. The humans in the room (PM, and any team members joining) own
the content; you keep the structure tight and cover the missing
perspectives (see "Who's in the room" in `SKILL.md`).

## The four-step flow

### Step 1 — One story at a time

Pick one capability. Frame it as a User Story title and confirm the
parts:

```
As a <role>, I want <capability> so that <benefit>.
```

Sanity checks before going further:

- Is `<role>` a real persona in this product, not a placeholder like
  "user"? If not, ask which persona this is for.
- Is `<benefit>` user-visible value, not "because the architecture
  says so"? If not, ask why this matters.
- Is `<capability>` solution-free? If the PM said "add a dropdown",
  propose the solution-free alternative once.

### Step 2 — Rules together

Decompose the capability into rules. A rule is a single testable
acceptance statement.

Drive the conversation like this:

> "Walk me through how this should behave. What's the most important
> thing it has to do?"

Capture each answer as one rule. Treat the rules as a list, not a wall
of text. Use EARS shapes loosely where they help — *"When the link
expires, the share view returns a 410"* reads better than a paragraph.

Stop when:

- The rules collectively gate Done — any product that satisfies all
  the rules is acceptable; any product that fails one rule is not.
- Adding another rule would just elaborate on an existing rule rather
  than describe new behaviour.

### Step 3 — Examples per rule, informally first

For each rule, elicit at least one concrete case — **as a one-liner,
not as Gherkin**. This is Example Mapping's signature move: during the
map, an example is *"the one where…"*:

- *the one where the link has expired*
- *the one where the viewer isn't signed in*
- *the one where the PM revoked it yesterday*

Don't force Given/When/Then mid-conversation — it slows the room down
and turns elicitation into transcription. **Formal Gherkin is written
after the map is agreed**, when you assemble the write proposal; that's
where the style rules in [`gherkin-style.md`](gherkin-style.md) apply
(no Backgrounds, no Scenario Outlines — one example per variant, one
behaviour per scenario, declarative, specific data chosen defensively).

Coaching while mapping:

- Happy path first, then 1–2 negative cases per rule, then edge cases.
- Each example must show something the others don't — no redundancy.
- If a rule can't produce a single concrete example, the rule is
  probably vague — sharpen it or question it.

### Step 4 — Park questions, don't debate them

When uncertainty appears — contradicting examples, an unknown outcome,
a business call nobody present can make — **name it as a question and
move on**. The red card exists so the workshop doesn't burn its timebox
on unresolvables. Debate is a smell that a question card is missing.

Never call `createOpenQuestions` unprompted: surface the parked
questions at the end and ask which ones to record. The human driving
the session chooses.

## Reading the finished map

The map is a readiness instrument — read it before writing anything:

- **Covered in red** (many questions) → too uncertain to build. Propose
  chasing the answers before formalising; recording the questions in
  Skald *is* the deliverable of this session.
- **Covered in blue** (many rules) → look for a seam: two capabilities a
  user would ask for separately. If there is one, route to INVEST
  splitting (`invest-splitting.md`) and split along it. If there is not,
  the story is whole — many rules on one capability is not a defect.
- **A tidy map** — a handful of rules, an example per rule, few or no
  questions → ready. Formalise the examples and propose the writes.
- **The session drags** — a well-understood story maps in roughly 25
  minutes. A mapping that keeps sprawling is telling you about the
  requirement, not about the participants. Say so, and propose
  splitting or parking.

## What the workshop produces

By the end of one pass for a single requirement, Skald has:

- One requirement row (`createRequirements`).
- N rule rows attached to it (`createRules`).
- ≥ 1 example per rule (`createExamples`) — formalised as Gherkin at
  proposal time.
- 0..N open questions the human chose to record
  (`createOpenQuestions`).

If the agent is doing a workshop end-to-end, this means one
`createRequirements` call, one batched `createRules` call, one batched
`createExamples` call per rule, and a final batched
`createOpenQuestions` call if any were chosen. Each write is preceded
by a plain-language proposal — see the confirmation discipline in the
main `SKILL.md`.

## A worked example

The PM says: *"I want PMs to be able to share a requirement with
stakeholders who don't have a Skald account."*

### Step 1 — title

Proposed:

> As a Product Manager, I want to share a requirement with stakeholders
> outside Skald so that they can read it and comment without an account.

Confirm with the PM before moving on.

### Step 2 — rules

Surfaced from the conversation:

- The share link grants read-only access to one requirement.
- The share link works for stakeholders who are not signed into Skald.
- The share link expires automatically after a configurable window.
- The PM can revoke the share link at any time.
- A revoked share link returns a 410 to subsequent reads.
- A share-link viewer can leave a comment that the PM sees inside
  Skald.

(Six rules — bordering on "covered in blue". Worth saying out loud:
*"this is a lot of rules for one story; if commenting feels separable,
that's a natural split."*)

### Step 3 — examples, informally

For *"expires automatically after a configurable window"*:

- *the one where the stakeholder opens a 7-day link on day 9*

For *"a viewer can leave a comment the PM sees"*:

- *the one where an anonymous viewer asks "Why MoSCoW Must?"*

Formalised later, at proposal time:

```
Given a requirement "Drag-and-drop reprioritization" exists
And the PM creates a share link with a 7-day window on 2026-06-01
When a stakeholder opens the link on 2026-06-09
Then the share view returns a 410
And the response body mentions the link has expired
```

```
Given a stakeholder is viewing a shared requirement
When the stakeholder submits a comment "Why MoSCoW Must?"
Then the comment appears on the requirement detail page inside Skald
And the comment is attributed to "External viewer" with no email
```

### Step 4 — parked questions

- *"Should the share link include child requirements (split from the
  parent), or only the parent?"*
- *"What happens to existing share links if the requirement is
  archived?"*

The PM decides both are worth recording. The agent calls
`createOpenQuestions` after the proposal is confirmed.

## Sources

- *Introducing Example Mapping*, Matt Wynne, Cucumber blog, 2015.
- *Specification by Example: How successful teams deliver the right
  software*, Gojko Adzic, 2011.
- *Bridging the Communication Gap*, Gojko Adzic, 2009.
- Cucumber Gherkin syntax reference — `https://cucumber.io/docs/gherkin/`.
