# Plan: PLAN-001 - Bootstrap `UAG-compiler`

**Status:** Draft
**Derived From:** ../specs/README.md
**Derivation Status:** Current

## Objective
Create the initial implementation skeleton exactly as defined in `../REPOSITORY_STRUCTURE.md`.

## Required Reading
- ../../README.md
- ../../AGENT.md
- ../artifact.md
- ../architecture.md
- ../REPOSITORY_STRUCTURE.md
- ../specs/README.md

## Architecture Graph Hardening
Before exporter or Studio integration work is treated as ready, the compiler must prove the graph contract:

1. Implement the load, parse, schema-check, resolve-imports, resolve-dialects, build-source-map, normalize, semantic-validate, lower, emit, and export stages.
2. Compile the canonical examples from `UAG` and verify non-empty relationships, views, source maps, and diagnostics.
3. Emit deterministic compile results with UAGL, diagnostics, source maps, artifacts, loss reports, timings, and compatibility metadata.
4. Validate relationship contract fields, view filters, package/import namespaces, literal secret detection, contract/schema bindings, and source-map coverage.
5. Require every exporter to declare target capabilities and produce a loss report.
6. Make CLI JSON output share the same compile result shape used by Studio.

## Steps
1. Create the root files and folders defined in `../REPOSITORY_STRUCTURE.md`.
2. Implement only the first milestone in `ROADMAP.md`.
3. Add tests corresponding to acceptance criteria.
4. Do not mark criteria verified until evidence exists.
5. Stop if an unresolved design question appears.
