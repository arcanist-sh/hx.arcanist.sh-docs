+++
title = "Compiler Backends"
weight = 1
+++

hx supports multiple Haskell compiler backends, allowing you to choose the best compiler for your use case.

## Overview

hx provides a unified interface for different compilers:

| Backend | Description | Best For |
|---------|-------------|----------|
| **GHC** | Standard Haskell compiler | Most projects, libraries |
| **BHC** | Basel Haskell Compiler | Numerical computing, ML |

## GHC Backend (Default)

GHC (Glasgow Haskell Compiler) is the standard Haskell compiler and the default backend.

### Configuration

```toml
[compiler]
backend = "ghc"

[compiler.ghc]
version = "9.8.2"
options = ["-Wall", "-Wextra"]
```

### Usage

```bash
# Build with GHC (default)
hx build

# Explicitly specify GHC
hx build --backend ghc
```

### GHC Features

- Full Haskell 2010 and GHC extensions support
- Excellent optimization (-O2)
- Mature ecosystem support
- HLS (Haskell Language Server) integration
- Extensive profiling tools

## BHC Backend

BHC (Basel Haskell Compiler) is an alternative compiler optimized for numerical computing and machine learning workloads.

### Configuration

```toml
[compiler]
backend = "bhc"

[compiler.bhc]
profile = "numeric"          # Optimization profile
emit_kernel_report = true    # Generate optimization reports
tensor_fusion = true         # Enable tensor fusion
target = "aarch64-linux-gnu" # Cross-compilation target
```

### Usage

All major commands support the BHC backend:

```bash
# Build with BHC
hx build --backend bhc

# Test with BHC
hx test --backend bhc

# Run with BHC
hx run --backend bhc

# Set in hx.toml to always use BHC
# [compiler]
# backend = "bhc"
```

### BHC Profiles

BHC offers specialized optimization profiles:

#### Default Profile

```toml
[compiler.bhc]
profile = "default"
```

Balanced optimization for general use.

#### Server Profile

```toml
[compiler.bhc]
profile = "server"
```

Optimized for:
- Long-running processes
- High throughput
- Consistent latency

#### Numeric Profile

```toml
[compiler.bhc]
profile = "numeric"
```

Optimized for:
- Numerical computation
- Array operations
- Linear algebra
- Machine learning workloads

Features:
- Tensor fusion
- SIMD optimizations
- Efficient array representations

#### Edge Profile

```toml
[compiler.bhc]
profile = "edge"
```

Optimized for:
- Small binary size
- Low memory usage
- Embedded systems
- Edge computing

### BHC-Specific Options

#### Kernel Reports

Generate reports on kernel optimization:

```toml
[compiler.bhc]
emit_kernel_report = true
```

Reports are written to `.hx/bhc-reports/`.

#### Tensor Fusion

Enable automatic tensor operation fusion:

```toml
[compiler.bhc]
tensor_fusion = true
```

This optimizes chains of array operations.

### BHC Targets

BHC supports these compilation targets:

| Target | Description |
|--------|-------------|
| `x86_64-linux-gnu` | Linux x86_64 |
| `aarch64-linux-gnu` | Linux ARM64 |
| `x86_64-apple-darwin` | macOS Intel |
| `aarch64-apple-darwin` | macOS Apple Silicon |
| `wasm32-wasi` | WebAssembly |
| `riscv64-linux-gnu` | RISC-V 64-bit |

## Switching Backends

### Per-Command

Override the configured backend for a single command:

```bash
hx build --backend ghc
hx build --backend bhc
```

### Per-Project

Set in `hx.toml`:

```toml
[compiler]
backend = "bhc"
```

### Per-Environment

Use environment variables:

```bash
export HX_BACKEND=bhc
hx build
```

## Backend Comparison

### Compilation Speed

| Backend | Debug | Release |
|---------|-------|---------|
| GHC | Fast | Moderate |
| BHC | Moderate | Fast (for numeric) |

### Runtime Performance

| Workload | GHC | BHC |
|----------|-----|-----|
| General | Excellent | Good |
| Numeric | Good | Excellent |
| Parsing | Excellent | Good |
| ML/Arrays | Good | Excellent |

### Binary Size

| Backend | Size |
|---------|------|
| GHC | Moderate |
| BHC (edge) | Small |

### Ecosystem Support

| Feature | GHC | BHC |
|---------|-----|-----|
| Hackage packages | All | Most |
| HLS | Yes | No |
| Profiling | Extensive | Basic |
| Template Haskell | Yes | Limited |

## Installing Backends

### Install GHC

```bash
hx toolchain install --ghc 9.8.2
```

### Install BHC

```bash
hx toolchain install --bhc 0.2.0
```

### List Installed

```bash
hx toolchain list
```

## Use Cases

### Use GHC When

- Building libraries for Hackage
- Needing full Template Haskell support
- Using HLS for IDE features
- Maximum ecosystem compatibility
- Detailed profiling needed

### Use BHC When

- Building ML/data science applications
- Heavy numerical computation
- Targeting WebAssembly
- Optimizing for embedded/edge
- Cross-compiling to ARM

## BHC Platform Snapshots

BHC Platform provides curated package sets — the BHC equivalent of Stackage. Each snapshot contains packages verified to build and work together under BHC.

### Configuration

```toml
[compiler]
backend = "bhc"

[bhc-platform]
snapshot = "bhc-platform-2026.1"
```

### CLI Commands

```bash
# List available snapshots
hx bhc-platform list

# Show snapshot details
hx bhc-platform info bhc-platform-2026.1

# Show all packages in a snapshot
hx bhc-platform info bhc-platform-2026.1 --packages

# Set snapshot for current project
hx bhc-platform set bhc-platform-2026.1
```

### Lock Integration

When a BHC Platform snapshot is configured, `hx lock` pins package versions from the snapshot into the resolver, providing reproducible builds without manually specifying constraints.

See [hx bhc-platform](@/docs/commands/bhc-platform.md) for the full command reference.

## BHC Project Templates

hx includes project templates optimized for BHC:

```bash
# Numeric computing (hmatrix, vector, massiv, statistics)
hx new numeric my-science

# Web server (Servant, Warp, WAI)
hx new server my-api
```

You can also create standard templates with BHC:

```bash
hx init --backend bhc
hx new webapp my-app --backend bhc
hx new cli my-tool --backend bhc
hx new library my-lib --backend bhc
```

## Example: ML Project

A project using BHC for ML workloads:

```toml
# hx.toml
[project]
name = "ml-classifier"

[compiler]
backend = "bhc"

[compiler.bhc]
profile = "numeric"
tensor_fusion = true
emit_kernel_report = true

[bhc-platform]
snapshot = "bhc-platform-2026.1"

[build]
release = true
```

```bash
# Build optimized ML binary
hx build --release

# Test
hx test

# Run
hx run -- --input data.csv

# Check optimization report
cat .hx/bhc-reports/kernel-report.txt
```

## See Also

- [hx build](@/docs/commands/build.md) — Build command
- [hx bhc-platform](@/docs/commands/bhc-platform.md) — BHC Platform snapshots
- [hx new](@/docs/commands/new.md) — Project templates
- [Cross-Compilation](@/docs/features/cross-compilation.md) — Building for other platforms
- [Toolchain Management](@/docs/features/toolchain.md) — Managing compilers
