# How we write a specification

The shape of a requirement in Skald at all four levels: its title, its description, its rules and
its examples. Settled during the
2026-09 requirements reconciliation and written down because the same mistakes were made twice, in
different words each time.

Every rule here has a reason. Where the reason is a count, the count is given, because a style rule
with no evidence behind it is a preference and gets argued with forever.

---

## 1. The title is the capability, in the reader's words

Short. A noun phrase or an imperative. What someone would ask for by name.

> Create a requirement · Export the requirements list · Supersede a decision when the answer changes

**Not the user story.** That goes in the description (§3). A title carrying the whole story runs 26
words against 14, and when 148 of 180 begin "As a product manager, I want…", the first five words of
every row in a list carry no information at all. A title is read in a list, in a palette result and
in a pasted link, and none of those has room for a sentence.

**Not a summary, and not a name with an explainer bolted on.** If a title needs a colon, what
follows the colon usually belongs in the description.

**Not a claim about behaviour.** A capability is something a person does; a claim is something the
product is true about. The difference is a verb somebody would use:

| Claim-shaped | Capability-shaped |
|---|---|
| A workspace's tabs keep what you did in them | Pick up a tab where you left it |
| A workspace has its own address | Send someone a link that works for them |
| Several conversations in one workspace | Have several conversations at once |
| Start a workspace from a prepared layout | Open a ready-made set of tabs |
| An empty workspace offers somewhere to start | See what to do in an empty workspace |

The left column is what the domain's own vocabulary produces when you describe a feature to
yourself. The right is what somebody asks for. Where the two differ, the title is the second one and
the first belongs in the description, if anywhere.

## 2. A CRUD capability takes the CRUD verb

Create. View. Update. Archive. Plainly, and repeated as often as the product repeats the operation.

**Never "Delete".** Constitution V.5: archive is the destructive verb on every Skald surface. A
title promising a delete promises something the product will not do.

The failure this prevents is **elegant variation**: reaching for a synonym to avoid repeating a
word. A first pass produced *Capture* for create, *Correct* for update, *Mint* for create-a-link,
*Ground* for add and *Take the requirements list out of Skald as a spreadsheet* for export. Each
reads fine alone. Twenty together read as generated, because no writer varies a verb that hard
unless something is making them.

**Not everything is CRUD.** Split, connect, generate, tag, supersede and the share-link viewer's
read are not operations on one row, and forcing them into CRUD verbs is the same error inverted.
The test in both directions: *would the reader say this word?*

## 3. The user story is the first line of the description

```
As a ‹persona›, I want ‹capability›, so that ‹benefit›.
```

Then a blank line, then the narrative.

**The benefit has to earn its place.** If the `so that` only restates the capability backwards, cut
it or find the real one. Twenty-two of 180 first-pass stories did exactly this:

> ✗ I want to find the decisions that apply, **so that I do not have to read the whole log**
> ✓ I want to find the decisions that apply, **so that I can check what we already settled before re-opening it**

The first negates the problem. The second names what the capability is *for*. This happens because
the template demands a benefit and the writer had no second thing to say, so it reached for the
problem and put "not" in front of it.

**Personas are argued per requirement**, not assigned from a table. One persona on 130 of 148
stories is a fixed prefix rather than a persona. If every requirement in a domain has the same
persona, that may be true. It may also be that nobody looked.

## 4. Internal capabilities take EARS, not a story

A user-facing capability gets a story. **Platform behaviour nobody asks for directly** gets EARS:
retention purges, webhook handling, migration guards, audit, rate limits, credentials, billing sync,
observability, CI/CD and test strategy.

> When a user's account is deleted, Skald shall erase them from every store that names them.

Nobody asks to be erased by a webhook. Writing it as a story invents a user who does.

EARS titles came through the reconciliation clean where stories did not, because the template leaves
no room for flourish. That is a feature.

**The title is the imperative core, not the whole EARS sentence.** The condition and the *shall*
belong in the description and the rules; the title says the behaviour. Every EARS title the
reconciliation has written looks like this:

> Limit how hard one person's agent can push · Record every external write in the audit trail ·
> Delete assistant recordings after thirty days · Block merge to main unless all required checks are
> green · Capture every schema change as a journalled migration

**Never "the system shall".** Skald is one system, so naming it carries nothing, and the phrase adds
four words to a title read in a list. The Database pass first wrote its six titles as textbook EARS
sentences — *"When a database schema changes, the system shall capture it as an ordered migration
file recorded in the journal"* — which ran 14 to 19 words against the 7 to 11 of every EARS title
already in Skald. The author had reached for the EARS reference instead of looking at the thirty
EARS titles the campaign had already written. Look at those first.

