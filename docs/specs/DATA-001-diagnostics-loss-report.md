# Spec: DATA-001 - Diagnostics and Loss Report Model

**Spec ID:** DATA-001
**Type:** Data
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose - Defines diagnostic and loss report data shapes.
1.2 Context - Diagnostics and loss reports keep compiler/exporter behavior honest.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) - none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define diagnostic fields.
   2.1.B Define loss report fields.
   2.1.C Define severity, source locations, affected-object refs, machine categories, and remediation.
2.2 Non-Goals (out of scope)
   2.2.A Does not define Studio UI layout.
   2.2.B Does not replace validation rules.

## 3. Requirements
3.1 Functional requirements
   3.1.A Diagnostics include ID, severity, code, category, message, affected object, source location, related objects, remediation, and stage.
   3.1.B Loss reports include target, target capability, preserved objects, omitted objects, degraded semantics, reason, category, fidelity score, and remediation.
   3.1.C Reports serialize.
   3.1.D Loss categories include unsupported-object, degraded-relationship, layout-loss, protocol-loss, security-policy-loss, runtime-loss, contract-loss, and provenance-loss.
   3.1.E Source-map diagnostics must be emitted when a diagnostic cannot point back to TAKG or a compiled object.
3.2 Non-functional requirements
   3.2.A Report output must be deterministic.
   3.2.B Reports must be machine-readable.
   3.2.C Reports must be compact enough for CLI output and complete enough for Studio inspection.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Diagnostic fields include `id`, `severity`, `code`, `category`, `message`, `stage`, `affected`, `source_location`, `related`, `remediation`, and `strict_behavior`.
   4.1.B Loss report fields include `target`, `target_capability`, `preserved`, `omitted`, `degraded`, `categories`, `fidelity_score`, and `remediation`.
   4.1.C Affected refs may target packages, objects, fields, relationships, source spans, export artifacts, or runtime observations.

## 5. Behavior
5.1 Happy path
   5.1.A Issue detected.
   5.1.B Diagnostic added.
   5.1.C CLI/Studio displays it.
5.2 Edge cases
   5.2.A Package-level diagnostics allowed.
   5.2.B Lossless exports may have empty omissions.
   5.2.C Multiple exporters may produce different loss reports from the same UAGL.
5.3 Error states
   5.3.A Unknown affected object becomes package diagnostic.
   5.3.B Invalid severity rejected.
   5.3.C Loss report without target capability fails strict validation.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] Diagnostics serialize (verifies 3.1.A) - Verified by: [--]
   6.1.B [ ] Loss reports serialize (verifies 3.1.B) - Verified by: [--]
   6.1.C [ ] Loss reports reference target (verifies 3.1.C) - Verified by: [--]
   6.1.D [ ] Loss categories cover export omissions and degradation (verifies 3.1.D) - Verified by: [--]
   6.1.E [ ] Diagnostics include source-map locations where available (verifies 3.1.E) - Verified by: [--]

## 7. Open Questions & Assumptions
7.1 Open questions - No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Loss reports mandatory for lossy exporters. - Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
