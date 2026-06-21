+++
title = "Workspaces"
weight = 6
+++

Work with multi-package projects in hx.

## Overview

A **workspace** is a project containing more than one local package. hx
recognizes a workspace whenever a `cabal.project` lists several local packages:

```cabal
-- cabal.project
packages:
  packages/core
  packages/cli
  packages/web
```

```
my-workspace/
├── packages/
│   ├── core/
│   │   ├── src/
│   │   └── core.cabal
│   ├── cli/
│   │   ├── app/
│   │   └── cli.cabal
│   └── web/
│       ├── src/
│       └── web.cabal
├── cabal.project
├── hx.toml
└── hx.lock
```

## Getting a Workspace

If you already have a multi-package Cabal or Stack project, adopt it with
[`hx import`](@/docs/commands/import.md):

```bash
hx import --from cabal   # or: --from stack
hx lock
```

Each local package becomes a workspace member.

## Building and Testing

By default, `hx build` and `hx test` operate on **all** workspace members, using
Cabal's `all` target:

```bash
# Build every member
hx build

# Test every member
hx test
```

### Targeting a Single Package

Use `--package <name>` to build or test just one member:

```bash
# Build only the `core` package
hx build --package core

# Test only the `cli` package
hx test --package cli
```

> Untargeted `hx build` / `hx test` across all members was fixed in 0.7.16.
> On earlier versions these commands required an explicit target.

## Custom Setup.hs

Members with `build-type: Custom` (a custom `Setup.hs`) build correctly — hx
delegates these to Cabal, so custom build logic is preserved.

## The Lockfile

A workspace produces a single `hx.lock` covering the whole project:

- Local members are recorded in a `[workspace]` section, with their paths.
- External dependencies are recorded as `[[packages]]` entries.

This keeps every member resolved against one consistent dependency set.

## See Also

- [hx import](@/docs/commands/import.md) — Adopt an existing Cabal/Stack project
- [hx build](@/docs/commands/build.md) — Build all members or a single one
- [hx test](@/docs/commands/test.md) — Test all members or a single one
- [Project Structure](@/docs/concepts/project-structure.md) — Project layouts
