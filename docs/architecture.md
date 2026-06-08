# Architecture — UAG-compiler

## Boundary
Transforms and validates architecture graphs. Imports UAG-core. Emits UAGL and projections.

## Modules
- `pipeline`: stage orchestration.
- `loader`: input files/packages.
- `parser`: YAML/JSON.
- `resolver`: IDs, refs, dialects.
- `normalizer`: canonical form.
- `validator`: semantic rules.
- `lowerer`: TAKG to UAGL.
- `exporters`: projections.
- `diagnostics`: errors and loss reports.
- `diff`: graph diffs.
- `query`: inspection.
- `package`: `.uagpkg` and future `.uagdb`.
