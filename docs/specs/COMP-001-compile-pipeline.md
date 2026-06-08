# Spec: COMP-001 — Compile Pipeline

**Spec ID:** COMP-001
**Type:** Component
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose — Specifies ordered compiler stages.
1.2 Context — A deterministic pipeline makes validation and exports reliable.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) — none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define stage order.
   2.1.B Define input/output per stage.
   2.1.C Define failure behavior.
2.2 Non-Goals (out of scope)
   2.2.A Does not define exporter syntax.
   2.2.B Does not define Studio UI.

## 3. Requirements
3.1 Functional requirements
   3.1.A Pipeline must run load, parse, resolve, normalize, validate, lower, emit.
   3.1.B Each stage returns diagnostics.
   3.1.C Fatal errors stop later stages.
3.2 Non-functional requirements
   3.2.A Stage output must be deterministic.
   3.2.B Pipeline must be fixture-testable.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Input is TAKG path or graph.
   4.1.B Output is compile result with UAGL/diagnostics/artifacts.

## 5. Behavior
5.1 Happy path
   5.1.A Pipeline receives source.
   5.1.B Stages execute.
   5.1.C Result returns.
5.2 Edge cases
   5.2.A Warnings do not stop unless strict.
   5.2.B Fatal parse stops resolution.
5.3 Error states
   5.3.A Invalid YAML stops parse.
   5.3.B Duplicate IDs fail validation.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] Stages exist (verifies §3.1.A) — Verified by: [—]
   6.1.B [ ] Diagnostics flow through stages (verifies §3.1.B) — Verified by: [—]
   6.1.C [ ] Fatal errors stop downstream (verifies §3.1.C) — Verified by: [—]

## 7. Open Questions & Assumptions
7.1 Open questions — No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Stage-based design is required. — Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
