# IMPLEMENTATION_PLAN_LIFECYCLE

## Purpose

Define the standard lifecycle and terminology for implementation plans used by implementation agents.

## Lifecycle

`Draft -> Draft Reviewed -> Implemented -> Review -> Suggest Revise -> Completed`

`Suggest Revise` is only used when review finds changes are needed. If no revision is required, the plan moves from `Review` directly to `Completed`.

## Status Definitions

- `Draft`: Initial version of the plan. Not yet reviewed.
- `Draft Reviewed`: The plan has been checked for clarity, scope, and feasibility, and is cleared to move into implementation.
- `Implemented`: Code has been written according to the plan.
- `Review`: The implementation is under code review or validation.
- `Suggest Revise`: The reviewer recommends changes or adjustments; if used, the plan may loop back to `Draft`.
- `Completed`: The implementation is approved and finished.

## Guidelines

- Keep plans small and testable.
- Avoid large, multi-purpose plans.
- Map each plan to one clear outcome.
- Keep steps deterministic and verifiable.
- Prefer concrete files, commands, and checks over vague action items.
- Treat `Draft Reviewed` as the required pre-implementation gate; do not advance a plan to `Implemented` until the draft has been reviewed and cleared.

## Optional Concepts

- `Blocked`: Use when progress is waiting on an external dependency; resume at the current status when unblocked.
- `Abandoned`: Use when the plan is no longer relevant and should not be continued.
