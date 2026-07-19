# EARS sentence patterns

EARS — Easy Approach to Requirements Syntax — was introduced by Alistair
Mavin at Rolls-Royce. It gives you six sentence shapes for writing
testable system requirements without slipping into prose. In Skald, use
EARS shapes as **guidelines**, not rigid templates: the goal is clarity,
not template fidelity.

Use EARS for requirements that describe platform behaviour the user
doesn't see directly — cron jobs, audit emits, retention purges,
webhook handlers, validation rules, rate limits. For user-visible
capabilities, use a User Story title instead.

## The six shapes

### 1. Ubiquitous

> The `<system>` shall `<response>`.

Use for behaviour that's always true, with no trigger or precondition.

Skald examples:
- The audit log shall record every MCP write call with the actor's
  user id.
- The product backlog shall preserve the global sort order across
  page reloads.

### 2. Event-driven

> When `<trigger>`, the `<system>` shall `<response>`.

Use for behaviour triggered by a specific event.

Skald examples:
- When a Clerk `user.deleted` webhook arrives, the audit log shall
  record a pseudonymization event for that user.
- When a domain is archived, the search index shall mark every
  artefact under that domain as archived in the same transaction.

### 3. State-driven

> While `<state>`, the `<system>` shall `<response>`.

Use for behaviour that applies whenever the system is in a particular
state — different from an event because it's ongoing.

Skald examples:
- While the MCP feature flag is off, the `/api/mcp` route shall
  return 404 to every request.
- While a requirement is in Draft, the agent shall surface open
  questions rather than mark the requirement Done.

### 4. Optional

> Where `<feature>`, the `<system>` shall `<response>`.

Use for behaviour that only applies if a particular feature, mode, or
configuration is present.

Skald examples:
- Where the project has a configured Vision, the dashboard shall
  display the Vision text above the goals widget.
- Where the user is an org admin, the data-export download shall
  include the full org export bundle rather than the user export.

### 5. Unwanted Behaviour

> If `<trigger>`, then the `<system>` shall `<response>`.

Use for behaviour the system performs when something undesirable
happens — invalid input, missing data, attacks, etc.

Skald examples:
- If a Skald MCP caller passes a `projectId` outside their
  organization, the server shall return JSON-RPC error code -32004
  (not-found) without revealing whether the project exists.
- If a search query against an artefact in another org leaks through
  the index, the rejection counter shall increment for that org.

### 6. Complex

EARS allows combining the building blocks above when a single shape
isn't enough. The pattern is:

> When `<trigger>`, while `<state>`, the `<system>` shall `<response>`.

Use sparingly. If you find yourself stacking more than two
preconditions, the requirement is probably too big — try the INVEST
splitting heuristic instead.

Skald example:
- When the retention purge cron runs, while there are archived
  artefacts older than 30 days, the system shall delete those rows
  and write one `retention_purge` audit event per affected table.

## Anti-patterns

Watch for these and rephrase:

- **Solution leakage**: "shall use a Postgres trigger to update the
  search index" — the trigger is the implementation, not the
  capability. Rephrase as: "shall update the search index when any
  indexed artefact changes."
- **Vague responses**: "shall handle invalid input gracefully" — what
  does "handle gracefully" mean? Specify the actual response.
- **Multiple requirements in one sentence**: "shall log the event and
  return 200 and update the user's profile" — split into separate
  requirements.

## Sources

- *Easy Approach to Requirements Syntax (EARS)*, Alistair Mavin et al.,
  2009. Originally published in *Requirements Engineering Conference
  (RE)*.
- *The Pocket Guide to Writing Better Requirements with EARS*,
  Alistair Mavin.
