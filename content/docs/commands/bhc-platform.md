+++
title = "hx bhc-platform"
weight = 22
+++

Manage BHC Platform curated snapshots.

## Synopsis

```bash
hx bhc-platform <COMMAND>
```

## Description

The `bhc-platform` command manages BHC Platform curated snapshots — Stackage-like package sets optimized for the BHC (Basel Haskell Compiler) backend. Each snapshot provides a set of packages verified to build and work together under BHC.

## Subcommands

### list

List all available BHC Platform snapshots.

```bash
hx bhc-platform list
```

**Output:**

```
  Available  BHC Platform snapshots

  bhc-platform-2026.1 (BHC 0.1.0, GHC-compat 9.8.2, 70 packages)

Set a platform with: hx bhc-platform set <platform>
```

### info

Show detailed information about a snapshot.

```bash
hx bhc-platform info <PLATFORM> [OPTIONS]
```

**Options:**

```
    --packages    Show all packages in the snapshot
```

**Examples:**

```bash
# Show snapshot metadata
hx bhc-platform info bhc-platform-2026.1

# Show all packages
hx bhc-platform info bhc-platform-2026.1 --packages
```

**Output:**

```
  Platform  bhc-platform-2026.1

  GHC compatibility: 9.8.2
  Packages:          70
  BHC version:       0.1.0
  Recommended profile: default
```

### set

Set the BHC Platform snapshot for the current project.

```bash
hx bhc-platform set <PLATFORM>
```

This updates the `[bhc-platform].snapshot` field in your `hx.toml`.

**Example:**

```bash
hx bhc-platform set bhc-platform-2026.1
```

**Output:**

```
  Set  BHC Platform to bhc-platform-2026.1 (70 packages, BHC 0.1.0)
```

## Available Snapshots

### bhc-platform-2026.1

The initial BHC Platform snapshot, curated for BHC 0.1.0 with GHC 9.8.2 compatibility. Contains ~70 packages covering:

| Category | Key Packages |
|----------|-------------|
| Core | base, ghc-prim, template-haskell |
| Text & Data | text, bytestring, aeson, yaml |
| Containers | containers, vector, unordered-containers |
| Transformers | mtl, transformers, unliftio |
| Web | servant, servant-server, warp, wai |
| Numeric | hmatrix, statistics, massiv |
| Testing | hspec, QuickCheck, tasty, hedgehog |
| Parsing | megaparsec, attoparsec, optparse-applicative |

## Configuration

```toml
[compiler]
backend = "bhc"

[bhc-platform]
snapshot = "bhc-platform-2026.1"
allow_newer = false
extra_deps = {}
```

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `snapshot` | string | none | Snapshot identifier |
| `allow_newer` | bool | `false` | Allow packages outside the snapshot |
| `extra_deps` | table | `{}` | Override specific package versions |

## How It Works

1. BHC Platform snapshots are embedded in hx (no network fetch required)
2. When you run `hx lock` with a snapshot configured and `backend = "bhc"`, package versions from the snapshot are pinned in the resolver
3. The lockfile records which snapshot was used
4. Additional dependencies from Hackage can be allowed with `allow_newer = true`

## Snapshot Identifiers

| Format | Example | Description |
|--------|---------|-------------|
| `bhc-platform-YYYY.N` | `bhc-platform-2026.1` | Specific snapshot version |

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Invalid platform identifier or platform not found |

## See Also

- [Compiler Backends](/docs/features/compiler-backends) — GHC and BHC support
- [hx.toml Reference](/docs/configuration/hx-toml) — `[bhc-platform]` configuration
- [hx lock](/docs/commands/lock) — Generate lockfile with snapshot pinning
- [hx new](/docs/commands/new) — Create projects with BHC templates
