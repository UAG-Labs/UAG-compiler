# Procedure: Add Specification File

## Purpose
Create a spec file that defines what a feature, component, API, or data model should do precisely enough to implement and verify without ambiguity.

## Trigger
Use this procedure when defining a new feature, component, API, data model, integration, or system overview.

## Research-First Requirement
Before writing or changing a spec, read `../research/initial-research.md`, relevant `../adr/ADR-*.md` files, `../artifact.md`, and `../open-questions.md`.

This initialized repository package already includes a research summary. Future work must either cite that summary or add new research notes before creating or changing a spec.

## Spec Types

| Type | Code | Use when |
|------|------|----------|
| Feature | `FEAT` | User-visible behavior end-to-end |
| Component | `COMP` | Internal service, module, or library component |
| Data | `DATA` | Schema, model, data contract, or file format |
| Integration | `INTG` | External system or API behavior |
| System-overview | `SYS` | Unified map linking many specs for a complex system |

## Naming Convention
Spec files follow:

```text
<TYPE>-<NNN>-<kebab-name>.md
```

Numbering is per type. Examples:

```text
SYS-001-compiler-system.md
COMP-001-compile-pipeline.md
DATA-001-uagl-compiled-ir.md
FEAT-001-visual-graph-editor.md
```

## Required Sections
Every spec must have seven fixed sections:

1. Overview
2. Scope
3. Requirements
4. Interface / Data
5. Behavior
6. Acceptance Criteria
7. Open Questions & Assumptions

Acceptance criteria must cite the requirements they verify. Use `Verified by: [—]` until actual evidence exists.

## Index Requirement
Every new spec must be added to `../specs/README.md`.

## No Placeholder Rule
Do not leave placeholder text. If a decision is unknown, add it to `../open-questions.md` and stop.
