# BRANCHING_AND_COMMITS

## Purpose

Standardize branch naming, commit messages, and diff baselines without hardcoding project-specific history into this reusable instruction file.

Project-specific branch types, default branch names, and preferred diff commands belong in `docs/PROJECT_CONTEXT.md`.

## Daily Branching And Commit Style

### Branch name style (guidelines)

- Format: `<type>/<short-kebab-case-scope>`
- Never work directly on the repository default branch. If your current branch is the default branch, create and switch to a new branch before making changes.
- If `docs/PROJECT_CONTEXT.md` defines allowed branch types, use those. Otherwise prefer:
  - `feat/` = new capability
  - `fix/` = bug fix
  - `test/` = tests or test tooling
  - `docs/` = documentation only
  - `chore/` = maintenance, repo hygiene, or config
  - `refactor/` = internal restructuring without behavior change
- Keep it short and specific (2 to 5 words), no punctuation, no ticket IDs unless already part of team flow.
- Prefer one clear scope noun, such as `api-client`, `dataset-adapter`, `lint-workflow`, or `agent-guidelines`.

### Commit message style (guidelines)

- Structure:
  1. Subject line: `<type>: <imperative summary>`
  2. Blank line
  3. Bullet list of key changes
- Subject line rules:
  - Use imperative voice (`add`, `document`, `ignore`, `configure`)
  - Keep it concise and focused on what changed
  - Match the branch scope (same topic)
- Bullet rules:
  - 3 to 6 bullets is preferred
  - Start each bullet with a verb
  - Call out behavior changes, new files, config changes, and user-facing commands
  - Mention safety/quality intent when relevant (for example leakage prevention or deterministic splits)

Template:
```text
<type>: <short summary aligned with branch scope>

- <change 1>
- <change 2>
- <change 3>
```

### Quick mapping rule (branch <-> commit)

- Branch scope should align with commit subject scope.
- If branch is `chore/agent-guidelines`, use a subject like:
  - `chore: add AGENTS.md ...`
- If branch is `feat/dataset-adapter`, use a subject like `feat: add dataset adapter ...`

## Diff Baseline Rule

- Always compare changes against the repository's default baseline branch.
- If `docs/PROJECT_CONTEXT.md` names the baseline branch, use it.
- Otherwise determine the remote default branch before comparing changes.
- Refresh refs before comparison with `git fetch origin`.

## Expected Output

- A branch name that matches the change scope
- A commit subject in imperative voice
- A short commit body that explains the key changes and any safety or validation intent
