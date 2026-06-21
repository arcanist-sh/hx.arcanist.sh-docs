+++
title = "hx why"
weight = 23
+++

Explain why a package is in the dependency graph.

## Synopsis

```bash
hx why <PACKAGE>
```

## Description

The `why` command explains why a given package appears in your resolved
dependency graph. It reads `hx.lock` and traces the dependency paths that pull
the package in — whether you depend on it directly or it arrives as a
transitive dependency of something else.

This answers questions like "I never asked for `vector` — what's bringing it
in?" without manually walking the lockfile.

## Examples

### Direct Dependency

```bash
hx why text
```

```
text 2.0.2 is required by:
  my-project (direct dependency)
```

### Transitive Dependency

```bash
hx why primitive
```

```
primitive 0.9.0.0 is required by:
  my-project → aeson → vector → primitive
  my-project → vector → primitive
```

Each line is a path from your project to the package, so you can see every
reason it is in the graph.

## Notes

- `hx why` reads `hx.lock`. Run [`hx lock`](@/docs/commands/lock.md) first if you
  do not have a lockfile yet.
- Only packages present in the resolved graph can be explained. If a package
  isn't in the lockfile, `why` reports that it is not part of the build plan.

## See Also

- [hx deps](@/docs/commands/deps.md) — Inspect the dependency graph
- [hx lock](@/docs/commands/lock.md) — Generate the lockfile
- [hx outdated](@/docs/commands/outdated.md) — Check for newer versions
