# Plan: PLAN-106 - Integration Contracts

**Status:** Draft
**Repo Scope:** `UAG-compiler`

## Goal
Stabilize compiler integration for Studio, CI, automation, and agents.

## Tasks
1. Document library API and CLI JSON contracts.
2. Define compile-result schema and compatibility expectations.
3. Add command contracts for Studio: open/validate/compile/export/query diagnostics.
4. Add examples for automation and CI usage.
5. Preserve backward compatibility for supported command output versions.

## Success Criteria
1. Studio can call compiler without parsing human text.
2. CI can fail on strict diagnostics.
3. Automation can consume JSON output for compile, validate, export, diff, query, and migrate.
4. Integration contracts are versioned.
