# Plan: PLAN-002 - Compiler Vertical Slice

**Status:** Draft
**Derived From:** ../specs/COMP-001-compile-pipeline.md, ../specs/COMP-002-cli.md, ../specs/COMP-003-exporter-system.md, ../specs/DATA-001-diagnostics-loss-report.md
**Derivation Status:** Current

## Objective
Implement the first TAKG-to-UAGL vertical slice that proves the graph contract against canonical examples with diagnostics, source maps, deterministic output, and at least one honest exporter.

## Preconditions
1. `UAG-core` exposes model types, typed IDs, diagnostics, serialization, and schema artifacts.
2. `UAG` canonical examples are available and non-empty.
3. Compiler crate boundaries are created.

## Work Packages
| Work Package | Scope | Primary Specs | Exit Criteria |
|---|---|---|---|
| WP1 Workspace skeleton | Create `crates/uag-compiler` and `crates/uag-cli` with library/CLI boundaries. | SYS-001, COMP-002 | `cargo test` and CLI smoke command run. |
| WP2 Loader/parser | Load TAKG files and package manifests through `UAG-core` serialization. | COMP-001 | Invalid YAML produces parse diagnostics. |
| WP3 Resolver | Resolve IDs, imports, namespaces, dialects, and object refs. | COMP-001 | Unknown refs produce structured diagnostics. |
| WP4 Source maps | Track source file/path/span to compiled objects and diagnostics. | COMP-001, DATA-001 | Diagnostics include source-map locations. |
| WP5 Semantic validation | Validate relationship endpoints/fields, view filters, package collisions, security policy, and contract bindings. | COMP-001 | Canonical fixtures produce deterministic validation results. |
| WP6 Lowering/emission | Lower TAKG to canonical UAGL. | COMP-001 | Repeated compile emits stable output. |
| WP7 CLI | Implement `compile`, `validate`, `inspect`, and `schema` first; stage `export`, `diff`, `query`, `package`, and `migrate` next. | COMP-002 | JSON output matches compile-result shape. |
| WP8 First exporter | Implement Mermaid or Markdown exporter with capability declaration and loss report. | COMP-003 | Exported artifact plus loss report produced from a canonical fixture. |

## Suggested Module Layout
```text
crates/uag-compiler/src/
  pipeline/
  loader/
  resolver/
  source_map/
  validator/
  lowerer/
  exporters/
  diagnostics/
  query/
  diff/
  package/
crates/uag-cli/src/
  main.rs
```

## Test Plan
1. Compile canonical TAKG examples from `UAG`.
2. Snapshot canonical UAGL output.
3. Snapshot diagnostics and source-map output.
4. Validate fatal parse/schema behavior.
5. Validate exporter loss reports against declared target capability.
6. CLI JSON output snapshot tests.

## Dependencies
1. `UAG-core` crate API for model, serialization, diagnostics, and schemas.
2. Canonical examples from `UAG`.
3. No dependency on `UAG-studio`.

## Risks
1. Building exporters before source maps and validation stabilize would create false confidence.
2. Query/diff/package can sprawl; keep the first vertical slice focused on compile/validate/inspect/schema.
3. If `UAG-core` canonical serialization changes, compiler snapshots will need coordinated updates.

## Exit Criteria
1. `uag compile <fixture>` emits deterministic UAGL.
2. `uag validate <fixture>` emits deterministic diagnostics.
3. Source maps exist for compiled entities and relationships.
4. At least one exporter emits a loss report.
5. `UAG-studio` can plan against a stable compile-result JSON shape.
