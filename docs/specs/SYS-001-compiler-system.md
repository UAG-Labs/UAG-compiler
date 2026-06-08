# Spec: SYS-001 — Compiler System

**Spec ID:** SYS-001
**Type:** System-overview
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose — Defines compiler system for TAKG to UAGL transformation, validation, diagnostics, and exports.
1.2 Context — The compiler makes graph data operational.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) — none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define pipeline.
   2.1.B Define CLI scope.
   2.1.C Define exporters.
   2.1.D Define diagnostics/loss reports.
2.2 Non-Goals (out of scope)
   2.2.A Does not define core structs.
   2.2.B Does not render Studio UI.
   2.2.C Does not own examples.

## 3. Requirements
3.1 Functional requirements
   3.1.A Compiler must load TAKG/UAGL.
   3.1.B Compiler must validate references/semantics.
   3.1.C Compiler must lower TAKG into UAGL.
   3.1.D Compiler must export from UAGL.
3.2 Non-functional requirements
   3.2.A Output must be deterministic.
   3.2.B Errors must be structured.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Interface is Rust library API plus `uag` CLI.
   4.1.B Inputs are TAKG/UAGL; outputs are UAGL/artifacts/diagnostics.

## 5. Behavior
5.1 Happy path
   5.1.A User runs compile.
   5.1.B Compiler loads TAKG.
   5.1.C Compiler emits UAGL.
5.2 Edge cases
   5.2.A Partial TAKG emits diagnostics.
   5.2.B Lossy export emits loss report.
5.3 Error states
   5.3.A Invalid path returns CLI error.
   5.3.B Unresolved reference returns diagnostic.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] Compile command exists (verifies §3.1.A) — Verified by: [—]
   6.1.B [ ] Reference validation exists (verifies §3.1.B) — Verified by: [—]
   6.1.C [ ] TAKG to UAGL output exists (verifies §3.1.C) — Verified by: [—]

## 7. Open Questions & Assumptions
7.1 Open questions — No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Compiler depends on UAG-core. — Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
