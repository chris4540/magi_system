---
name: remelight-file-memory
description: build or operate a simple file-based memory system for coding agents inspired by remelight from agentscope-ai/reme. use when chatgpt needs to design, scaffold, explain, or implement agent memory as editable files instead of a database or vector store, especially for codex-style agents. this skill is for a minimal first version that keeps long-term memory in markdown files, stores raw dialog and tool outputs on disk, uses plain text or filename search before embeddings, and favors readable, copyable, debuggable memory workflows.
---

# ReMeLight file memory

Implement a **minimal, file-first memory system** inspired by ReMeLight.

Design principle: **memory as files, files as memory**.

Use this skill to help a coding agent:
- persist useful context across runs,
- compact long conversations,
- keep tool outputs out of the prompt when they get too large,
- retrieve prior memory using simple text search,
- avoid vector databases in the first version.

This skill is intentionally **simple-first**:
- prefer markdown and jsonl files,
- prefer deterministic file operations,
- prefer grep/string matching over embeddings,
- prefer inspectable memory over opaque storage.

## Required constraints

Follow these constraints unless the user explicitly overrides them:

1. **Follow ReMeLight concepts**
   - Keep the core split between long-term memory, daily memory, raw dialog, and tool-result spillover.
   - Keep compaction and persistence as first-class behaviors.

2. **Treat memory as files**
   - Store memory in normal project files.
   - Make files readable, editable, copyable, and versionable.
   - Avoid hiding memory in databases for v1.

3. **Omit embeddings first**
   - Do not introduce vector storage, embedding models, or semantic retrieval unless the user explicitly asks.
   - Start with filename matching, section matching, and plain text search.

4. **Target codex-style agents**
   - Optimize for coding assistants that work over repositories, plans, errors, tool outputs, and incremental tasks.
   - Prefer memory structures that preserve file paths, commands, decisions, errors, and next steps.

## Default directory layout

Use this layout unless the repo already has an established equivalent:

```text
working_dir/
├── MEMORY.md                # durable long-term memory
├── memory/
│   └── YYYY-MM-DD.md        # daily distilled notes
├── dialog/
│   └── YYYY-MM-DD.jsonl     # raw message history
├── tool_result/
│   └── <uuid>.txt           # spilled large tool outputs
└── state/
    └── index.json           # optional lightweight metadata cache
```

## Meaning of each file

### `MEMORY.md`
Use for stable memory that should survive across sessions.

Good examples:
- user coding preferences,
- repository conventions,
- architecture decisions,
- recurring constraints,
- canonical commands,
- important project paths,
- known failure patterns,
- preferred output formats.

Do **not** dump everything here. Keep it curated.

Recommended sections:

```markdown
# Memory

## User Preferences
## Project Facts
## Architecture Decisions
## Coding Conventions
## Known Commands
## Known Pitfalls
## Active Goals
```

### `memory/YYYY-MM-DD.md`
Use for day-level distilled memory.

This file should capture:
- what happened,
- what changed,
- what mattered,
- what may matter again.

Recommended sections:

```markdown
# 2026-04-15

## Summary
## Important Decisions
## Files Touched
## Errors and Fixes
## Follow-ups
## Candidate Promotions to MEMORY.md
```

### `dialog/YYYY-MM-DD.jsonl`
Store the raw turn-by-turn interaction log.

Each line should be a compact JSON object such as:

```json
{"ts":"2026-04-15T10:22:31Z","role":"user","content":"Investigate failing tests"}
{"ts":"2026-04-15T10:22:52Z","role":"assistant","content":"I found two failures in parser.spec.ts"}
```

Keep it append-only when possible.

### `tool_result/<uuid>.txt`
Use this for large tool outputs that would otherwise bloat context.

Examples:
- long test logs,
- compiler output,
- huge diffs,
- lint output,
- stack traces,
- search results,
- generated reports.

When spilling a tool result:
- save the full output to a file,
- keep only a short summary plus file reference in active context,
- include enough detail for later reopening.

Example reference text:

```text
Full pytest log stored at tool_result/3f2c7f.txt
Relevant excerpt: lines 120-166
Summary: 3 failures in auth middleware tests due to missing mock session.
```

## Core behaviors

Implement or emulate these behaviors.

### 1. Append raw interaction history
After each meaningful turn or tool action:
- append a compact record to `dialog/YYYY-MM-DD.jsonl`,
- include timestamp, role, and concise content,
- optionally include metadata like task id, command, exit code, or touched files.

