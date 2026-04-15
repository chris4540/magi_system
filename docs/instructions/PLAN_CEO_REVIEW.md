# PLAN_CEO_REVIEW

## Purpose

Use this instruction after `YC_OFFICE_HOURS.md` and before implementation when a repository needs a product, project, or scope-level review.

This workflow is adapted from `gstack`'s `plan-ceo-review` workflow by Garry Tan and contributors. The goal is to challenge scope, clarify the wedge, and make sure the requested work is the right work for the current repository.

## When To Use

Use this instruction when:
- the user asks to think bigger or smaller about a plan,
- the user wants a product or project perspective,
- the requested feature may be too narrow, too broad, or aimed at the wrong user outcome,
- the repository has multiple plausible ways to frame the work before code begins.

Typical triggers:
- "review this from product design or project perspective"
- "is this the right feature?"
- "should we expand or cut scope?"
- "what is the real wedge here?"

## Required Inputs

Before running this review, read:
- `docs/PROJECT_CONTEXT.md`
- `docs/instructions/PROJECT_SPEC.md`
- `docs/instructions/SAFETY_RULES.md`
- `docs/instructions/YC_OFFICE_HOURS.md`
- the active design doc or implementation plan, if one exists
- the relevant user request and any recent product notes in `docs/`

## Review Goal

Decide whether the current plan is:
- the right wedge,
- the right scope size,
- aimed at the right user or operator,
- worth building in this repository now.

This review should sharpen the problem statement before architecture or implementation details dominate the discussion.

## Review Modes

### 1) Scope Expansion

Use when the current request is too small to deliver the outcome the user actually wants.

Question:
- what larger but still coherent version would create meaningfully more value?

### 2) Selective Expansion

Use when the current request is viable, but there are adjacent upgrades worth surfacing one by one.

Question:
- what additions are strong candidates, and which should stay out of scope?

### 3) Hold Scope

Use when the plan is directionally right and the main need is sharper articulation, not expansion or reduction.

Question:
- how do we make the current scope defensible without adding churn?

### 4) Scope Reduction

Use when the plan is too large, too risky, or too abstract for the current repository and timeline.

Question:
- what is the smallest version that still proves the intended value?

## Required Workflow

1. Restate the current plan in plain language.
2. Identify the user, operator, or stakeholder who benefits most.
3. State 3 to 5 premises that the plan depends on.
4. Challenge those premises directly.
5. Generate at least 2 approaches and ideally 3:
   - minimum viable
   - recommended
   - optional more ambitious version
6. For each approach, cover:
   - summary
   - expected user value
   - implementation effort
   - repository fit
   - key risks
   - what stays out of scope
7. Make one explicit recommendation.
8. End with a concrete next action:
   - revise the design doc,
   - proceed to `PLAN_ENG_REVIEW.md`,
   - or stop and gather more product evidence.

## Project-Fit Guardrails

- Preserve the current project's stated strategy, users, and boundaries from `docs/PROJECT_CONTEXT.md`.
- Prefer changes that improve the actual outcome named in the project context instead of adding adjacent complexity.
- Be skeptical of work that adds heavy infrastructure, abstraction, or user-interface surface unless the project context explicitly supports it.
- For internal tooling or operator-facing repos, optimize for clarity and reproducibility over storytelling.
- Do not treat "more ambitious" as automatically better. Recommend expansion only when it improves the actual outcome.

## Expected Output

Produce a short review memo with:
- problem statement
- premises
- approaches considered
- recommendation
- open questions
- next action

If the user approves the direction, hand off to `docs/instructions/PLAN_ENG_REVIEW.md` before implementation on non-trivial changes.

## Done Criteria

This review is complete when:
- the wedge and scope are explicit,
- at least two plausible approaches were compared,
- one recommendation was made,
- the next action is unambiguous.
