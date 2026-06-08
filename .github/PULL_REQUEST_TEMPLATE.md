## Summary
<!-- What does this PR change and why? -->

## Changes
- 

## Related Issues
<!-- Closes # -->

## Checklist
- [ ] No undocumented behavior introduced
- [ ] Compiler output is deterministic (same graph + same policy = identical output)
- [ ] Loss report emitted for any dropped graph semantics — no silent loss
- [ ] New adapters are Rust-only and registered via the policy engine interface
- [ ] Naming logic lives in `src/policy/` — no hardcoded identifiers in emitters
- [ ] Codegen emitters are independent — no shared emit logic across language targets
- [ ] Compilation readiness failures report the specific missing information
- [ ] Blockers recorded in `docs/open-questions.md` if applicable
