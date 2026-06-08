# Plan: PLAN-104 - Exporters and Loss Reports

**Status:** Draft
**Repo Scope:** `UAG-compiler`

## Goal
Implement production exporters with honest loss reporting.

## Tasks
1. Implement exporter registry.
2. Implement Mermaid, D2, Markdown summary, and AI context exporters.
3. Add target capability declarations for each exporter.
4. Emit loss reports for unsupported objects, degraded relationships, layout loss, protocol loss, security-policy loss, runtime loss, contract loss, and provenance loss.
5. Apply classification/redaction policy before AI context output.
6. Add exporter snapshot tests.

## Success Criteria
1. Each exporter consumes UAGL, not TAKG.
2. Each exporter emits artifact plus loss report.
3. AI context export blocks or redacts according to policy.
4. Exporter output is deterministic for unchanged input.
