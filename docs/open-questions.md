# Open Questions - `UAG-compiler`

## Status
All initial audit questions have been answered for planning purposes. Future compiler/export uncertainty should be added as new open questions.

## Research Basis
- Structurizr DSL separates source, model, views, extensions/includes, and generated/exported representations: https://docs.structurizr.com/dsl/language
- Structurizr distinguishes source DSL from compiled JSON with layout/editor information: https://docs.structurizr.com/workspaces/file-types
- D2 demonstrates declarative diagram generation from text and exposes layout concerns as part of export ergonomics: https://d2lang.com/tour/intro
- OpenAPI descriptions are machine-readable HTTP-like API documents: https://learn.openapis.org/specification/structure.html
- AsyncAPI defines event-driven concepts such as channels, messages, operations, servers, and protocol bindings: https://www.asyncapi.com/docs/reference/specification/v3.0.0

## Question Format
```markdown
## Q-001 - Title
Status: Open | Resolved
Raised by:
Question:
Why it matters:
Options:
Impacts:
Decision needed before:
Resolution evidence:
```

## Q-001 - What is the exact compile result contract?
Status: Resolved
Raised by: Audit of `COMP-001-compile-pipeline.md`, `SYS-001-compiler-system.md`, and Studio integration specs.
Question: What fields must every compile result contain: UAGL, diagnostics, warnings, loss report, stage timings, source map, export artifacts, or all of these?
Why it matters: Studio, CLI, CI, and package APIs need a stable result shape.
Options:
- Minimal: optional UAGL plus diagnostics.
- Rich: UAGL, diagnostics, warnings, source map, loss report, stage metadata.
- Separate results for compile, validate, export, diff, and package commands.
AI recommendation: Use a rich compile result: optional UAGL, diagnostics, warnings, source map, loss report summary, and stage metadata. Keep export artifacts in export-specific results.
Decision: Use a rich compile result: optional UAGL, diagnostics, warnings, source map, loss report summary, and stage metadata. Keep export artifacts in export-specific results.
Impacts: Rust API, CLI JSON output, Studio panels, tests.
Decision needed before: Compiler library API.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-002 - Which pipeline stages are fatal?
Status: Resolved
Raised by: Current stage list: load, parse, resolve, normalize, validate, lower, emit.
Question: Which diagnostics stop downstream stages, and which allow partial output?
Why it matters: Studio must tolerate invalid TAKG, while CLI/CI need reliable exit codes.
Options:
- Stop on first fatal stage.
- Continue as far as possible and return partial analysis.
- Stage-specific policies with fatal, blocking, warning, and loss categories.
AI recommendation: Adopt stage-specific policies: load/parse failures are fatal, resolve/validate can return partial analysis, and lower/emit require compile-ready graph state.
Decision: Adopt stage-specific policies: load/parse failures are fatal, resolve/validate can return partial analysis, and lower/emit require compile-ready graph state.
Impacts: Diagnostics, CLI behavior, Studio live validation, test matrix.
Decision needed before: Pipeline orchestration implementation.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-003 - How does the compiler handle unresolved references during editing?
Status: Resolved
Raised by: TAKG allows unresolved references; UAGL requires resolved references.
Question: Should unresolved references prevent UAGL emission, emit partial UAGL, or produce a separate analysis graph?
Why it matters: Studio needs helpful feedback while editing incomplete graphs.
Options:
- No UAGL if unresolved references exist.
- Partial UAGL with explicit unresolved placeholders.
- Analysis-only result until graph is compile-ready.
AI recommendation: Do not emit valid UAGL when unresolved references remain. Return diagnostics plus analysis metadata, and let Studio keep editing the TAKG draft.
Decision: Do not emit valid UAGL when unresolved references remain. Return diagnostics plus analysis metadata, and let Studio keep editing the TAKG draft.
Impacts: UAGL validity, Studio UX, CLI exit codes, diagnostics.
Decision needed before: Resolver and lowerer implementation.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-004 - What source map is needed between TAKG, UAGL, diagnostics, and exports?
Status: Resolved
Raised by: Studio needs clickable diagnostics and exporters need loss evidence.
Question: How are source file spans, object IDs, generated UAGL IDs, and exported artifact fragments linked?
Why it matters: Users need to trace a generated diagram/API/doc line back to graph source.
Options:
- Object-ID-only source map.
- File/line/column spans plus object IDs.
- Full provenance graph carried through compilation and export.
AI recommendation: Implement source maps with both object IDs and file/line/column spans. Add generated artifact fragment IDs later when exporters mature.
Decision: Implement source maps with both object IDs and file/line/column spans. Add generated artifact fragment IDs later when exporters mature.
Impacts: Parser, diagnostics, exporters, Studio navigation, AI context trust.
Decision needed before: Parser and diagnostics implementation.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-005 - Which export targets are first and how are losses classified?
Status: Resolved
Raised by: Exporter spec plus D2/OpenAPI/AsyncAPI research.
Question: Which targets ship in milestone one, and what preserved/lost semantics must each target declare?
Why it matters: Exporters must not pretend a static diagram or API contract preserves the whole architecture graph.
Options:
- Mermaid and Markdown first.
- Mermaid, D2, OpenAPI, AsyncAPI first.
- One diagram target plus one contract target plus AI context.
AI recommendation: Implement Mermaid, D2, Markdown summary, and AI context first; require each exporter to declare preserved semantics and loss categories in code and tests.
Decision: Implement Mermaid, D2, Markdown summary, and AI context first; require each exporter to declare preserved semantics and loss categories in code and tests.
Impacts: Exporter API, loss reports, examples, acceptance tests.
Decision needed before: Exporter implementation plan.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-006 - What does validation own versus `UAG-core`?
Status: Resolved
Raised by: Core owns validation primitives; compiler owns full validation.
Question: Which validation rules live in compiler: semantic references, dialect rules, security, exportability, package integrity, schema validation?
Why it matters: Rule placement affects reuse by Studio and API stability.
Options:
- Core validates shape; compiler validates semantics.
- Compiler owns all validation except schema generation.
- Core provides a rule trait and compiler registers rules.
AI recommendation: Let core validate shape, IDs, serialization, and primitive dialect references; compiler validates cross-object semantics, exportability, packages, and security policy.
Decision: Let core validate shape, IDs, serialization, and primitive dialect references; compiler validates cross-object semantics, exportability, packages, and security policy.
Impacts: Core dependency boundary, compiler modules, Studio live validation.
Decision needed before: Validator module implementation.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-007 - What is the CLI output contract?
Status: Resolved
Raised by: CLI spec lists compile, validate, export, inspect, diff, package.
Question: Should every command support machine-readable JSON output in addition to human-readable terminal output?
Why it matters: CI, Studio, scripts, and future MCP tools will likely need stable machine output.
Options:
- Human output only initially.
- `--json` for all commands.
- JSON by default for subcommands intended for integration.
AI recommendation: Support `--json` on every command from the start, while keeping human-readable output as the default for terminal use.
Decision: Support `--json` on every command from the start, while keeping human-readable output as the default for terminal use.
Impacts: CLI design, tests, documentation, Studio integration.
Decision needed before: CLI implementation.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-008 - How does package format relate to raw TAKG/UAGL files?
Status: Resolved
Raised by: Architecture mentions `.uagpkg` and future `.uagdb`.
Question: Is `.uagpkg` a zip-like bundle, a directory convention, a database, or deferred until raw files stabilize?
Why it matters: Studio open/save, examples, compiler package command, and CI fixtures may depend on package behavior.
Options:
- Defer package format until TAKG/UAGL stabilize.
- Directory package with manifest.
- Zip package with manifest, source, compiled outputs, exports, and source maps.
AI recommendation: Defer binary `.uagpkg` until raw files stabilize. Start with a directory package manifest that can later be zipped without changing semantics.
Decision: Defer binary `.uagpkg` until raw files stabilize. Start with a directory package manifest that can later be zipped without changing semantics.
Impacts: CLI package command, Studio project IO, versioning, examples.
Decision needed before: Implementing package command or Studio project format.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-009 - How should diff and query operate?
Status: Resolved
Raised by: Compiler owns diff/query support but no specs exist yet.
Question: Do diff and query operate on TAKG, UAGL, packages, or all of them?
Why it matters: Diff/query are central to architecture governance and AI/MCP workflows.
Options:
- UAGL-only for deterministic semantic diff/query.
- TAKG for editor-aware diff plus UAGL for semantic diff.
- Defer until compile/export path is stable.
AI recommendation: Make diff/query UAGL-first for deterministic semantics; defer TAKG-aware visual/layout diff until Studio layout behavior is stable.
Decision: Make diff/query UAGL-first for deterministic semantics; defer TAKG-aware visual/layout diff until Studio layout behavior is stable.
Impacts: CLI, package format, source maps, MCP future tools.
Decision needed before: Adding diff/query implementation plans.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Q-010 - What security policy applies to generated AI context?
Status: Resolved
Raised by: Export goals include AI context YAML/JSON and TAKG bans literal secrets.
Question: What redaction, classification, or trust-boundary metadata must the compiler enforce before exporting AI-readable context?
Why it matters: Architecture context can expose sensitive topology, internal endpoints, data classifications, and operational constraints.
Options:
- Block AI context export until sensitivity metadata exists.
- Export with heuristic redaction and warnings.
- Require explicit export policy file.
AI recommendation: Block AI context export unless sensitivity policy is explicit; milestone one can ship a conservative policy file plus redaction diagnostics.
Decision: Block AI context export unless sensitivity policy is explicit; milestone one can ship a conservative policy file plus redaction diagnostics.
Impacts: Exporters, diagnostics, Studio UX, security docs.
Decision needed before: AI context exporter implementation.
Resolution evidence: [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md)

## Resolved Initialization Decisions
- R-001: Repos are `UAG`, `UAG-core`, `UAG-compiler`, and `UAG-studio`.
- R-002: Rust is used for system-level implementation.
- R-003: React + TypeScript are used for Studio frontend.
- R-004: TAKG is editable source; UAGL is compiled IR.
- R-005: All specs follow fixed `TYPE-NNN-name.md` naming and seven-section format.
