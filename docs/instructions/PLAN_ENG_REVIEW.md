# PLAN_ENG_REVIEW

## Purpose

Use this instruction after direction is approved and before implementation when a repository needs an engineering-readiness review.

This workflow is adapted from `gstack`'s `plan-eng-review` workflow by Garry Tan and contributors. The purpose is to lock in architecture, data flow, failure modes, validation, and tests before code changes begin.

## When To Use

Use this instruction when:
- a design doc or plan has been approved,
- the next step is implementation of a non-trivial change,
- the user asks for architecture review, technical review, or execution review,
- the work touches shared modules, workflow stages, manifests, CLI surface, or data contracts.

Typical triggers:
- "review the engineering plan"
- "is this buildable?"
- "what are the risks before implementation?"
- "lock in the architecture"

## Required Inputs

Before running this review, read:
- `docs/PROJECT_CONTEXT.md`
- `docs/instructions/PROJECT_SPEC.md`
- `docs/instructions/SAFETY_RULES.md`
- `docs/instructions/LINTING_AND_FORMATTING.md`
- the approved design doc, office-hours note, or implementation plan
- the code areas likely to change

## Review Goal

Decide whether the approved direction is ready to implement in this repository without avoidable rework, regressions, or contract drift.

The review should answer:
- where the change belongs,
- what contracts change or stay stable,
- what can fail,
- how success and safety will be tested.

## Required Workflow

1. Restate the approved change in implementation terms.
2. Identify the exact surfaces affected:
   - entry points or executable surfaces
   - shared package or library modules
   - data contracts or configuration schemas
   - tests
   - docs and operator runbooks
3. Decide the smallest coherent placement for the work.
4. Review the architecture and data flow:
   - input boundaries
   - metadata flow
   - validation points
   - output artifacts
   - split and seed behavior
5. Review contract stability:
   - public commands, script names, or CLI flags
   - lifecycle or interface contracts named in `docs/PROJECT_CONTEXT.md`
   - data types, manifests, or config schemas
6. Review failure modes and safety checks:
   - missing files
   - invalid metadata
   - leakage-prone features or invalid inputs
   - wrong split or environment strategy
   - service or infrastructure configuration gaps
7. Review test impact:
   - success path
   - failure path
   - deterministic behavior
   - docs updates for user-facing changes
8. Produce an implementation recommendation:
   - ready to implement
   - ready with concerns
   - blocked pending clarification

## Project-Fit Guardrails

- Keep changes incremental.
- Preserve public entry points, package paths, and external interfaces unless explicitly requested.
- Keep the contracts named in `docs/PROJECT_CONTEXT.md` stable by default.
- Prefer the project's stated validation and data-flow strategy over ad hoc branching.
- Preserve deterministic behavior and reproducible outputs.
- Keep outputs human-readable and filesystem-inspectable when that is part of the project contract.

## Expected Output

Produce a short engineering-readiness memo with:
- change summary
- affected files or modules
- architecture and data-flow notes
- contract risks
- test plan
- open questions
- go / no-go recommendation

When the review identifies unresolved issues, those issues should be closed in the plan before implementation starts.

## Done Criteria

This review is complete when:
- code placement is justified,
- interfaces and invariants are explicit,
- key failure modes are named,
- the required tests and docs updates are known,
- the implementation can proceed without major ambiguity.
