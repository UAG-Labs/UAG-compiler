<p align="center">
  <img src="./assets/banner/uag-labs-readme-banner.svg" alt="UAG-compiler banner" />
</p>

# UAG-compiler

Rust compiler and CLI for transforming TAKG source graphs into UAGL compiled architecture IR and generated outputs.

## Pipeline
```text
load → parse → resolve → normalize → validate → lower → emit UAGL → export → diagnostics/loss reports
```

## Owns
- compiler library
- CLI
- loaders/parsers/resolvers/normalizers
- validators
- lowering passes
- exporters
- diagnostics and loss reports
- diff/query/package support

## Does Not Own
- core data model definitions
- Studio UI
- root documentation examples as authoritative specs

## First CLI Targets
```bash
uag compile system.takg.yaml --out system.uagl.yaml
uag validate system.uagl.yaml
uag export system.uagl.yaml --target mermaid --out overview.mmd
```
