# Spec: COMP-002 - CLI

**Spec ID:** COMP-002
**Type:** Component
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose - Specifies `uag` command line interface.
1.2 Context - CLI enables development and CI workflows without Studio.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) - none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define commands.
   2.1.B Define I/O behavior.
   2.1.C Define exit codes.
   2.1.D Define machine-readable output for CI, Studio, and automation.
2.2 Non-Goals (out of scope)
   2.2.A Does not define GUI.
   2.2.B Does not implement shell completions in MVP.

## 3. Requirements
3.1 Functional requirements
   3.1.A CLI must expose compile, validate, export, inspect, diff, query, package, schema, and migrate.
   3.1.B CLI must print diagnostics.
   3.1.C CLI must support machine output with `--format json`.
   3.1.D CLI diff must compare canonical UAGL objects by stable IDs and normalized field paths.
   3.1.E CLI query must operate on canonical UAGL selectors, not diagram layout.
   3.1.F CLI package commands must validate imports, namespace collisions, dependency locks, and compatibility metadata.
3.2 Non-functional requirements
   3.2.A Fatal errors return nonzero.
   3.2.B Output overwrite requires explicit flag.
   3.2.C JSON output must be deterministic for CI snapshots.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Inputs are args/files.
   4.1.B Outputs are files/stdout/exit codes.
   4.1.C Exit codes distinguish success, warnings in strict mode, validation error, parse error, IO error, unsupported target, and internal error.
   4.1.D Machine output uses the same compile result shape returned to Studio.

## 5. Behavior
5.1 Happy path
   5.1.A User runs command.
   5.1.B CLI calls library.
   5.1.C CLI writes output.
5.2 Edge cases
   5.2.A Missing output path uses safe default.
   5.2.B Existing output requires overwrite.
   5.2.C `--strict` turns warnings matching configured categories into failures.
5.3 Error states
   5.3.A Missing input returns nonzero.
   5.3.B Invalid target lists supported targets.
   5.3.C Package/import mismatch returns compatibility diagnostics.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] Compile command works (verifies 3.1.A) - Verified by: [--]
   6.1.B [ ] Diagnostics print (verifies 3.1.B) - Verified by: [--]
   6.1.C [ ] Fatal errors nonzero (verifies 3.2.A) - Verified by: [--]
   6.1.D [ ] JSON output matches compile result schema (verifies 3.1.C) - Verified by: [--]
   6.1.E [ ] Diff/query/package commands use canonical UAGL (verifies 3.1.D-3.1.F) - Verified by: [--]

## 7. Open Questions & Assumptions
7.1 Open questions - No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A CLI is Rust. - Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
