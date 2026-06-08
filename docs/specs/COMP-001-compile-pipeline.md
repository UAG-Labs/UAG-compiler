# Spec: COMP-001 - Compile Pipeline

**Spec ID:** COMP-001
**Type:** Component
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose - Specifies ordered compiler stages.
1.2 Context - A deterministic pipeline makes validation and exports reliable.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) - none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define stage order.
   2.1.B Define input/output per stage.
   2.1.C Define failure behavior.
   2.1.D Produce UAGL with source maps, validation results, runtime attachment refs, and deterministic ordering.
2.2 Non-Goals (out of scope)
   2.2.A Does not define Studio UI.
   2.2.B Does not bypass `UAG-core` schemas or model types.

## 3. Requirements
3.1 Functional requirements
   3.1.A Pipeline must run load, parse, schema-check, resolve-imports, resolve-dialects, build-source-map, normalize, semantic-validate, lower, emit, and export.
   3.1.B Each stage returns diagnostics with source locations when source information is available.
   3.1.C Fatal errors stop later stages that require invalid data.
   3.1.D Semantic validation must check relationship endpoints, relationship contract fields, view filters, package namespace collisions, source-map completeness, literal secret detection, and contract/schema bindings.
   3.1.E Compiler must produce semantic diff/query inputs from canonical UAGL.
   3.1.F Compiler must distinguish design intent from runtime observations and report drift when observations cannot be mapped.
3.2 Non-functional requirements
   3.2.A Stage output must be deterministic.
   3.2.B Pipeline must be fixture-testable against canonical examples.
   3.2.C Compile results must be machine-readable for CLI and Studio.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Input is TAKG path, package manifest, or graph.
   4.1.B Output is compile result with UAGL, diagnostics, source maps, artifacts, and loss reports.
   4.1.C Compile result fields include `status`, `uagl`, `diagnostics`, `source_maps`, `artifacts`, `loss_reports`, `timings`, and `compatibility`.
   4.1.D Query and diff operations consume canonical UAGL, not raw TAKG.

## 5. Behavior
5.1 Happy path
   5.1.A Pipeline receives source.
   5.1.B Stages execute in order.
   5.1.C Result returns UAGL and diagnostics.
5.2 Edge cases
   5.2.A Warnings do not stop unless strict.
   5.2.B Fatal parse stops resolution.
   5.2.C Fatal schema failure may still return parse diagnostics.
   5.2.D Runtime-only observation without design ref produces drift diagnostic, not a new design entity.
5.3 Error states
   5.3.A Invalid YAML stops parse.
   5.3.B Duplicate IDs fail validation.
   5.3.C Broken source-map coverage fails compiler tests.
   5.3.D Relationship missing protocol/mode/cardinality emits semantic diagnostic.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] Stages exist (verifies 3.1.A) - Verified by: [--]
   6.1.B [ ] Diagnostics flow through stages (verifies 3.1.B) - Verified by: [--]
   6.1.C [ ] Fatal errors stop downstream (verifies 3.1.C) - Verified by: [--]
   6.1.D [ ] Canonical examples compile with source maps (verifies 3.1.D) - Verified by: [--]
   6.1.E [ ] Query/diff consume canonical UAGL (verifies 3.1.E) - Verified by: [--]

## 7. Open Questions & Assumptions
7.1 Open questions - No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Stage-based design is required. - Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
