+++
title = "hx import"
weight = 15
+++

Adopt an existing Cabal or Stack project.

## Synopsis

```bash
hx import --from <SOURCE> [OPTIONS]
```

## Description

The `import` command adopts an existing Haskell project into hx by reading its
existing configuration and generating an `hx.toml`. Unlike `hx init`, which
starts from a blank slate, `import` is built around discovering and preserving
the layout you already have — multiple packages, extra dependencies, and
toolchain settings.

It is the recommended entry point for bringing an existing project under hx.

## Options

```
    --from <SOURCE>     Project format to import [cabal, stack]
    --force             Overwrite an existing hx.toml
-v, --verbose           Show detailed output
```

## Importing from Cabal

```bash
cd my-cabal-project
hx import --from cabal
hx lock
```

This works whether or not you have a `cabal.project`:

- **With `cabal.project`** — every listed local package is recorded. A project
  listing several local packages is imported as a [workspace](@/docs/guides/workspaces.md).
- **With only a bare `.cabal` file** (no `cabal.project`) — the directory is
  treated as a single-package project, and `hx.toml` is generated directly from
  the `.cabal` file.

## Importing from Stack

```bash
cd my-stack-project
hx import --from stack
hx lock
```

hx reads `stack.yaml` and:

- Maps the resolver to an equivalent GHC version
- Records every local package (multi-package Stack projects become
  [workspaces](@/docs/guides/workspaces.md))
- Carries over `extra-deps`

You do **not** need to delete `stack.yaml` first — `import` reads it in place.

## After Import

Run `hx lock` to generate the lockfile, then build:

```bash
hx lock
hx build
```

## Discoverability

If you run a project command (such as `hx build`) outside an hx project but
inside a Cabal or Stack project, hx detects this and suggests the right
adoption path:

```
error: no hx project found in this directory or any parent

  This looks like a Cabal project. To adopt it:
      hx import --from cabal

  Or start fresh with:
      hx init
```

The same hint appears for Stack projects, suggesting `hx import --from stack`.

## import vs init

| | `hx import` | `hx init` |
|--|-------------|-----------|
| Purpose | Adopt an existing Cabal/Stack project | Initialize hx config in place |
| Multi-package | Detects workspace members | Single package |
| Reads | `cabal.project` / `stack.yaml`, `.cabal` | Existing config, best-effort |

Use `hx import` to adopt an established project; reach for `hx init` for a
minimal, hand-tuned setup.

## See Also

- [hx init](@/docs/commands/init.md) — Initialize hx in place
- [hx lock](@/docs/commands/lock.md) — Generate the lockfile after import
- [Workspaces](@/docs/guides/workspaces.md) — Multi-package projects
- [Migrating from Cabal](@/docs/guides/migrating-from-cabal.md)
- [Migrating from Stack](@/docs/guides/migrating-from-stack.md)
