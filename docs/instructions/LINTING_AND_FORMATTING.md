# LINTING_AND_FORMATTING

## Purpose

Standardize linting and formatting workflow while keeping project-specific tool choices and commands in `docs/PROJECT_CONTEXT.md`.

## Required Workflow

When you modify files that are covered by repo-defined linters or formatters, run the canonical lint and format commands from `docs/PROJECT_CONTEXT.md` before handoff.

Prefer targeted runs on touched files over whole-repo formatting during routine changes.

## What This Covers

- repo-defined lint fixes
- formatting
- import ordering or equivalent style normalization
- review of auto-fixes before finalizing

## Scope Rule

- Apply this instruction whenever source files are added or modified in a language with an established lint or format workflow.
- If the repo defines language-specific commands in `docs/PROJECT_CONTEXT.md`, use them exactly.
- If auto-fixes change structure or behavior, review the result before finalizing.

## Verification

Linting and formatting are part of the local delivery checklist in addition to the repo's test command.

Typical sequence:

```bash
<lint-command> <touched-files>
<format-command> <touched-files>
<test-command>
```

## Notes

- Do not rely only on editor save hooks for agent-made edits.
- If no files covered by repo lint or format tools changed, this instruction does not add extra commands.
