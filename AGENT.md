# AGENT.md — Codex Context for `UAG-compiler`

## Repository Identity
Repository: `UAG-compiler`  
Organization: `UAG-Labs`  
GitHub description: Rust compiler and CLI for transforming TAKG source graphs into UAGL compiled architecture IR and generated outputs.

## Non-Negotiable Rule
The graph is the source of truth. The diagram is a view. The export is a projection.

## Role
Rust compiler, validator, exporter, diagnostics, package, diff, query, and CLI repository.

## Technology
Rust workspace. No React UI.

## Dependency Boundary
Depends on UAG-core. UAG-studio calls it.

## Expected Output
Compiler library and CLI transforming TAKG to UAGL and exports.

## Working Instructions
1. Read `README.md`, `docs/architecture.md`, `docs/artifact.md`, `docs/REPOSITORY_STRUCTURE.md`, and `docs/specs/README.md` before implementation.
2. Add or update specs using `docs/procedures/add-specification-file.md`.
3. Do not implement undocumented behavior.
4. Do not create unresolved implementation questions. Record blockers in `docs/open-questions.md` and stop.
5. Preserve TAKG as editable source graph and UAGL as compiled IR.
6. Keep generated output deterministic wherever possible.
7. Keep repo responsibilities inside this repo's boundary.
