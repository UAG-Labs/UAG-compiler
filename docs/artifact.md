# Artifact Definition — `UAG-compiler`

## Purpose
Defines what a successful first implementation artifact looks like.

## Success Conditions
- The repository can be understood without prior conversation context.
- Root README, docs architecture, roadmap, structure, and specs are present.
- No unresolved open questions exist in this initialization package.
- Implementation can begin from specs without architecture clarification.
- Acceptance criteria remain unchecked until tests or manual evidence exist.

## Done for Bootstrap
- README explains the repo.
- `specs/README.md` indexes all specs.
- `REPOSITORY_STRUCTURE.md` defines expected file/folder layout.
- `plans/PLAN-001-bootstrap.md` defines first execution plan.

## Implementation Readiness Decisions
The initial open-question audit has been answered for planning purposes. The accepted baseline decisions are recorded in [ADR-0002 Implementation Readiness Decisions](./adr/ADR-0002-implementation-readiness-decisions.md) and reflected in [open-questions.md](./open-questions.md).

Answered questions:
- Q-001: What is the exact compile result contract?
- Q-002: Which pipeline stages are fatal?
- Q-003: How does the compiler handle unresolved references during editing?
- Q-004: What source map is needed between TAKG, UAGL, diagnostics, and exports?
- Q-005: Which export targets are first and how are losses classified?
- Q-006: What does validation own versus `UAG-core`?
- Q-007: What is the CLI output contract?
- Q-008: How does package format relate to raw TAKG/UAGL files?
- Q-009: How should diff and query operate?
- Q-010: What security policy applies to generated AI context?

