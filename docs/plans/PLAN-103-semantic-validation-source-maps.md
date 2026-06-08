# Plan: PLAN-103 - Semantic Validation and Source Maps

**Status:** Draft
**Repo Scope:** `UAG-compiler`

## Goal
Make compiler validation precise enough for CLI, exporters, and Studio diagnostics.

## Tasks
1. Validate IDs, references, dialect resolution, relationship endpoints, relationship fields, view filters, package/import namespaces, policy fields, contract bindings, and runtime observation attachment.
2. Build source maps from TAKG source path/span to UAGL objects and diagnostics.
3. Emit drift diagnostics for runtime observations that cannot map to design objects.
4. Emit source-map diagnostics when traceability is missing.
5. Add strict mode behavior by severity/category.

## Success Criteria
1. Diagnostics include source location where possible.
2. Relationship and view semantic errors are caught.
3. Source-map coverage is tested against canonical fixtures.
4. Strict mode can fail builds based on configured severity/category.