### 2. Compact context when it gets too large
When recent history becomes too large for the model window:
- keep the most recent, high-signal messages in active context,
- summarize older messages into a structured checkpoint,
- write durable insights to `memory/YYYY-MM-DD.md`,
- promote truly stable facts to `MEMORY.md`.

Preferred checkpoint structure:

```markdown
## Goal
## Constraints
## Progress
## Key Decisions
## Next Steps
## Critical Context
```

For coding agents, `Critical Context` should favor:
- file paths,
- function names,
- commands,
- failing tests,
- error messages,
- branch or migration details,
- assumptions still in force.

### 3. Spill long tool outputs to files
If a tool result is too large to keep inline:
- store it under `tool_result/`,
- replace the inline payload with a concise summary,
- keep a path reference and optionally a line range.

Use aggressive spilling for older outputs and lighter spilling for the newest output.

### 4. Persist important memory
Regularly distill interaction history into durable memory.

Promote content by stability:
- **ephemeral** → keep only in dialog,
- **session-relevant** → write to daily memory,
- **cross-session stable** → write to `MEMORY.md`.

Promotion rules:
- prefer facts over chatter,
- prefer durable preferences over one-off requests,
- prefer decisions over speculation,
- avoid duplicating the same fact in many places.

### 5. Retrieve memory without embeddings
For v1, retrieval must avoid vectors.

Use this ranking order:

1. exact filename/path match,
2. exact heading match,
3. exact phrase match,
4. token overlap / substring match,
5. recency as a tie-breaker.

Search targets in priority order:
1. `MEMORY.md`,
2. newest files in `memory/`,
3. optionally recent `dialog/` files,
4. referenced `tool_result/` files only when needed.

For each retrieval, return:
- source file path,
- matched heading or line context if available,
- short explanation of relevance,
- excerpt or summary.

## Retrieval workflow

When asked to recall relevant memory, follow this workflow:

1. Search `MEMORY.md` for direct matches.
2. Search recent daily memory files for supporting context.
3. Search recent dialog logs only if curated memory is insufficient.
4. Open large tool-result files only if the answer likely depends on them.
5. Synthesize a concise answer.
6. If new durable facts are learned during retrieval, suggest or perform a memory update.

Do not pretend retrieval is semantic if it is only lexical.
Be explicit when matches are weak.

## Write/update workflow

When updating memory files:

1. Read the target file first.
2. Merge with existing content instead of blindly appending duplicates.
3. Preserve useful human-written structure.
4. Keep edits minimal and auditable.
5. Prefer section-based updates.
6. If confidence is low, add a `TODO` or `Needs confirmation` note rather than inventing facts.

## What to store for coding agents

Prioritize memory that improves future coding performance.

High-value memory examples:
- preferred languages, frameworks, and package managers,
- linting or formatting rules,
- repository build/test commands,
- important environment quirks,
- architectural boundaries,
- common failure modes and fixes,
- naming conventions,
- deployment caveats,
- active refactor plans,
- unfinished work and next steps.

Low-value memory examples:
- filler chat,
- repeated acknowledgements,
- transient logs with no future value,
- speculative guesses that were not validated.

## Suggested lightweight APIs

When designing code for this system, prefer a tiny surface area such as:

```python
append_dialog(message)
compact_context(messages) -> summary
persist_memory(messages_or_summary)
search_memory(query) -> results
spill_tool_result(content) -> file_ref
load_relevant_memory(query) -> prompt_block
```

Keep implementations composable and file-oriented.

## Output expectations when helping the user implement this

When asked to build or modify a simple ReMeLight-style system:
- scaffold the directory structure,
- define the file schemas clearly,
- implement retrieval with plain file search first,
- add compaction and spill-to-file behaviors,
- keep dependencies minimal,
- avoid vector DB setup unless explicitly requested.

When asked for code, prefer:
- Python,
- small modules,
- straightforward filesystem operations,
- explicit comments around promotion and retrieval logic.

## Guardrails

Do not:
- add embeddings by default,
- add a database by default,
- over-engineer ranking,
- store everything forever without curation,
- overwrite curated memory with noisy summaries,
- present weak lexical matches as strong memory recall.

## Upgrade path for later versions

Only introduce these when the user asks:
- embedding-based retrieval,
- hybrid lexical + semantic ranking,
- file watchers,
- background summarization,
- TTL cleanup for tool-result files,
- metadata indexes,
- workspace-specific memory partitions,
- multi-agent shared memory.

## Preferred reasoning stance

When using this skill, optimize for:
- simplicity,
- inspectability,
- easy migration,
- low dependency count,
- codex-agent usefulness,
- deterministic behavior.

When choosing between elegance and debuggability, choose **debuggability**.
