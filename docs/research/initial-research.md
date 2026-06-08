# Research Summary — `UAG-compiler`

## Research Basis
The initial design incorporates lessons from graph-native architecture modeling, compiler IR design, low-level systems modeling, high-level enterprise architecture, API/event description formats, and visual graph editors.

## Lessons Applied
- Separate editable source graph from compiled IR.
- Do not flatten high-level and low-level architecture.
- Keep views and exports as projections.
- Add diagnostics and loss reports to prevent false completeness.
- Use Rust for compiler/core/system-level reliability.
- Use React for Studio frontend and graph canvas.
- Keep unknowns and assumptions explicit.
- Keep repo boundaries explicit.

## Trade-Offs Resolved
- Four-repo organization instead of one giant repo.
- TAKG + UAGL instead of one overloaded file.
- Rust core/compiler instead of JavaScript-only implementation.
- React + Tauri Studio instead of browser-only editor for first serious implementation.
