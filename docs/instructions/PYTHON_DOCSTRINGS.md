# PYTHON_DOCSTRINGS

## Purpose

Keep touched public Python APIs understandable with concise Google-style
docstrings.

Project-specific expectations about which surfaces count as public belong in
`docs/PROJECT_CONTEXT.md`.

## When To Use

Use this instruction when you touch:
- public classes,
- public methods,
- protected methods,
- adapter methods,
- non-trivial public functions.

## Required Workflow

1. Add or update a concise Google-style docstring when touching a covered
   public Python surface.
2. Keep the docstring focused on behavior and operator value rather than
   repeating implementation detail.
3. Include sections such as `Args`, `Returns`, or `Raises` only when they help
   clarify the contract.
4. Do not add boilerplate docstrings to private helpers or trivial local
   functions unless they have become part of a public surface.

## Style Guardrails

- Prefer short summaries over long prose.
- Keep wording easy to maintain as code changes.
- Do not restate obvious type information unless it improves clarity.
- Preserve the repo's existing Google-style preference.

## Example

```python
def build_bundle(metadata: DatasetMetadata, frame: pd.DataFrame) -> DatasetBundle:
    """Build a validated dataset bundle from metadata and prepared rows.

    Args:
        metadata: Parsed dataset metadata used to define the bundle contract.
        frame: Prepared dataset rows that match the metadata schema.

    Returns:
        DatasetBundle ready for downstream pipeline stages.
    """
```

## Done Criteria

This instruction is complete when touched public Python APIs have concise
Google-style docstrings that clarify the contract without unnecessary detail.
