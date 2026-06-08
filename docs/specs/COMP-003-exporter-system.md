# Spec: COMP-003 - Exporter System

**Spec ID:** COMP-003
**Type:** Component
**Status:** Draft
**Date:** 2026-06-08
**Author:** Agent

## 1. Overview
1.1 Purpose - Specifies UAGL target exporters.
1.2 Context - UAGL needs projections into diagrams, docs, API specs, deployment stubs, and AI context.
1.3 Related artifacts
   1.3.A ADR: [ADR-0001 Repository Purpose](../adr/ADR-0001-repository-purpose.md)
   1.3.B Research: [Research Summary](../research/initial-research.md)
   1.3.C Open questions: [Open Questions](../open-questions.md) - none unresolved in this initialized package.
   1.3.D Plan: [Bootstrap Plan](../plans/PLAN-001-bootstrap.md)

## 2. Scope
2.1 Goals
   2.1.A Define exporter registry.
   2.1.B Define initial exporters.
   2.1.C Define loss reports.
   2.1.D Define target capability declarations.
2.2 Non-Goals (out of scope)
   2.2.A Does not make target formats source of truth.
   2.2.B Does not bypass UAGL.

## 3. Requirements
3.1 Functional requirements
   3.1.A Exporters must consume UAGL.
   3.1.B Exporters must declare preserved/lost semantics before export.
   3.1.C Lossy exporters must emit loss reports.
   3.1.D Initial exporters are Mermaid, D2, Markdown summary, and AI context.
   3.1.E OpenAPI and AsyncAPI exporters must wait until operations/contracts/messages/bindings are schema-backed.
   3.1.F Deployment exporters must preserve desired-vs-observed separation and must not invent runtime status.
   3.1.G AI context exporters must apply classification and redaction policy before writing output.
3.2 Non-functional requirements
   3.2.A Output must be deterministic.
   3.2.B Unchanged input yields stable output.
   3.2.C Exporters must be fixture-tested against at least one rich canonical example.

## 4. Interface / Data
4.1 Type-specific detail
   4.1.A Input is UAGL/options.
   4.1.B Output is artifact and loss report.
   4.1.C Exporter capability fields include `target`, `supported_objects`, `supported_relationship_fields`, `supported_view_kinds`, `security_policy_support`, `layout_support`, `contract_support`, and `runtime_support`.
   4.1.D Loss reports compare UAGL semantics against the target capability declaration.

## 5. Behavior
5.1 Happy path
   5.1.A User exports target.
   5.1.B Exporter queries UAGL view.
   5.1.C Exporter writes artifact and loss report.
5.2 Edge cases
   5.2.A Unsupported objects are omitted with loss report.
   5.2.B Empty view exports empty artifact or diagnostic.
   5.2.C Security policy may redact fields and record security-policy-loss.
5.3 Error states
   5.3.A Unsupported target errors.
   5.3.B File write failure reports path.
   5.3.C Export without capability declaration fails strict mode.

## 6. Acceptance Criteria
6.1 Criteria
   6.1.A [ ] Mermaid exporter exists (verifies 3.1.A) - Verified by: [--]
   6.1.B [ ] Loss reports emitted (verifies 3.1.B) - Verified by: [--]
   6.1.C [ ] Exports consume UAGL (verifies 3.1.C) - Verified by: [--]
   6.1.D [ ] Exporters declare capability and loss categories (verifies 3.1.B and 4.1.C) - Verified by: [--]
   6.1.E [ ] AI context exporter applies policy redaction (verifies 3.1.G) - Verified by: [--]

## 7. Open Questions & Assumptions
7.1 Open questions - No unresolved open questions are allowed in this initialized documentation package. Future uncertainty must be recorded in [Open Questions](../open-questions.md) before implementation continues.
7.2 Assumptions
   7.2.A Mermaid and Markdown are first exporters. - Validated: ../research/initial-research.md and ../adr/ADR-0001-repository-purpose.md.
