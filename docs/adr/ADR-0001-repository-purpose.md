# ADR-0001 — Repository Purpose and Boundary

**Status:** Accepted  
**Date:** 2026-06-08

## Context
UAG-Labs is a four-repo organization. Each repo needs a strict boundary so implementation does not collapse into an unmaintainable monolith.

## Decision
`UAG-compiler` has this role:

Rust compiler, validator, exporter, diagnostics, package, diff, query, and CLI repository.

## Consequences
- This repo must not absorb sibling responsibilities.
- Specs define expected implementation behavior.
- New behavior requires specs before code.
- Cross-repo dependency direction must remain explicit.
