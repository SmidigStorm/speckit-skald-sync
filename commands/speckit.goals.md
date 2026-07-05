---
description: Author and track OKRs in Skald — time periods, objectives, key results, weekly check-ins (speckit-skald-sync preset command).
---

# Goals / OKRs (`/speckit-goals`)

Project-level, cadence-driven: quarter boundaries (author/refine) and weekly (check-ins).
Methodology: follow the `skald-goals` skill — Perdoo-style. Hard rules: Objectives are
qualitative and inspiring; Key Results are **measurable outcomes, not outputs** ("increase
activation to 40%", never "ship the onboarding flow"); ≤5 KRs per Objective; KRs are
stretch-calibrated.

## Flow

1. **Read first**: `mcp__skald__getTimePeriods`, `listGoals` / `getExistingGoals` for the
   current state; `getCheckInHistory` when discussing progress.
2. **Author mode** (typically at a period boundary): create/refine the time period
   (`createTimePeriod` / `updateTimePeriod`), objectives (`createObjective` /
   `updateObjective`), and key results (`createKeyResult` / `updateKeyResult`). Push back on
   output-shaped KRs — propose the outcome behind them.
3. **Check-in mode** (weekly): for each active KR, record value + note via `createCheckIn`,
   reading `getCheckInHistory` for the trend. Surface KRs with no recent check-in.
4. **Connect delivery**: when discussing which work serves which KR, propose
   `linkKeyResultToBacklogItem` (or unlink when a link is wrong). This is the same link
   `/speckit-specify` offers per-feature — here it's managed portfolio-wide.

## Discipline

Read before write; ONE confirmation per write; no deletes. Skald MCP unavailable → stop
(goals live only in Skald).
