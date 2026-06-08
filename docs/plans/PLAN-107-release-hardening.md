# Plan: PLAN-107 - Release Hardening

**Status:** Draft
**Repo Scope:** `UAG-compiler`

## Goal
Prepare compiler and CLI for production release.

## Tasks
1. Complete fixture, snapshot, invalid-input, package, exporter, and CLI tests.
2. Add performance tests for large graphs.
3. Add docs for every command and output format.
4. Audit errors, diagnostics, and panics.
5. Verify compatibility matrix entries.
6. Prepare release artifacts.

## Success Criteria
1. `cargo test` passes.
2. CLI docs match implemented commands.
3. Canonical fixtures compile/export consistently.
4. No known panic path exists for user input.
5. Release notes include compatibility and migration guidance.
