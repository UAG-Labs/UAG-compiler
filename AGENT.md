# AGENT.md — Context for `UAG-compiler`

## Repository Identity
Repository: `UAG-compiler`
Organization: `UAG-Labs`
Role: Rust compiler, policy engine, codegen, adapter system, CLI.

## Non-Negotiable Rules
- The architecture graph is the source of truth.
- The diagram is a view.
- The export is a projection.
- The code is a compilation target.
- The compiler emits one clean output — no conflict markers, no parallel code, no deprecated regions.
- All identifiers are derived from graph node names via policy rules. No AI-generated names.
- Constraints that cannot be enforced in code are compile errors, not comments.
- Silent loss is a hard failure. Every semantic that cannot be expressed must be reported.
- Business logic that cannot be expressed in the graph is a typed Hole — the compiler routes it to an adapter via the policy engine.
- Rust is the single compilation substrate. All targets compile through Rust or Rust-driven codegen.

## Technology
Rust workspace. No React UI. Depends on UAG-core.

## Dependency Boundary
Depends on UAG-core for types and primitives. UAG-studio calls it. Does not depend on UAG-studio.

## Expected Output
- Compiler library and CLI
- Policy engine with naming convention enforcement
- Codegen module (codegen/) with per-language emitters: Rust, TypeScript, React, C
- Adapter registry and platform adapters
- Projection emitters: Mermaid, D2, Markdown, OpenAPI, AsyncAPI, AI context, CI/CD configs
- Diagnostics and loss reporting pipeline
- Incremental, event-driven recompilation support

## Working Instructions
1. Read `README.md`, `docs/architecture.md`, `docs/artifact.md`, `docs/REPOSITORY_STRUCTURE.md`, and `docs/specs/README.md` before any work.
2. Add or update specs using `docs/procedures/add-specification-file.md`.
3. Do not implement undocumented behavior.
4. Do not create unresolved questions. Record blockers in `docs/open-questions.md` and stop.
5. Policy engine logic lives in `src/policy/`. Naming, adapter bindings, and code decisions are policy concerns, not compiler hardcoding.
6. Codegen emitters live in `src/codegen/<language>/`. Each emitter is independent.
7. Adapters live in `src/adapters/`. Each adapter satisfies one or more graph primitive types for one platform.
8. Keep generated output deterministic. Same graph + same policy = identical output on every run.
9. Keep repo responsibilities inside this repo's boundary.
