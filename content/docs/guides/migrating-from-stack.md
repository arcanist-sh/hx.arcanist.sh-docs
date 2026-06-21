+++
title = "Migrating from Stack"
weight = 2
+++

Guide for moving a Stack project to hx.

## Overview

Stack and hx serve similar purposes but with different approaches:

| Aspect | Stack | hx |
|--------|-------|-----|
| Toolchain | Bundles GHC | Uses GHCup/ghcup |
| Resolver | LTS/Nightly | GHC version + lockfile |
| Config | stack.yaml | hx.toml |
| Lock | stack.yaml.lock | hx.lock |

## Quick Migration

The fastest way to adopt hx is `hx import --from stack`, which reads
`stack.yaml` in place — handling the resolver, multi-package layouts, and
`extra-deps` — and generates `hx.toml` for you:

```bash
cd my-stack-project

# Adopt the existing Stack project
hx import --from stack

# Generate lockfile
hx lock

# Build
hx build

# Test
hx test
```

You do **not** need to delete `stack.yaml` to get started — `import` reads it
where it is. A multi-package Stack project is imported as a
[workspace](@/docs/guides/workspaces.md).

> Prefer to hand-tune the configuration instead? Use
> [`hx init --detect`](@/docs/commands/init.md) and follow the
> [Step-by-Step](#step-by-step-migration) section below.

## Step-by-Step Migration

These steps use `hx init` as an alternative to `hx import` when you want to
review each piece of configuration by hand.

### 1. Analyze Your Stack Project

Check your `stack.yaml`:

```yaml
resolver: lts-22.0
packages:
  - .
extra-deps:
  - some-package-1.0.0
```

Note:
- Resolver version
- Extra dependencies
- Any special configuration

### 2. Initialize hx

```bash
hx init --detect
```

hx will:
- Read your `stack.yaml`
- Determine the GHC version from the resolver
- Create `hx.toml`

### 3. Review hx.toml

The generated `hx.toml`:

```toml
[project]
name = "my-project"
version = "0.1.0"

[toolchain]
ghc = "9.6.5"  # From LTS-22.0

[build]
ghc-options = ["-Wall"]
```

### 4. Handle Extra Dependencies

Stack's `extra-deps` need to be handled:

**Option A: Add to .cabal file**

If available on Hackage, add directly:

```cabal
build-depends:
  , some-package ^>=1.0
```

**Option B: Add as source dependency**

For packages not on Hackage, add to `cabal.project`:

```cabal
source-repository-package
  type: git
  location: https://github.com/user/some-package
  tag: v1.0.0
```

### 5. Generate Lockfile

```bash
hx lock
```

This creates `hx.lock` with pinned versions.

### 6. Verify Build

```bash
hx build
hx test
```

### 7. Clean Up (Optional)

hx never requires you to remove `stack.yaml` — it is read in place during
import and ignored afterward, so you can keep Stack and hx side by side
indefinitely. Only once you are confident in the migration, and only if you no
longer want Stack at all, remove the Stack files:

```bash
# Remove Stack files (optional, after you're confident)
rm stack.yaml stack.yaml.lock

# Keep .cabal file - it's still needed
```

## Resolver Mapping

Common Stack resolvers and their GHC versions:

| Resolver | GHC Version |
|----------|-------------|
| lts-22.0 | 9.6.4 |
| lts-21.0 | 9.4.7 |
| lts-20.0 | 9.2.8 |
| nightly-2024-01-01 | 9.8.1 |

Find the GHC version for any resolver:
```bash
# Check resolver's GHC version
curl -s https://www.stackage.org/lts-22.0 | grep ghc-version
```

## Handling Stack Features

### Stack Scripts

Stack's script interpreter:

```haskell
#!/usr/bin/env stack
-- stack script --resolver lts-22.0

main = putStrLn "Hello"
```

Use GHC directly or create a minimal project instead.

### Custom Snapshots

If using custom snapshots, add dependencies to `cabal.project`:

```cabal
source-repository-package
  type: git
  location: https://github.com/user/custom-package
  tag: v1.0.0
```

### Package Flags

Stack's `flags`:

```yaml
flags:
  some-package:
    some-flag: true
```

Convert to `cabal.project`:

```cabal
package some-package
  flags: +some-flag
```

### Extra Include/Lib Dirs

Stack's `extra-include-dirs`:

```yaml
extra-include-dirs:
  - /usr/local/include
extra-lib-dirs:
  - /usr/local/lib
```

Convert to `cabal.project`:

```cabal
package *
  extra-include-dirs: /usr/local/include
  extra-lib-dirs: /usr/local/lib
```

## Side-by-Side Usage

You can use Stack and hx in the same project:

```bash
# Build with Stack
stack build

# Build with hx
hx build
```

Both read from the same `.cabal` file.

## Comparison

### Commands

| Stack | hx |
|-------|-----|
| `stack build` | `hx build` |
| `stack test` | `hx test` |
| `stack run` | `hx run` |
| `stack ghci` | `hx repl` |
| `stack clean` | `hx clean` |
| `stack setup` | `hx toolchain install` |

### Files

| Stack | hx |
|-------|-----|
| `stack.yaml` | `hx.toml` |
| `stack.yaml.lock` | `hx.lock` |
| `.stack-work/` | `dist-newstyle/` |

## Troubleshooting

### Resolver Not Found

```
error: Unknown resolver: lts-22.0
```

Specify GHC version explicitly:

```bash
hx init --ghc 9.6.5
```

### Missing Dependencies

If build fails with missing packages:

```bash
# Check what's needed
hx build 2>&1 | grep "unknown package"

# Add missing packages
hx add missing-package
```

### GHC Version Mismatch

Stack and hx may use different GHC versions:

```bash
# See Stack's GHC
stack ghc -- --version

# See hx's GHC
hx toolchain list

# Install matching version
hx toolchain install --ghc 9.6.5
```

## Benefits of Migration

After migrating to hx:

1. **Unified tooling** — Same GHC used for all tools
2. **Faster builds** — Better caching in some scenarios
3. **Modern lockfile** — TOML format with checksums
4. **BHC support** — Alternative compiler backend
5. **Better diagnostics** — Improved error messages

## See Also

- [hx import](@/docs/commands/import.md) — Adopt an existing Stack project
- [hx init](@/docs/commands/init.md) — Initialize hx
- [hx lock](@/docs/commands/lock.md) — Generate lockfile
- [Workspaces](@/docs/guides/workspaces.md) — Multi-package projects
- [Configuration](@/docs/configuration/hx-toml.md) — hx.toml reference
