# Tool catalogue for the Skald Requirements skill

The tools you'll touch when working on requirements, grouped by purpose.
Every project-scoped tool takes a `projectId` argument — call
`listProjects` first if you don't have one. Names are identical on both
agentic surfaces (in-app Skald Agent and Skald MCP server) per
Constitution V (Agent–MCP parity).

## Read tools (no confirmation needed)

Use these freely. They're how you ground every conversation in the real
state of the product before proposing changes.

### Primary

- **`listProjects()`** — every project in the org. Always the first call
  when you don't know which project to act in.
- **`listRequirements({ projectId })`** — every requirement in the
  project with id, displayId, title, type, priority, domainId, status.
- **`getExistingRequirements({ projectId })`** — same data, slightly
  richer shape, useful when reviewing the backlog for gaps or
  duplicates.
- **`listRules({ requirementId })`** — the rules attached to one
  requirement.
- **`listExamples({ requirementId })`** — the examples attached to one
  requirement.
- **`listOpenQuestions({ requirementId })`** — the open questions
  attached to one requirement.
- **`getRequirementForSplit({ id })`** — pre-flight read before
  `splitRequirement`. Returns the rules, examples, and open questions
  so you can plan how they should land on the new requirements.

### Adjacent reads worth doing before a write

These don't return requirement data, but you usually want them to write
a *good* requirement. Use them in the read-before-write step of the
Writing rules.

- **`listDomains({ projectId })`** — pick the right home domain. A
  requirement that lives under the wrong domain confuses everyone.
- **`listDomainTerms({ projectId })`** — surface the project's existing
  ubiquitous-language terms so the requirement reads consistently.
  Skald's glossary distinguishes Entity / Process / Term — reuse the
  exact titles where the term applies.
- **`listGoals({ projectId })`** — check whether the requirement
  supports an existing Objective or Key Result, and offer to link.
- **`listPbis({ projectId })`** — check whether an existing PBI already
  covers this capability before drafting a new requirement.

## Write tools (Writing rules apply)

A write the PM asked for happens straight away, followed by a summary of
what was written. Propose first, and write only after a go-ahead, for
content you drafted rather than the PM, or an archive, restore or
destructive write (the skill's
Writing section).

### The requirement itself

- **`createRequirements({ projectId, items: [...] })`** — create one or
  more requirements in one call. Batch by domain when possible.
- **`updateRequirement({ projectId, id, ... })`** — change title, type,
  priority, status, or the home **domain** on an existing requirement. Pass
  `domainId` (a domain in the same project, from `listDomains`) to re-home it,
  or `domainId: null` to clear the assignment. A domain in another org or
  project is rejected by the tenancy check.
- **`splitRequirement({ projectId, id, splitInto: [...] })`** — split
  one requirement into multiple new ones, preserving rules / examples /
  open questions as mapped. Always call `getRequirementForSplit` first.

### Rules

- **`createRules({ requirementId, rules: [...] })`** — attach one or
  more rules to a requirement.
- **`updateRule({ id, ... })`** — change a rule's text.
- **`archiveRule({ id })`** — soft-remove a rule. Skald has no delete
  tool by design.

### Examples

- **`createExamples({ ruleId, examples: [...] })`** — attach one or
  more Gherkin examples to a rule.
- **`updateExample({ id, ... })`** — change an example.
- **`archiveExample({ id })`** — soft-remove an example.

### Open Questions

The PM decides which open questions get recorded. You may surface them
in conversation freely, but only call these tools when the PM
explicitly chooses to record / resolve one.

- **`createOpenQuestions({ requirementId, questions: [...] })`** — record
  one or more open questions on a requirement.
- **`answerOpenQuestion({ id, answer })`** — record an answer; the
  question moves to Answered state.
- **`dismissOpenQuestion({ id, reason? })`** — declare the question
  no longer relevant; the question moves to Dismissed.
- **`reopenOpenQuestion({ id })`** — flip an Answered or Dismissed
  question back to Open.
- **`archiveOpenQuestion({ id })`** — soft-remove the question entirely.

## What you will NOT find in this catalogue

By design, Skald has no delete tools. Destructive operations go through
`archive*` instead, which preserves history and lets you reverse the
operation if needed. Don't simulate deletion via `update*` either —
that breaks the audit trail.

Skald's MCP server also doesn't expose tools that change tool
inventories themselves, or that bypass tenancy checks. Every write
verifies `verify*Ownership(uuid, orgId)` per CLAUDE.md's multi-tenancy
rules; an `orgId` your auth token doesn't own will fail at the
ownership check, not at the tool's input validation.
