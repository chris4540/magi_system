# AGENTS.md

This repository uses the documents in `docs/` as the local skill set for agents.
Read the relevant docs before making changes, and keep your work aligned with them.

## Primary Skill Docs

- `docs/instructions/PROJECT_SPEC.md`
  - Use for project intent, scope fit, users, and affected workflows.
- `docs/instructions/SAFETY_RULES.md`
  - Use for non-negotiable safety, contract, validation, and change-scope rules.
- `docs/instructions/BRANCHING_AND_COMMITS.md`
  - Use for branch naming, commit style, and diff baseline guidance.
- `docs/instructions/LINTING_AND_FORMATTING.md`
  - Use for linting, formatting, and verification workflow after code changes.
- `docs/instructions/IMPLEMENTATION_PLAN_LIFECYCLE.md`
  - Use for plan status, review gates, and implementation-plan terminology.
- `docs/instructions/IMPLEMENTATION_MEMORY.md`
  - Use when recording durable work notes, decisions, and follow-up context.

## Supporting Skill Docs

- `docs/instructions/PYTHON_DOCSTRINGS.md`
  - Use for Python docstring style and documentation expectations.
- `docs/instructions/PROJECT_INIT.md`
  - Use as the architecture and bootstrap reference for the MAGI-style system.
- `docs/instructions/PLAN_ENG_REVIEW.md`
  - Use for engineering review of plans and implementation detail.
- `docs/instructions/PLAN_CEO_REVIEW.md`
  - Use for product or strategy review of plans.
- `docs/instructions/CLICKUP_SWE_TICKET.md`
  - Use when converting work into a ClickUp-style engineering ticket.
- `docs/instructions/YC_OFFICE_HOURS.md`
  - Use when the task is a design or advisory exercise that needs structured feedback.

## Working Rules

1. Prefer the smallest coherent change.
2. Preserve existing interfaces and entry points unless the user asks otherwise.
3. Add or update tests when behavior changes.
4. Keep docs command-accurate and aligned with the current repo workflow.
5. If a task touches multiple areas, consult the most specific applicable doc first, then the supporting docs.

## Suggested Order Of Use

1. Read `PROJECT_SPEC` and `SAFETY_RULES`.
2. Read the task-specific doc, such as `LINTING_AND_FORMATTING` or `IMPLEMENTATION_MEMORY`.
3. Apply the guidance directly in the affected files.
4. Verify the change with the repo’s relevant checks.
