+++
title = "Documentation"
template = "section.html"
sort_by = "weight"
+++

**hx** is the whole Haskell toolchain in one fast binary. It installs GHC and Cabal, builds, tests, formats and lints your code, pins every dependency by hash, and publishes your packages. It is written in Rust, starts in milliseconds, and works with the `.cabal` files and Hackage you already use.

## What is hx?

hx drives GHC and Cabal rather than replacing them, and adds what they leave to you:

- **Managed toolchains**: GHC, Cabal and HLS from the release metadata ghcup publishes, with every download verified
- **Verifiable lockfiles**: every dependency pinned by source hash and Hackage revision, with offline builds and a CI check
- **Workspaces**: multi-package projects with `cabal.project` as the single member list, and `--changed` to act only on what a branch affects
- **Batteries included**: format, lint, test, coverage, profiling, documentation and publishing in one tool
- **Agents**: an MCP server (`hx mcp`) that exposes the same commands to AI agents
- **Two compilers**: GHC by default, and [BHC](https://arcanist.sh/bhc/) with one setting

New here? Start with [Installation](@/docs/installation.md) and the [Quick Start](@/docs/quickstart.md). The overview, benchmarks and release notes live on [arcanist.sh/hx](https://arcanist.sh/hx/).

## Documentation Sections

### [Getting Started](@/docs/installation.md)
Install hx and create your first project.

### [Commands Reference](@/docs/commands/_index.md)
Complete reference for all hx CLI commands.

### [Configuration Reference](@/docs/configuration/_index.md)
Full documentation for `hx.toml` configuration files.

### [Features](@/docs/features/_index.md)
In-depth coverage of hx features like compiler backends, cross-compilation, and toolchain management.

### [Concepts](@/docs/concepts/_index.md)
Understanding how hx works: project structure, dependency resolution, and build system.

### [Guides](@/docs/guides/_index.md)
Step-by-step tutorials and workflow guides.

### [Development](@/docs/development/_index.md)
Benchmarks, testing, and contributing to hx.

## Quick Links

- [Installation](@/docs/installation.md)
- [Quick Start](@/docs/quickstart.md)
- [CLI Reference](@/docs/commands/_index.md)
- [hx.toml Reference](@/docs/configuration/hx-toml.md)
- [Benchmarks](@/docs/development/benchmarks.md)
- [Troubleshooting](@/docs/guides/troubleshooting.md)
