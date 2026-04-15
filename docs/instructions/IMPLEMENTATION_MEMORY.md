# IMPLEMENTATION_MEMORY

## Purpose

Preserve working memory in repo-local documentation so plans, decisions, discussion history, and implementation considerations survive across turns, branches, and agents.

Project-specific storage locations and naming conventions belong in `docs/PROJECT_CONTEXT.md`.

## When To Use

Use this instruction when:
- the user asks to keep work as memory, notes, or implementation plans,
- a task is non-trivial and spans multiple steps or sessions,
- a docs-only change materially alters project positioning, workflow guidance, operator handoff, architecture expectations, or command-accurate usage,
- an important design or tradeoff discussion should remain discoverable,
- a change builds on earlier commits, plans, or review docs,
- a new instruction, workflow, or project convention is being introduced.

## Required Inputs

Before recording memory, read:
- `docs/PROJECT_CONTEXT.md`
- the relevant user request
- the current plan, design doc, or implementation-plan file if one exists
- any commit hashes, PR references, or prior documents that the new note should connect to

## Required Workflow

1. Decide whether to update an existing implementation-plan file or create a new one.
2. Use the canonical memory location and naming style from `docs/PROJECT_CONTEXT.md`.
3. Record the minimum durable context needed for a future agent or engineer to continue the work without guesswork.
4. Include references to relevant commits, plans, or docs when the current work depends on earlier history.
5. Capture decisions and tradeoffs, not just outcomes.
6. End with a concrete status and next action, even if the next action is "no further work planned."

## What To Record

Include the parts that matter for the task at hand:
- problem statement
- constraints
- relevant prior context or commit references
- approaches considered
- recommendation or decision
- scope
- work completed
- open questions
- next action
- status

## Recording Rules

- Prefer factual summaries over long transcripts.
- Record why a decision was made when that reasoning is likely to matter later.
- Treat the canonical implementation-memory location from `docs/PROJECT_CONTEXT.md` as the repo's durable working-memory layer, not just a folder for pre-implementation plans.
- Treat implementation memory as append-only by default.
- Append dated follow-on updates for new information instead of rewriting prior plan entries, job results, task results, or review history.
- Only revise an active plan when the user explicitly asks for a revision.
- Do not duplicate README content or user-facing docs unless needed for context.
- Treat documentation-only changes that materially affect project positioning, operator workflow, architecture guidance, or user-facing commands as non-trivial and record them.
- Formatting-only edits, typo fixes, and narrow copy cleanup do not require a memory update.
- Prefer updating the closest existing implementation-plan file over creating many small files.
- Create a new implementation-plan file only when the work is materially new and does not fit an existing thread.
- Do not create a new memory file for every trivial edit.
- If the idea or scope shifts, append a dated follow-on update that explains the new direction, why it changed, and the current next action.
- If the work changes direction, append rather than overwrite so the earlier record remains intact.

## Done Criteria

This instruction is complete when the repository contains enough durable context for a future agent or engineer to understand what was planned, what was decided, what changed, and what remains.
