# ADR-0002 - Implementation Readiness Decisions for UAG-compiler

## Status
Accepted

## Context
The first documentation audit identified open implementation-readiness questions for UAG-compiler. The questions covered compile result shape, fatal stages, unresolved references, source maps, exporters, validation ownership, CLI output, packages, diff/query, and AI context policy. Each question received an AI recommendation after reviewing the repository documentation and comparable systems.

## Decision
Adopt the AI recommendations recorded in [open-questions.md](../open-questions.md) as the planning baseline for the first implementation plans. These decisions are not final product law; they are the current accepted defaults that implementation plans should use unless a later ADR supersedes them.

## Decisions
| Question | Topic | Decision |
|---|---|---|
| Q-001 | What is the exact compile result contract? | Use a rich compile result: optional UAGL, diagnostics, warnings, source map, loss report summary, and stage metadata. Keep export artifacts in export-specific results. |
| Q-002 | Which pipeline stages are fatal? | Adopt stage-specific policies: load/parse failures are fatal, resolve/validate can return partial analysis, and lower/emit require compile-ready graph state. |
| Q-003 | How does the compiler handle unresolved references during editing? | Do not emit valid UAGL when unresolved references remain. Return diagnostics plus analysis metadata, and let Studio keep editing the TAKG draft. |
| Q-004 | What source map is needed between TAKG, UAGL, diagnostics, and exports? | Implement source maps with both object IDs and file/line/column spans. Add generated artifact fragment IDs later when exporters mature. |
| Q-005 | Which export targets are first and how are losses classified? | Implement Mermaid, D2, Markdown summary, and AI context first; require each exporter to declare preserved semantics and loss categories in code and tests. |
| Q-006 | What does validation own versus `UAG-core`? | Let core validate shape, IDs, serialization, and primitive dialect references; compiler validates cross-object semantics, exportability, packages, and security policy. |
| Q-007 | What is the CLI output contract? | Support `--json` on every command from the start, while keeping human-readable output as the default for terminal use. |
| Q-008 | How does package format relate to raw TAKG/UAGL files? | Defer binary `.uagpkg` until raw files stabilize. Start with a directory package manifest that can later be zipped without changing semantics. |
| Q-009 | How should diff and query operate? | Make diff/query UAGL-first for deterministic semantics; defer TAKG-aware visual/layout diff until Studio layout behavior is stable. |
| Q-010 | What security policy applies to generated AI context? | Block AI context export unless sensitivity policy is explicit; milestone one can ship a conservative policy file plus redaction diagnostics. |

## Consequences
- Implementation plans can proceed from a concrete baseline instead of unresolved ambiguity.
- Future disagreement should create a new open question and, if accepted, a superseding ADR.
- Specs and plans should cite this ADR when they rely on these decisions.

## Follow-up
- Update implementation plans to reference this ADR.
- Promote decisions into detailed specs when implementation starts.
