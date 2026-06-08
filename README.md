<p align="center">
  <img src="./assets/banner/uag-labs-readme-banner.svg" alt="UAG-compiler banner" />
</p>

# UAG-compiler

**The UAG compiler validates, transforms, and compiles architecture graphs into real implementation targets.**

The compiler is the engine of the UAG system. It takes a TAKG source graph, runs the policy engine, lowers to UAGL, and emits clean, complete output across all targets. The output is always authoritative — one source repo, generated in full, no conflict markers, no parallel code.

```text
TAKG source graph
→ policy engine (naming, code decisions, adapter bindings)
→ validate + resolve
→ lower to UAGL
→ codegen/ module emits:
    Rust · TypeScript · React · C
    diagrams (Mermaid, D2) · docs (Markdown)
    API contracts (OpenAPI, AsyncAPI)
    deployment configs (Dockerfile, Helm, Terraform)
    AI context bundles
```

## End State

A UAG graph describing a service compiles to a runnable, idiomatic codebase in the target language. The developer does not edit generated code — all intent flows through the graph. Recompiling replaces the output cleanly.

## Policy Engine

The policy engine is the compiler's decision layer. It answers the "what to code" question that the graph alone cannot answer.

**What the policy engine defines:**
- Naming conventions — all identifiers derive from graph node names + policy rules, never AI-generated
- Adapter bindings — which Rust adapter satisfies each graph primitive on each target platform
- Code decision semantics — how constraints compile to enforcement, how goals influence generated patterns
- Error/retry/concurrency defaults — what gets generated when the graph doesn't specify
- Compilation target profiles — which platforms are active for a given compilation run

## Layered Behavior Model

Business logic stays in the graph through five layers:

```text
1. STRUCTURE   nodes + edges           what exists, what connects
2. STATE       state machines          control flow per capability, typed transitions
3. COMPOSITION predicates + effects    logic from composable primitives
4. POLICY      adapter bindings        implementation decisions, naming, conventions
5. HOLES       typed contracts         irreducible logic — contract tracked in graph, impl in adapter
```

The compiler processes all five layers. Layer 5 (Holes) is the honest acknowledgment that some business logic cannot be expressed as graph structure — but the hole's contract, inputs, outputs, and invariants are all graph nodes. The implementation lives in a registered adapter, not in hand-edited generated code.

## Adapter System

Every compilation target is backed by a Rust adapter. The adapter defines how a graph primitive compiles on a specific platform.

```text
PostgresAdapter    → satisfies "persistent resource" via SQLx
AxumAdapter        → satisfies "HTTP capability" via route emit
KafkaAdapter       → satisfies "event stream" via rdkafka
ReactAdapter       → satisfies "UI capability" via component emit
TerraformAdapter   → satisfies "cloud resource" via HCL emit
```

Rust is the single compilation substrate. All targets compile through Rust or through Rust-driven codegen. When a platform gap exists, a new adapter is built and registered — the graph ecosystem gains that target without any graph changes.

## Module Structure

```text
uag-compiler/
  src/
    pipeline/      stage orchestration
    loader/        TAKG file and package loading
    parser/        YAML/JSON deserialization
    resolver/      ID resolution, ref linking, dialect expansion
    normalizer/    canonical form, deduplication
    validator/     semantic validation (not just syntax)
    lowerer/       TAKG → UAGL lowering passes
    policy/        policy engine — naming, adapters, code decisions
    codegen/       per-language emitters
      rust/
      typescript/
      react/
      c/
    exporters/     projection emitters (diagrams, docs, contracts, AI context)
    adapters/      platform adapter registry and resolution
    diagnostics/   errors, warnings, loss reports
    diff/          graph diff
    query/         graph inspection
    ci/            CI/CD config emitters
```

## Failure Modes the Compiler Must Prevent

- **F-1 Abstract collapse** — emitting only generic scaffolding with no meaningful structure
- **F-2 Graph over-specification** — the graph encoding implementation details the policy engine should own
- **F-3 Constraint theater** — constraints that appear in the graph but compile to comments, not enforcement
- **F-4 Silent loss** — dropping graph semantics without reporting them
- **F-5 Non-idiomatic output** — generated Rust/TypeScript that no developer would recognize as normal code
- **F-6 Compilation readiness confusion** — failing without explaining exactly what the graph is missing

## Success Criteria

- A TAKG graph describing one service (3 capabilities, 2 events, 1 resource, 1 constraint) compiles to a `cargo build`-passing Rust project
- Generated code is idiomatic — a senior engineer would not identify it as generated without being told
- Security boundary constraints produce type-system enforcement, not comments
- Every emitter produces a loss report; silent loss is a hard compile error
- The compiler distinguishes "architecturally valid" from "compilable for target X"
- Changing one graph node regenerates only affected output files in under one second on a 50-node graph
- All identifiers derive deterministically from graph node names via policy rules — no AI-generated names

## CLI

```bash
uag compile system.takg.yaml --target rust --policy default.policy.yaml
uag validate system.takg.yaml
uag emit system.uagl.yaml --target typescript --out ./src
uag emit system.uagl.yaml --target ci --platform github-actions
uag diff v1.uagl.yaml v2.uagl.yaml
uag query system.uagl.yaml --holes     # list all unresolved holes
uag query system.uagl.yaml --gaps      # list all compilation readiness gaps
```