**One subject per domain.** That same pass mixed *the system shall*, *the CI gate shall* and
*Production migrations shall* across six titles. Inside one domain that reads as inattention.

## 5. The narrative says why, not what

The rules already say what. The description is for what the reader cannot reconstruct:

- **what the capability is for**, in terms of the person doing the work;
- **why it is shaped the way it is**, especially where an obvious alternative was rejected;
- **what goes wrong without it**, concretely;
- **the one fact likely to surprise somebody**, stated rather than left to be discovered.

Cross-reference by requirement id where another requirement owns a neighbouring claim. Say plainly
where a promise stops: *"there is nowhere in Skald a person can go and read it back"* is worth more
than silence, and more than a promise the capability cannot keep.

## 6. A rule is one sentence

A rule that needs a paragraph is two rules. It may carry an explanation beneath it for reasoning
that would otherwise be lost. Most rules do not need one, and forcing a paragraph out of somebody
produces filler.

Rules stay in the order they were written. That order is the order of the conversation.

**Rule count is a prompt to look, never a verdict.** A requirement is one capability; if thirteen
rules are really three capabilities, split it, and if six rules are one capability, leave it.

## 7. An example's title says what happens, in the words of the case

An example is a concrete scenario. Its title names that case plainly enough that a reader knows what
the scenario does without reading the steps.

This is the level where compressed, clever phrasing does most damage, because an example title is
read in a list of twenty and in a failing CI report, where nobody has the steps in front of them.
Real titles from the first pass, and what they should have said:

| Written | Says |
|---|---|
| Coverage hidden on screen is still in the file | Hidden columns are still in the exported CSV |
| Hiding detail leaves the map and takes the acceptance work away | Map mode hides rules and examples but keeps every requirement |
| The same narrowing asked of both gives the same four requirements | The table and the treemap show the same requirements under the same filter |
| A canvas with no domain chosen carries every active requirement | With no domain filter the canvas shows every requirement |
| Setup written on a requirement stands above its rules and stays there | A Background shows above the rules and survives a reload |

The pattern in the left column is a title written to *mean* something rather than to *say* it: the
subject is abstracted ("coverage", "the acceptance work", "the same narrowing"), the verb is doing
metaphorical work ("carries", "leaves", "stands"), and the reader has to decode before they can
judge. The right column names the thing on the screen and the thing that happens to it.

**The test:** read the title alone. Do you know what the scenario checks? If you have to open the
steps to find out, the title is not finished.

**Concrete over general.** *"A 499 kr basket shows the banner"* beats *"An order below the threshold
shows the banner"*. The number is the point of an example; a title that generalises it back out has
turned the example into a second copy of its rule.

## 8. The tells to strike out

From Wikipedia's *Signs of AI writing*, filtered to the ones that actually showed up. The first pass
over 21 requirement narratives carried 64 em dashes in 4,055 words.

| Tell | Looks like | Instead |
|---|---|---|
| **Em and en dashes** | anything using `—` as punctuation | a full stop, a comma, a colon, or parentheses |
| **Negative parallelism** | "A requirement is not a record of what somebody once thought; it is…" | say what it is |
| **Elegant variation** | Create → Capture → Mint → Ground, across neighbouring titles | repeat the word |
| **Manufactured punchlines** | "A log that anyone can delete from is not a log." | say the claim |
| **Aphorism formulas** | "Archiving is the middle road", "a month-long grace period, not a second filing cabinet" | say the mechanism |
| **Rule of three** | "superseded, rejected, or overtaken by events" | as many as are true |
| **Tailing negation** | "…, no guessing." | write the clause |
| **Copula avoidance** | "serves as", "represents", "stands as" | "is" |
| **-ing analysis tails** | "…, ensuring the member never loses their place." | stop at the full stop |
| **Signposting** | "This requirement covers…" | start with the content |
| **Diff-anchored prose** | "PB-184 restated the destination without changing the guarantee" | describe the thing as it is now |
| **The aphoristic closer** | a quotable last sentence on every paragraph | let most paragraphs end on the fact |
| **"rather than" as a tic** | "telemetry rather than conversation", "a decision rather than a deficit" | say what it is; use the contrast once, where it earns its place |

**What not to strip.** Specific hard-to-fabricate detail (*"a 100-krone order earns 100 points"*),
an admission that something does not work, a stated trade-off, a sentence that is short because it
is emphatic. Plain technical prose is not a tell. Over-editing produces the other kind of generated
text: correct, even, and saying nothing.

## 9. Write to a stranger

The test a reviewer applies: **could someone who has never seen the code build this from the
requirement?** And: **would any rule here fail if the behaviour it describes were broken?**

A requirement that passes the first and fails the second is documentation, not a specification.

## 10. A requirement about a mechanism is still written in the reader's words

