# CLICKUP_SWE_TICKET

## Purpose

Use this instruction to turn rough engineering input into plain markdown software engineering tickets that can be pasted directly into ClickUp.

This instruction is adapted from the user-provided `clickup-swe-ticket` skill bundle and rewritten into this repository's single-file instruction convention.

## When To Use

Use this instruction when:
- the user has a rough bug report, feature request, improvement note, spike idea, or tech debt description,
- engineering notes, discussions, or plans need to be normalized into a scannable ticket,
- the goal is to make scope, acceptance criteria, and open questions explicit without inventing details.

Typical triggers:
- "turn this into a ClickUp ticket"
- "write a ticket from these notes"
- "clean up this engineering ticket"
- "split this discussion into tickets"

## Required Inputs

Before drafting the ticket, read:
- `docs/PROJECT_CONTEXT.md`
- the user's notes, problem statement, engineering discussion, or plan
- any linked design doc, implementation plan, or bug context that should constrain the ticket

## Default Workflow

1. Classify the input as one of: `Bug`, `Feature`, `Improvement`, `Spike`, or `Tech Debt`.
2. Extract only actionable engineering details supported by the input.
3. Fill the matching ticket structure.
4. Keep unresolved details visible as `TBD` or `Open question` instead of guessing.
5. Prefer crisp, testable wording over long prose.
6. If the user asks for multiple tickets, create one ticket per discrete work item.
7. Split combined notes into separate tickets when they imply different owners, releases, or acceptance criteria.

## Output Rules

- Output plain markdown unless the user asks for commentary.
- Make the result directly copy-pasteable into a ClickUp task description.
- Start with a one-line title using `[Type] concise summary`.
- Use short sections with clear headings.
- Use checklists for requirements, scope, and acceptance criteria.
- Never invent metrics, dates, owners, root causes, or implementation details.
- If critical information is missing, keep the section and mark the missing item as `TBD`.
- If the input is ambiguous between two ticket types, choose the more execution-oriented type and note the ambiguity briefly in `Context`.

## Ticket Type Decision Rules

Choose the type using these defaults:

- `Bug`: something is broken, regressed, incorrect, or inconsistent.
- `Feature`: a new user-visible or system capability is needed.
- `Improvement`: existing behavior should be enhanced without being net-new or purely maintenance.
- `Spike`: investigation, prototype, or decision-support work is needed before implementation.
- `Tech Debt`: refactor, cleanup, migration, reliability hardening, or internal maintenance work with limited direct user visibility.

If the input contains both discovery and implementation, either:
- create two tickets, one `[Spike]` and one implementation ticket, or
- create one implementation ticket and move unanswered design questions into `Open questions`.

## Title Rules

Write short, scannable titles such as:
- `[Bug] login fails for sso users on safari`
- `[Feature] add csv export to usage dashboard`
- `[Improvement] reduce dashboard load time for large workspaces`
- `[Spike] evaluate options for background job retries`
- `[Tech Debt] refactor billing webhook retry logic`

Avoid vague titles such as `fix issue`, `improve system`, or `work on api`.

## Required Sections For All Tickets

Include these sections in this order unless the user asks for a smaller format:

### Summary

One short paragraph describing the problem or goal.

### Context

Capture why this work matters now, what triggered it, and any known constraints or dependencies.

### Scope

Use checkboxes for in-scope work.

### Out Of Scope

List explicit non-goals when helpful. If unknown, write `- TBD`.

### Acceptance Criteria

Use checkboxes. Every bullet should be testable.

### Open Questions

List unknowns, risks, or decisions needed. If none are known, write `- None at this time`.

## Type-Specific Additions

### Bug

Also include:
- `Impact`
- `Environment`
- `Steps to reproduce`
- `Expected behavior`
- `Actual behavior`

Prefer concrete reproduction steps when available. If they are unavailable, keep `Steps to reproduce` with `1. TBD` rather than fabricating steps.

### Feature

Also include:
- `User / stakeholder`
- `Problem to solve`
- `Requirements`
- `Dependencies`

### Improvement

Also include:
- `Current behavior`
- `Desired improvement`
- `Success signal`

### Spike

Also include:
- `Question to answer`
- `Approach`
- `Deliverable`
- `Timebox`

Spike acceptance criteria should focus on learning outputs such as a recommendation, prototype, comparison, or documented findings.

### Tech Debt

Also include:
- `Current pain`
- `Proposed cleanup`
- `Risk / migration notes`

## Quality Bar

Before finalizing, check that the ticket:
- has exactly one clear outcome,
- separates facts from assumptions,
- includes testable acceptance criteria,
- does not hide uncertainty,
- is brief enough for an engineer to scan quickly.

If the input is extremely rough, first normalize it into concise facts, then produce the ticket.

## Output Templates

Use the matching template below.

### Bug

