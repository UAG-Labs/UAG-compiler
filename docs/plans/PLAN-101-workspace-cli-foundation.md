# Plan: PLAN-101 - Workspace and CLI Foundation

**Status:** Draft
**Repo Scope:** `UAG-compiler`

## Goal
Create the compiler workspace, crates, command shell, and test harness.

## Tasks
1. Create `crates/uag-compiler` library crate.
2. Create `crates/uag-cli` binary crate.
3. Add command shell for compile, validate, export, inspect, query, diff, package, schema, and migrate.
4. Add fixture-loading test utilities.
5. Add JSON output envelope shared by CLI and future Studio integration.

## Success Criteria
1. `cargo test` runs for compiler and CLI crates.
2. CLI help lists all planned commands.
3. Commands can return structured success/error envelopes even before full implementation.
4. No Studio UI or core model internals are implemented here.
