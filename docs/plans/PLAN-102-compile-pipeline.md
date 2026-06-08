# Plan: PLAN-102 - Compile Pipeline

**Status:** Draft
**Repo Scope:** `UAG-compiler`

## Goal
Implement deterministic TAKG-to-UAGL compilation.

## Tasks
1. Implement load, parse, schema-check, resolve-imports, resolve-dialects, build-source-map, normalize, semantic-validate, lower, emit, and export stage orchestration.
2. Define fatal versus recoverable stage behavior.
3. Use `UAG-core` serialization and model types.
4. Emit compile result with status, UAGL, diagnostics, source maps, artifacts, loss reports, timings, and compatibility metadata.
5. Snapshot canonical output.

## Success Criteria
1. Valid fixtures compile to UAGL.
2. Invalid YAML stops at parse with diagnostics.
3. Repeated output is deterministic.
4. Stage timings and diagnostics are included in machine-readable output.
