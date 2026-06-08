# Architecture — UAG-compiler

## Identity
The UAG compiler is a graph-native architecture compiler. It does not scaffold or template. It compiles. The architecture graph is the source of truth; the compiler's job is to transform that graph into every downstream artifact with fidelity, determinism, and honesty about what it cannot express.

## Boundary
Imports UAG-core for all type definitions. Emits UAGL, code, projections, and CI/CD configs. Does not own graph type definitions or Studio UI.

## Pipeline Stages

```text
TAKG input
  → loader         read .takg.yaml files and packages
  → parser         deserialize to UAG-core types
  → resolver       resolve node IDs, edge refs, dialect expansions
  → normalizer     canonical form, deduplication, inheritance flattening
  → validator      semantic validation — architecture correctness, not just syntax
  → policy engine  naming resolution, adapter binding, code decision evaluation
  → lowerer        TAKG → UAGL lowering passes (behavior layer processing)
  → UAGL output
  → codegen/       per-language code emitters (Rust, TypeScript, React, C)
  → exporters/     projection emitters (diagrams, docs, contracts, AI context, CI/CD)
  → diagnostics    errors, warnings, loss reports per emitter
```

## Policy Engine (`src/policy/`)

The policy engine answers the "what to code" question. The graph describes intent; the policy engine decides implementation.

**Responsibilities:**
- **Naming** — all identifiers derive from graph node names + convention rules defined in policy. No AI-generated names. The policy file specifies: case convention per language (snake_case for Rust, camelCase for TypeScript), prefix/suffix rules per node type, module path derivation.
- **Adapter binding** — for each graph primitive type and platform target, the policy resolves which adapter satisfies it. Unresolvable bindings are compilation errors.
- **Code decision semantics** — how constraints compile to enforcement, how goals influence pattern selection, what error/retry/concurrency defaults apply when not specified in the graph.
- **Compilation target profiles** — which targets are active, which adapters are available, what the output structure looks like.

The policy file is a first-class input to the compiler:
```bash
uag compile system.takg.yaml --policy production.policy.yaml --target rust
```

## Behavior Layer Processing (`src/lowerer/`)

The lowerer processes each behavior layer from the graph:

**Layer 2 — State Machines:** Each capability's state machine is lowered to a typed transition table in UAGL. The lowerer checks exhaustiveness — every state must have defined transitions, every terminal state must be reachable.

**Layer 3 — Composition:** Predicate trees are lowered to typed boolean expressions. Effect nodes are lowered to typed resource operation descriptors. Transformation edges are lowered to shape-typed conversion pipelines.

**Layer 4 — Policy:** The policy engine runs here, binding every graph primitive to its adapter and applying naming conventions.

**Layer 5 — Holes:** Hole nodes are lowered to typed interface contracts in UAGL. The policy engine routes each hole to its registered adapter. An unrouted hole at compile time is a compilation readiness error, not a warning.

## Codegen Module (`src/codegen/`)

The codegen module houses per-language emitters. Each emitter reads UAGL and emits idiomatic code for one target language.

```text
src/codegen/
  rust/        Rust emitter — structs, traits, impl blocks, async/await patterns
  typescript/  TypeScript emitter — interfaces, classes, type aliases
  react/       React emitter — component skeletons, typed props, hooks
  c/           C emitter — header files, struct definitions, function signatures
```

Emitters are independent. Adding a new language target means adding a new directory. Codegen modules do not share emit logic — idiomatic output requires language-specific decisions.

## Adapter System (`src/adapters/`)

Every graph primitive that requires platform-specific implementation is satisfied by an adapter. Adapters are registered in the policy file and resolved at compile time.

```text
Core adapters (first milestone):
  PostgresAdapter   — persistent resource via SQLx
  AxumAdapter       — HTTP capability via Axum route emit
  KafkaAdapter      — event stream via rdkafka
  ReactAdapter      — UI capability via React component emit
  TerraformAdapter  — cloud resource via HCL emit
  GHActionsAdapter  — CI/CD pipeline via GitHub Actions YAML emit
```

Rust is the single compilation substrate. All adapters emit Rust, or emit configuration/DSL files driven by Rust tooling. When a platform gap exists — no adapter covers a required primitive — it is a compilation error that names the missing capability. The resolution is to build the adapter in Rust and register it.

## CI/CD Emission (`src/ci/`)

Deployment topology, environment targets, and release gates are structural — they compile from the graph cleanly. The CI/CD emitters produce:
- GitHub Actions workflows
- Dockerfiles
- Helm charts
- Terraform modules

These are treated as compilation targets, not exports. They have the same determinism and loss-reporting requirements as code targets.

## Diagnostics and Loss Reporting (`src/diagnostics/`)

Every emitter produces a loss report alongside its output. The loss report records every graph semantic that could not be fully expressed in the target. Silent loss is a hard compiler error — the compiler cannot emit output without also emitting its complete loss report.

Three diagnostic levels:
- **Error** — compilation cannot complete (missing adapter, unresolved hole, exhaustiveness failure)
- **Warning** — compilation completes but with known semantic degradation
- **Loss** — compilation completes but specific semantics were dropped in this target; recorded per node

## Compilation Readiness vs. Architectural Validity

The compiler distinguishes two separate checks:

**Architectural validity** — does the graph correctly describe an architecture? No broken refs, no violated constraints, no invalid state machines. This check runs against TAKG.

**Compilation readiness for target X** — does the graph have enough information to compile to target X? All holes have registered adapters, all capabilities have bound implementations, all constraints have generators. This check runs after policy resolution and is target-specific.

A graph can be architecturally valid but not compilation-ready. The compiler reports the specific missing information, not a generic failure.

## Determinism

Same graph + same policy = identical output on every run. This is a hard requirement. The compiler uses deterministic ordering for all collections, deterministic name derivation, and no random or timestamp-based identifiers in emitted code.

## Incremental Recompilation

The compiler maintains a node hash manifest. On recompilation, only nodes whose hash has changed — and their downstream dependents — are re-emitted. On a 50-node graph with one changed node, this should complete in under one second.
