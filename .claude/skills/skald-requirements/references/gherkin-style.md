# Writing good Gherkin examples

How to write the Gherkin scenarios that illustrate a rule in Skald. Distilled from Andy Knight's *Automation Panda* BDD series (sources at the bottom). When an example you draft conflicts with a rule below, the rule wins — propose the fix to the PM.

## The two rules that override everything

1. **The Golden Gherkin Rule** — write Gherkin so that someone who does **not** know the feature can understand it. Business language, complete sentences, clarity over cleverness. The scenario is a specification first and a test second.
2. **The Cardinal Rule of BDD — one scenario, one behaviour.** Each scenario covers exactly **one** behaviour, expressed as **one `When`→`Then` pair**. If you have two `When`→`Then` pairs, you have two behaviours — split them into two scenarios.

## Behaviour-driven, not procedure-driven

The classic beginner mistake is transcribing click-by-click instructions into Gherkin. Specify *behaviour*, not *mechanics*.

- ❌ Procedure-driven (imperative, two behaviours in one scenario):
  ```gherkin
  Given the user opens a browser
  And the user navigates to the org page
  When the user clicks "New project"
  And the user types "Apollo"
  And the user clicks Save
  Then the project appears
  When the user clicks "Connect team"
  Then the team is connected
  ```
- ✅ Behaviour-driven (declarative, one behaviour each):
  ```gherkin
  Scenario: Creating a workspace adds it to the active list
  Given the organisation has no workspace named "Apollo"
  When the member creates a workspace named "Apollo"
  Then "Apollo" appears in the active workspace list
  ```

**Declarative over imperative.** Describe *what* happens at a business level; leave *how* (clicks, keystrokes, selectors) to the step definitions. Aim for **≤ 10 steps** per scenario — usually far fewer.

## Step grammar

- **Third person, never first person.** "the member creates…", not "I create…". Mixed point of view is ambiguous and kills step reuse.
- **Present tense throughout.** `Given` the state *is* established; `When` the actor *does* the action; `Then` the system *shows* the outcome. Avoid "the user will…" / "the user navigated…".
- **Subject–predicate sentences.** Every step (including `And` lines) names its subject. ❌ "And duplicate names rejected" → ✅ "And the system rejects the duplicate name".
- **No `Or`.** Gherkin has no `Or` — every step runs in sequence.
- **Scenario Outlines are allowed** (house rule flipped by spec 080 — full Gherkin 6). Use one when the *behaviour* is identical and only values differ; the outline body plus its `Examples:` table live in the example's gherkin text. Do NOT use one when behaviour genuinely differs between rows — that is separate scenarios wearing a table as a disguise. Keep tables short; a table with twenty rows is a unit test that escaped.
- **`Given` = state, not action.** Set up the world with declarative state ("the organisation has a team named Platform"), not by re-driving earlier behaviour.

## Specific data, defensively chosen

- Push for **concrete data** in `Given` and `Then`: "Given a workspace named 'Apollo' with 2 connected teams" beats "Given a workspace".
- But **data defensively** — assert the *general* property the behaviour guarantees, not brittle incidental detail. Assert "the archived workspace is absent from the switcher", not "the switcher shows exactly 3 specific names".

## Scenario titles

- **One short line.** A long title means the scenario covers too much.
- **No conjunctions** (`and`/`or`/`but`) and **no rationale words** (`because`/`so`/`since`) — those signal multiple behaviours or leak the *why*.
- **No test/assertion words** — drop `verify`, `assert`, `should`. State the behaviour as fact.
- ❌ "Verify the user can rename a project and see it update everywhere" → ✅ "Renaming a workspace updates its name across the app".

## Background

Skald supports `Background` at both levels (spec 080 — full Gherkin 6): a requirement's Background applies to all its examples, a rule's Background to that rule's examples. Where both exist, both apply, requirement-level first — Gherkin's own semantics, nothing invented. Store steps only (no `Background:` header line).

**Use a Background when two or more examples under the same parent share the same leading `Given` steps** — repetition there is noise that drifts. A Background for one example is noise of a different kind: inline it instead.

Conventions (adopted verbatim from the Skald-SDD kit's `background-and-outlines.md` — one style guide, not two):

- **`Given` only.** Never a `When`, never a `Then`. Setup, not action, not assertion. (Skald stores whatever you write — validation happens when feature files are generated — so discipline here is the guard.)
- **Keep it under four lines.** The reader has to hold it in their head while reading every example below it.
- **Do not set up complicated state.** Use a higher-level step: `Given Ingrid is signed in as an organisation owner`, not six steps constructing that condition.
- **Make it vivid.** Colourful, specific names telling a small story — `the Platform team`, not `Team A`.
- **If only some examples need the setup, put it in those examples.** A Background half the examples ignore is misleading — the reader assumes it applies.
- **Examples must not restate what their Background establishes** — that reintroduces the drift the Background exists to remove.

**Hoisting is the default drafting shape.** When you draft or edit examples that share leading `Given` steps, shape the proposal with the shared setup already extracted into the Background — the one confirmation covers the whole proposal; never ask a separate "may I hoist?" question. Never restructure stored examples you were not asked to touch.

## One rule → one or more examples

In Example Mapping terms, a **rule** (acceptance criterion) is illustrated by **one or more examples**. Write the **happy path first**, then the most valuable negative/edge example(s). Each example is one scenario obeying the Cardinal Rule. Don't pad — an example must show something the others don't (the Unique Example Rule: no redundant scenarios).

## Quick checklist before saving an example

- [ ] Exactly one `When`→`Then` behaviour (Cardinal Rule)
- [ ] Declarative, ≤ ~10 steps, third person, present tense
- [ ] Concrete but defensively-chosen data
- [ ] Title: one line, no conjunctions, no `verify`/`assert`/`should`
- [ ] Readable by someone who's never seen the feature (Golden Rule)
- [ ] Shared setup hoisted to the right-level `Background` (2+ examples), no `Or`

## Sources

- Andy Knight, *BDD 101: Writing Good Gherkin* — `https://automationpanda.com/2017/01/30/bdd-101-writing-good-gherkin/`
- *4 Rules for Writing Good Gherkin* (Gherkin's Golden Rule, the Cardinal Rule of BDD, the Unique Example Rule, the Good Grammar Rule) — `https://automationpanda.com/2020/02/21/4-rules-for-writing-good-gherkin/`
- *Good Gherkin Scenario Titles* — `https://automationpanda.com/2018/01/31/good-gherkin-scenario-titles/`
- *Are Gherkin Scenarios with Multiple When-Then Pairs Okay?* — `https://automationpanda.com/2018/02/03/are-gherkin-scenarios-with-multiple-when-then-pairs-okay/`
- *Should Gherkin Steps Use First-Person or Third-Person?* — `https://automationpanda.com/2017/01/18/should-gherkin-steps-use-first-person-or-third-person/`
- *Should Gherkin Steps use Past, Present, or Future Tense?* — `https://automationpanda.com/2021/05/11/should-gherkin-steps-use-past-present-or-future-tense/`
- *BDD Example Mapping* — `https://automationpanda.com/2018/02/27/bdd-example-mapping/`
- *In BDD, What Should Be A Feature?* — `https://automationpanda.com/2017/10/19/in-bdd-what-should-be-a-feature/`
