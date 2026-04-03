# AI Read First

Use this file before exploring `boundary-protocol`.

## Read First

1. `README.md`
2. `CORE/`
3. `CONTRACTS/`

## Read Next Only If Needed

- `RESPONSIBILITY/`
- `VALIDATION/`
- `RATIONALE/`
- specific schemas under `SCHEMAS/`

## Do Not Read By Default

- the whole repository tree
- all examples
- all rationale notes
- downstream implementation repositories

## Use This Repo When

- the task changes protocol semantics
- the task changes accountability meaning
- the task changes shared artifact vocabulary
- the task changes validation semantics used across the stack

## Typical Validation

- protocol/schema consistency checks only if the task explicitly changes them

## Hand-off Rule

If the task is about execution, ingestion, or product UI, leave this repo and
work in the relevant implementation repo instead.
