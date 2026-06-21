+++
title = "hx deps"
weight = 24
+++

Inspect the resolved dependency graph.

## Synopsis

```bash
hx deps <SUBCOMMAND>
```

## Subcommands

| Subcommand | Description |
|------------|-------------|
| `graph` | Print the dependency graph |
| `tree` | Show dependencies as a tree |
| `list` | List all resolved dependencies |

All subcommands read `hx.lock`. Run [`hx lock`](@/docs/commands/lock.md) first if
you do not have a lockfile yet.

## hx deps graph

Print the resolved dependency graph.

```bash
hx deps graph
```

```
my-project
├── aeson 2.1.2.1
│   ├── text 2.0.2
│   └── vector 0.13.1.0
├── text 2.0.2
└── containers 0.6.7
```

## hx deps tree

Show dependencies as a tree rooted at your project, expanding transitive
dependencies under each parent.

```bash
hx deps tree
```

```
my-project
├── aeson 2.1.2.1
│   ├── base 4.18.2.1
│   ├── bytestring 0.11.5.3
│   ├── text 2.0.2
│   └── vector 0.13.1.0
│       └── primitive 0.9.0.0
└── text 2.0.2
```

## hx deps list

List every resolved dependency as a flat, sorted list of name and version.

```bash
hx deps list
```

```
aeson      2.1.2.1
base       4.18.2.1
bytestring 0.11.5.3
containers 0.6.7
primitive  0.9.0.0
text       2.0.2
vector     0.13.1.0
```

This is convenient for scripting and for diffing the dependency set between
revisions.

## See Also

- [hx why](@/docs/commands/why.md) — Explain why a package is in the graph
- [hx lock](@/docs/commands/lock.md) — Generate the lockfile
- [hx outdated](@/docs/commands/outdated.md) — Check for newer versions
- [Dependency Resolution](@/docs/concepts/dependency-resolution.md)