The trap has a shape: when the *subject* of a requirement is a piece of machinery, the machinery's
vocabulary feels like the precise choice, and it is the wrong one. The reader is not the person who
built it.

| Written | Says |
|---|---|
| Load a domain's methodology on demand | Apply the method that fits the work |
| Load the assistant's tools on demand | Never decline a capability Skald has |
| Reconstruct an assistant run that went wrong | Record every assistant conversation |
| Bound an organisation's monthly AI spend | Cap an organisation's monthly AI spend |
| A project-scoped assistant that knows the vision and users | The assistant knows which project you are in and what it is for |

All five were written in one pass over the Artificial Intelligence domain, which is what makes them
a pattern rather than five slips. The failures are three:

- **Implementation words.** *Load on demand* is how it is built. *Run* is a telemetry noun. *Bound*
  is what the code does to a number. None is a word a product manager says.
- **A title scoped smaller or wider than the requirement.** *…that went wrong* implies only failures
  are recorded; every conversation is. Check the title against the rules before keeping it.
- **A phrase that points at nothing in a list.** *Make the case for working this way*: which way?
  A title is read beside twenty others with no surrounding paragraph to lean on.

**The test:** read the title with no other context and ask what the product does. If the answer needs
the description, the title has not been written yet.


## 11. If nobody can ask for it, it is not a requirement

Before writing a title, ask whether a person could walk up and ask for the thing. If they could
not, it is a rule, a constraint, or a property of the product, whatever the user-story template
makes it look like.

The template is what hides this. **Any sentence at all can be forced into "As a ‹persona›, I want
‹X›, so that ‹Y›"**, and once it has been, it looks exactly like a requirement. Three in the
Workbench domain had been sitting in Skald as Delivered requirements for months:

| Stored as a requirement | What it actually is | Where it goes |
|---|---|---|
| *"I want the product to say project for the top-level container and workspace for the things in the rail, and the code to agree"* | a rule about how we write | the constraint register, and a backlog item for the glossary half |
| *"I want my workspaces to stay where I put them, so that opening one never moves it"* | a rule of the rail | rules of the rail's requirement |
| *"I want the product's shell anyway, so that I can still reach my account… and make a project"* | a state the product must survive | one rule each on the surfaces that must survive it |

Three tests that catch them:

- **A property of how the product is built or written** is a constraint. Vocabulary, naming,
  parity between two surfaces, ordering guarantees that no member chose.
- **A statement about one surface's behaviour** is a rule of that surface's requirement. "Things
  stay where I put them" is the rail behaving; the rail is the capability.
- **A state the product must survive** is not a capability either. "No project yet", "nothing in
  the backlog", "the platform is down" are conditions, and each surface's behaviour in that
  condition is a rule of that surface. An empty backlog belongs to the backlog list's requirement,
  not to a requirement called "the backlog with nothing in it".

The third is the one that produces the most convincing fakes, because the state is genuinely
important and genuinely needs specifying. It just needs specifying in several places rather than
one.

## 12. Two titles that say one thing are one requirement, or two badly named ones

When a pair reads as the same capability, one of two things is true and they need different fixes.

**They are the same, and one is a rule of the other.** *Make a workspace of your own* and *Create a
workspace* are one capability: the dialog is how you create one, not a second thing to create.
Fold it in.

**They are different and the titles hide it.** *Connect a coding agent to Skald* and *Set up
coding-agent access without asking anyone* read as one requirement and are two: the first is the
protocol surface working, the second is whether the instructions are complete enough to finish
alone. There was a hard reason to keep them apart, which the titles gave no hint of: one has no
browser and is specification only, the other is the domain's only runnable file. Retitled to *A
coding agent reads the product it is building* and *Everything needed to connect an agent is in one
place*, the difference is visible.

**The test:** read the two titles with nothing else. If you cannot say what one does that the other
does not, fix it now, because a reader with less context than you will not do better.


---

## Where this came from

Working agreement decisions 18, 20, 21, 22, 23 and 26 of the 2026-09-10 requirements reconciliation
(`docs/requirements-cleanup/working-agreement.md`). The counts are from the retitle review and the
narrative pass over the Requirements domain, recorded in
`.skald/explorations/2026-09-10-requirements-reconciliation/`. Section 10 and the last six rows of
§8's table come from the Artificial Intelligence pass. Sections 11 and 12, and §1's claim-versus-
capability table, come from the Workbench pass, where Arne removed three of the twelve proposed
requirements on the grounds that nobody could ask for any of them. All three had been sitting in
Skald as Delivered requirements, written as user stories, for months.

Related: `gherkin-style.md` for examples and Backgrounds, `ears-patterns.md` for the EARS shapes,
`invest-splitting.md` for when one requirement is two.