```markdown
[Bug] <concise summary>

## Summary
<short description>

## Impact
<who or what is affected>

## Environment
- Environment: <prod/staging/local/TBD>
- Surface: <web/api/mobile/worker/TBD>
- Version / build: <if known, else TBD>

## Steps to reproduce
1. <step or TBD>
2. <step>
3. <step>

## Expected behavior
<expected result>

## Actual behavior
<actual result>

## Context
<context, links, trigger, constraints>

## Scope
- [ ] <in-scope item>
- [ ] <in-scope item>

## Out of scope
- <non-goal or TBD>

## Acceptance criteria
- [ ] <testable outcome>
- [ ] <testable outcome>
- [ ] <monitoring, regression, or validation item if relevant>

## Open questions
- <unknown, dependency, or None at this time>
```

### Feature

```markdown
[Feature] <concise summary>

## Summary
<short description>

## User / stakeholder
<primary user, team, or system>

## Problem to solve
<what outcome is needed>

## Context
<context, links, trigger, constraints>

## Requirements
- [ ] <requirement>
- [ ] <requirement>
- [ ] <requirement>

## Scope
- [ ] <in-scope item>
- [ ] <in-scope item>

## Out of scope
- <non-goal or TBD>

## Dependencies
- <dependency or None known>

## Acceptance criteria
- [ ] <testable outcome>
- [ ] <testable outcome>
- [ ] <testable outcome>

## Open questions
- <unknown, dependency, or None at this time>
```

### Improvement

```markdown
[Improvement] <concise summary>

## Summary
<short description>

## Current behavior
<what happens today>

## Desired improvement
<what should be better>

## Context
<context, links, trigger, constraints>

## Scope
- [ ] <in-scope item>
- [ ] <in-scope item>

## Out of scope
- <non-goal or TBD>

## Success signal
- <observable improvement or TBD>

## Acceptance criteria
- [ ] <testable outcome>
- [ ] <testable outcome>

## Open questions
- <unknown, dependency, or None at this time>
```

### Spike

```markdown
[Spike] <concise summary>

## Summary
<short description>

## Question to answer
<decision or uncertainty>

## Context
<context, links, trigger, constraints>

## Approach
- [ ] <investigation step>
- [ ] <investigation step>
- [ ] <prototype, comparison, or validation step>

## Deliverable
- <document, recommendation, prototype, or decision memo>

## Timebox
- <timebox or TBD>

## Out of scope
- <non-goal or TBD>

## Acceptance criteria
- [ ] <clear learning output>
- [ ] <clear recommendation or decision record>
- [ ] <risks / tradeoffs documented>

## Open questions
- <unknown, dependency, or None at this time>
```

### Tech Debt

```markdown
[Tech Debt] <concise summary>

## Summary
<short description>

## Current pain
<what is costly, fragile, or hard to maintain>

## Proposed cleanup
<what should be changed>

## Context
<context, links, trigger, constraints>

## Scope
- [ ] <in-scope item>
- [ ] <in-scope item>

## Out of scope
- <non-goal or TBD>

## Risk / migration notes
- <risk, rollout note, or TBD>

## Acceptance criteria
- [ ] <testable engineering outcome>
- [ ] <testable engineering outcome>
- [ ] <validation / rollback / monitoring item if relevant>

## Open questions
- <unknown, dependency, or None at this time>
```

## Notes-To-Ticket Behavior

When converting notes, discussions, or plans:
- remove filler language and preserve only decisions, requirements, constraints, and unknowns,
- split unrelated ideas into separate tickets,
- move speculative ideas into `Open questions`,
- convert implied success conditions into explicit acceptance criteria when justified by the input,
- keep engineering terminology from the source when it improves precision.

## Examples

Example 1: rough bug statement
- Input: `Users say the dashboard sometimes hangs after changing date filters. Seems worse on large workspaces. We think it might be related to the summary query timing out. Need a ticket.`
- Use `Bug` if the current behavior is a failure or regression.
- State uncertainty in `Context` or `Open questions`.
- Do not claim the query timeout is the root cause unless confirmed.

Example 2: feature notes
- Input: `Need CSV export on the usage dashboard for customer success. Should respect active filters and probably handle large exports asynchronously.`
- Use `Feature`.
- Convert `should respect active filters` into a requirement and acceptance criterion.
- Leave async behavior as a requirement only if clearly requested; otherwise move it to `Open questions`.

Example 3: engineering discussion
- Input: `We need to clean up retry logic in billing webhooks. Failures are hard to trace, retries are duplicated in multiple places, and on-call has no clear signal when jobs are exhausted.`
- Use `Tech Debt`.
- Turn the maintenance pain into `Current pain`.
- Convert observability needs into acceptance criteria if supported by the notes.

Example 4: discovery work
- Input: `Before we implement background job retries, compare Redis-backed retries vs queue-native retries and recommend one. A short prototype is okay.`
- Use `Spike`.
- Set `Deliverable` to a recommendation or prototype.
- Keep implementation out of scope unless explicitly requested.

## Done Criteria

This instruction is complete when the output is a plain markdown ticket that:
- is directly pasteable into ClickUp,
- has one clear outcome,
- uses the right ticket type,
- keeps unknowns explicit,
- and contains testable acceptance criteria.
