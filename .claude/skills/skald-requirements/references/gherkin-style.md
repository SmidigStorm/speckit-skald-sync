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
- **No Scenario Outlines in Skald** (house rule). Where classic Gherkin would use a Scenario Outline with an examples table, write **one example per variant on the rule** instead — each variant is its own scenario. Skald's examples are read one at a time on the requirement; an outline's parameter table doesn't render as such and hides which variant matters.
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

Skald scenarios are written **without `Background`** — each example stands alone so it reads in isolation in the requirement detail. (Cucumber allows `Background`; Skald's convention is to inline the `Given`.)

## One rule → one or more examples

In Example Mapping terms, a **rule** (acceptance criterion) is illustrated by **one or more examples**. Write the **happy path first**, then the most valuable negative/edge example(s). Each example is one scenario obeying the Cardinal Rule. Don't pad — an example must show something the others don't (the Unique Example Rule: no redundant scenarios).

## Quick checklist before saving an example

- [ ] Exactly one `When`→`Then` behaviour (Cardinal Rule)
- [ ] Declarative, ≤ ~10 steps, third person, present tense
- [ ] Concrete but defensively-chosen data
- [ ] Title: one line, no conjunctions, no `verify`/`assert`/`should`
- [ ] Readable by someone who's never seen the feature (Golden Rule)
- [ ] No `Background`, no `Or`

## Sources

- Andy Knight, *BDD 101: Writing Good Gherkin* — `https://automationpanda.com/2017/01/30/bdd-101-writing-good-gherkin/`
- *4 Rules for Writing Good Gherkin* (Gherkin's Golden Rule, the Cardinal Rule of BDD, the Unique Example Rule, the Good Grammar Rule) — `https://automationpanda.com/2020/02/21/4-rules-for-writing-good-gherkin/`
- *Good Gherkin Scenario Titles* — `https://automationpanda.com/2018/01/31/good-gherkin-scenario-titles/`
- *Are Gherkin Scenarios with Multiple When-Then Pairs Okay?* — `https://automationpanda.com/2018/02/03/are-gherkin-scenarios-with-multiple-when-then-pairs-okay/`
- *Should Gherkin Steps Use First-Person or Third-Person?* — `https://automationpanda.com/2017/01/18/should-gherkin-steps-use-first-person-or-third-person/`
- *Should Gherkin Steps use Past, Present, or Future Tense?* — `https://automationpanda.com/2021/05/11/should-gherkin-steps-use-past-present-or-future-tense/`
- *BDD Example Mapping* — `https://automationpanda.com/2018/02/27/bdd-example-mapping/`
- *In BDD, What Should Be A Feature?* — `https://automationpanda.com/2017/10/19/in-bdd-what-should-be-a-feature/`
