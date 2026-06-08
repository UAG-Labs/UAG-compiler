# Plan: PLAN-105 - Package Query Diff Migrate

**Status:** Draft
**Repo Scope:** `UAG-compiler`

## Goal
Finish graph operations needed for large systems and long-term maintenance.

## Tasks
1. Implement package manifest handling, imports, namespaces, dependency locks, and compatibility checks.
2. Implement query selectors over canonical UAGL.
3. Implement semantic diff over stable IDs and normalized field paths.
4. Implement migration commands for supported schema/language version changes.
5. Add machine-readable output for all operations.

## Success Criteria
1. Multi-package fixtures resolve correctly.
2. Query and diff do not depend on layout.
3. Migration commands produce diagnostics and changed output.
4. Package/import conflicts are deterministic and diagnosable.
