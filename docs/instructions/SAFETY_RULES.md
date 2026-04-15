# SAFETY_RULES

## 1) Purpose

These rules are mandatory guardrails for AI agents editing a repository.
Goal: ship useful changes without breaking reproducibility, data correctness, or current interfaces.

Concrete contracts, entry points, leakage notes, and service-specific safeguards belong in `docs/PROJECT_CONTEXT.md`.

## 2) Non-Negotiable Contract Rules

1. Preserve public contracts:
- Do not break the public interfaces or lifecycle contracts named in `docs/PROJECT_CONTEXT.md` unless explicitly requested.

2. Preserve existing entry points:
- Keep the project's documented entry points executable.
- Do not rename existing scripts, package paths, public modules, or CLI flags unless explicitly requested.

3. Preserve deterministic behavior:
- Use config-controlled seeds and split strategies where applicable.

4. Fail loudly on invalid input:
- Raise explicit `ValueError` / `FileNotFoundError` with clear, testable messages.

5. Standardize validation at boundaries:
- Follow the project's documented validation preference for external inputs and keep validation logic close to the boundary.
- Keep trusted internal runtime objects lightweight unless the project context says otherwise.

## 3) Data Leakage And Validation Rules

1. Avoid leakage in features:
- Keep identifiers and target-derived columns out of model features unless intentionally needed and documented.
- Follow any domain-specific leakage notes documented in `docs/PROJECT_CONTEXT.md`.

2. Validate required schema early:
- Ensure target and feature columns exist before split/train.
- Check configured source file existence with explicit errors.

3. Keep split behavior appropriate to task:
- Use stratified split only when labels are valid for classification.
- Group/time-sensitive data should avoid random row-level leakage patterns.

## 4) Change Scope Rules

1. Keep changes incremental:
- Prefer smallest coherent change over broad refactors.

2. Keep abstractions practical:
- Avoid adding framework layers or generic wrappers without clear reuse.

3. Respect current stack bias:
- Follow the stack and abstraction bias documented in `docs/PROJECT_CONTEXT.md`.

## 5) Testing And Verification Rules

1. Any behavior change requires tests:
- Update or add tests that cover the changed behavior.

2. Test style expectations:
- Follow the project's documented test style expectations from `docs/PROJECT_CONTEXT.md`.
- Keep tests deterministic and assert the most important observable behavior.

3. Validation command before completion:
```bash
<test-command-from-project-context>
```

If tests cannot be run, state that explicitly in handoff notes.

## 6) Documentation Safety Rules

1. Keep docs command-accurate:
- Commands must reflect real repo entry points and current CLI options.
- Use the project's canonical workflow from `docs/PROJECT_CONTEXT.md`.

2. When introducing new dataset flow:
- Document raw file location, metadata expectations, and split derivation rules.

## 7) Secret And Service Hygiene

1. Never hardcode secrets:
- No subscription secrets/tokens in committed files.

2. Keep external-service config explicit:
- Require the project's documented service configuration before submit or deploy actions.
- Surface missing config via clear validation errors.

3. Keep run traceability artifacts:
- Preserve any traceability artifacts promised by the project context.

## 8) Safe Delivery Checklist

Before finalizing a feature, verify:
- interfaces unchanged unless requested,
- deterministic behavior preserved,
- tests added/updated and run,
- docs updated for new user-facing behavior,
- outputs remain human-readable CSV/JSON/text.
