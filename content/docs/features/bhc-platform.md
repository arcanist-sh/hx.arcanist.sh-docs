+++
title = "BHC Platform"
weight = 2
+++

BHC Platform provides curated package snapshots for the BHC (Basel Haskell Compiler) backend — the BHC equivalent of [Stackage](https://www.stackage.org/).

## Overview

When using the BHC backend, BHC Platform snapshots provide a set of packages verified to build and work together. This gives you reproducible builds without manually specifying version constraints for every dependency.

## Quick Start

```bash
# List available snapshots
hx bhc-platform list

# Set a snapshot for your project
hx bhc-platform set bhc-platform-2026.1

# Lock dependencies using the snapshot
hx lock
```

Or configure directly in `hx.toml`:

```toml
[compiler]
backend = "bhc"

[bhc-platform]
snapshot = "bhc-platform-2026.1"
```

## How It Works

1. **Embedded snapshots** — BHC Platform data is embedded in hx, so no network fetch is required
2. **Version pinning** — When you run `hx lock`, package versions from the snapshot are pinned in the resolver
3. **Lockfile recording** — The lockfile records which snapshot was used for reproducibility
4. **Hackage fallback** — Packages not in the snapshot can be resolved from Hackage if `allow_newer = true`

## Available Snapshots

### bhc-platform-2026.1

The initial BHC Platform snapshot:

- **BHC version:** 0.1.0
- **GHC compatibility:** 9.8.2
- **Packages:** ~70

Covers the core Haskell ecosystem plus specialized packages for web, numeric, and server workloads:

| Category | Packages |
|----------|----------|
| **Core** | base, ghc-prim, integer-gmp, template-haskell |
| **Text & ByteString** | text, bytestring, utf8-string |
| **Containers** | containers, unordered-containers, hashable, vector |
| **Transformers** | mtl, transformers, exceptions, unliftio |
| **Serialization** | aeson, yaml, binary, cereal |
| **Optics** | lens, microlens, generic-lens |
| **Parsing** | megaparsec, attoparsec, optparse-applicative |
| **HTTP & Web** | http-client, http-types, servant, servant-server, warp, wai, wai-extra |
| **Database** | persistent, esqueleto, resource-pool |
| **Numeric** | hmatrix, statistics, massiv, random |
| **Testing** | hspec, QuickCheck, tasty, hedgehog, hspec-wai |
| **Streaming** | conduit, streaming, pipes |
| **CLI & Logging** | optparse-applicative, monad-logger, fast-logger, co-log |

## Configuration

### Basic

```toml
[bhc-platform]
snapshot = "bhc-platform-2026.1"
```

### With Extra Dependencies

Allow packages outside the snapshot and override specific versions:

```toml
[bhc-platform]
snapshot = "bhc-platform-2026.1"
allow_newer = true
extra_deps = { my-custom-lib = "1.0.0" }
```

### Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `snapshot` | string | none | Snapshot identifier (e.g., `bhc-platform-2026.1`) |
| `allow_newer` | bool | `false` | Allow packages from Hackage outside the snapshot |
| `extra_deps` | table | `{}` | Override specific package versions |

## BHC Platform vs Stackage

| Feature | BHC Platform | Stackage |
|---------|-------------|----------|
| **For** | BHC backend | GHC backend |
| **Delivery** | Embedded in hx | Fetched from stackage.org |
| **Package count** | ~70 (curated) | ~3000+ |
| **Offline** | Yes | After first fetch |
| **Configuration** | `[bhc-platform]` | `[toolchain].resolver` |

Use BHC Platform when `[compiler].backend = "bhc"`. Use Stackage when using GHC.

## Project Templates

The `numeric` and `server` project templates come pre-configured with BHC Platform:

```bash
# Numeric computing project
hx new numeric my-science
# → backend = "bhc", profile = "numeric", snapshot = pre-configured

# Web server project
hx new server my-api
# → backend = "bhc", profile = "server", snapshot = pre-configured
```

## CLI Commands

| Command | Description |
|---------|-------------|
| `hx bhc-platform list` | List available snapshots |
| `hx bhc-platform info <platform>` | Show snapshot details |
| `hx bhc-platform info <platform> --packages` | Show all packages |
| `hx bhc-platform set <platform>` | Set snapshot in hx.toml |

See [hx bhc-platform](/docs/commands/bhc-platform) for the full command reference.

## Troubleshooting

### Package not in snapshot

```
error: Could not resolve dependency: some-package
```

Set `allow_newer = true` in `[bhc-platform]` to allow resolving from Hackage, or add the package to `extra_deps`.

### BHC not installed

```
error: BHC is not installed
```

Install BHC with `hx toolchain install --bhc latest`.

### Using with GHC backend

BHC Platform snapshots are only applied when `[compiler].backend = "bhc"`. If you're using GHC, configure a [Stackage resolver](/docs/features/lockfiles) instead.

## See Also

- [Compiler Backends](/docs/features/compiler-backends) — GHC and BHC overview
- [hx bhc-platform](/docs/commands/bhc-platform) — Command reference
- [hx.toml Reference](/docs/configuration/hx-toml) — Configuration details
- [hx new](/docs/commands/new) — BHC project templates
