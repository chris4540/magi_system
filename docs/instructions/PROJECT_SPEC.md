# PROJECT_SPEC

## 1) Product Intent

This instruction explains how to gather and restate project or product intent without embedding repository-specific facts in this reusable file.

Project-specific goals, users, architecture boundaries, commands, and constraints belong in `docs/PROJECT_CONTEXT.md`.

## 2) When To Use

Use this instruction when you need to:
- understand what the repository is for,
- decide whether a request fits the current project scope,
- summarize the current strategy before planning or implementation,
- challenge a request that appears misaligned with the project context.

## 3) Required Inputs

Before using this instruction, read:
- `docs/PROJECT_CONTEXT.md`
- the current user request
- the relevant design doc or implementation plan, if one exists
- any nearby README or docs pages that describe the affected workflow

## 4) Required Workflow

1. Read the project identity and primary goal in `docs/PROJECT_CONTEXT.md`.
2. Identify the relevant users, operators, or stakeholders for the request.
3. Restate the current project strategy in plain language.
4. Identify the relevant code areas, entry points, workflows, and contracts named in `docs/PROJECT_CONTEXT.md`.
5. Name the constraints and out-of-scope items that the request must respect.
6. Decide whether the request:
   - fits the existing project direction,
   - extends it in a coherent way,
   - or conflicts with the stated purpose and boundaries.
7. If there is conflict or ambiguity, surface it explicitly before implementation.

## 5) Expected Output

Produce a short context memo or summary with:
- project goal
- relevant users or operators
- affected workflows or surfaces
- relevant constraints
- repo-fit assessment
- open questions, if any

## 6) Done Criteria

This instruction is complete when the project intent, repo fit, and key constraints have been restated clearly enough to inform planning or implementation without guessing.
