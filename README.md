# Spec Kit ↔ Skald Sync (Basic)

A [GitHub Spec Kit](https://github.com/github/spec-kit) preset that makes the Spec Kit flow
and **Skald** (an AI-native product-management app — the source of truth for requirements)
move as one. **One Skald backlog item (PBI) = one Spec Kit feature**: the PBI's journey from
"Refining Requirements" to "Done" *is* the Spec Kit flow.

> This repository is a **read-only mirror**, auto-synced from the preset's source of truth.
> Issues and PRs here won't be picked up.

## Prerequisites

- Spec Kit ≥ 0.6.0 with preset support.
- The Skald MCP server connected in your agent runtime (tools appear as `mcp__skald__*`).

## Bundled methodology skills

The `skald-*` methodology skills the commands reference are **included in this repo** under
[`.claude/skills/`](.claude/skills) — `skald-strategy`, `skald-requirements`, `skald-planning`,
`skald-goals`, `skald-domain-management`, and `skald-health-check`. These are the **single
source of truth**, published straight from the Skald app (the same files the in-app agent
loads); this mirror is regenerated on every change.

The commands carry the *workflow* (when to run, what to read/write, status transitions,
confirmation discipline) and reference these skills for the underlying *methodology* (Perdoo
OKRs, Gherkin conventions, Entity/Process/Term classification, INVEST splitting, the health
scan rubric).

**To use them:** copy `.claude/skills/` into your project. Claude Code discovers skills there
automatically; other agents can point their skill loader at the same folder (or move them to
wherever your runtime expects). Because the skills travel with this repo, the preset works
straight out of a clone — no separate skill install.

## What it does

### Appends to the core flow

| Command | Sync obligation |
|---|---|
| `speckit.specify` | Resolve/create the ONE feature PBI (`Refining Requirements`); adopt-or-create linked Requirements (`Draft`) + rules + Gherkin examples; open questions from `[NEEDS CLARIFICATION]`; release assignment (suggest or create); goal-link question; `skald:` frontmatter + "Skald traceability" line |
| `speckit.clarify` | Answer/raise open questions; update rules/examples from answers; on approval: Requirements → `Approved`, PBI → `Ready for Planning` |
| `speckit.plan` | Catch-up approval guard; open questions; `Blocked` requirement handling; short plan summary appended to the PBI description (`## Plan (specs/NNN-name)`) |
| `speckit.tasks` | Annotate `tasks.md` with PB-x + the story→requirement map; **no status change** |
| `speckit.analyze` | Read-only two-way drift table (spec vs Skald) in the analysis report |
| `speckit.implement` | Start → PBI `In Progress`; story checkpoint green → that Requirement `Delivered`; feature gate green → PBI `Done`; honesty rule: never on a failing gate |

### New optional commands

Feature-adjacent (recommended by the appends when relevant, never required):

```
[specify-vision] → specify → [domain-knowledge] → clarify → [design-brief → (external design) → design-read] → plan → tasks → analyze → implement
```

Project-level / cadence:

| Command | Purpose |
|---|---|
| `/speckit-product-health` | Read-only full-product scan → 1–5 routed next steps; onboarding entry point |
| `/speckit-specify-vision` | Draft/refine the project Vision + User groups |
| `/speckit-domain-knowledge` | Entities/processes/terms — feature mode or standalone discovery |
| `/speckit-requirements-workshop` | Spec-by-Example workshop on one requirement |
| `/speckit-backlog-refinement` | Create/split/size PBIs until one is ready to specify |
| `/speckit-goals` | OKRs: time periods, objectives, key results, weekly check-ins |
| `/speckit-release-planning` | Release lifecycle: contents, dates, Planned→In Progress→Released |
| `/speckit-design-brief` / `/speckit-design-read` | Claude Design handoff out / in (repo design system always wins) |

## Decisions baked in

- Requirement statuses: `Draft` at creation → `Approved` at spec approval → `Delivered` per
  green story checkpoint (`Blocked` while a question prevents planning).
- PBI statuses: `Refining Requirements` → `Ready for Planning` (approval) → `In Progress`
  (first task) → `Done` (green gate).
- Adopt before creating: existing catalog requirements are linked/refined, never duplicated.
- All writes obey Skald MCP discipline: read before write, plain-language summary, **one
  confirmation per write, no deletes** (archive only, explicit).
- Skald unreachable → skip with a `TODO(skald-sync)` marker; a sync step never blocks a
  Spec Kit step.
- The spec ↔ Skald link lives in `spec.md` frontmatter (`skald: { project, projectId, pbi,
  pbiId }`) plus a human-readable "Skald traceability" line.

**Command overrides only.** The runtime template resolver is winner-takes-all for templates,
so the preset composes exclusively through command appends (into
`.claude/skills/speckit-*/SKILL.md`) and net-new commands.

## Install

```bash
specify preset add --dev /path/to/speckit-skald-sync --priority 30
```

Pick the priority so this preset's appends land **after** any other command-appending presets
(higher number = later append) — the Skald writes should be the last thing each command does.

## Verify

```bash
grep -ln "speckit-skald-sync preset" .claude/skills/speckit-*/SKILL.md   # the six appends
ls .claude/skills/ | grep -E "speckit-(product-health|specify-vision|domain-knowledge|requirements-workshop|backlog-refinement|goals|release-planning|design-brief|design-read)"
```
