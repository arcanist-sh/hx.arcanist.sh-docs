+++
title = "Commands Reference"
weight = 10
template = "section.html"
sort_by = "weight"
+++

Complete reference for all hx CLI commands.

## Command Categories

### Build Commands
Commands for building, running, and testing your project.

| Command | Description |
|---------|-------------|
| [build](@/docs/commands/build.md) | Compile the project |
| [run](@/docs/commands/run.md) | Build and run the executable |
| [test](@/docs/commands/test.md) | Run the test suite |
| [bench](@/docs/commands/bench.md) | Run benchmarks |
| [check](@/docs/commands/check.md) | Type check without compiling |
| [repl](@/docs/commands/repl.md) | Start interactive GHCi session |
| [doc](@/docs/commands/doc.md) | Generate documentation |

### Project Management
Commands for creating and managing projects.

| Command | Description |
|---------|-------------|
| [new](@/docs/commands/new.md) | Create a new project |
| [init](@/docs/commands/init.md) | Initialize hx in existing project |
| [add](@/docs/commands/add.md) | Add dependencies |
| [remove](@/docs/commands/remove.md) | Remove dependencies |
| [update](@/docs/commands/update.md) | Update dependencies |

### Dependency Management
Commands for managing dependencies and lockfiles.

| Command | Description |
|---------|-------------|
| [lock](@/docs/commands/lock.md) | Generate/update lockfile |
| [sync](@/docs/commands/sync.md) | Sync dependencies from lockfile |
| [outdated](@/docs/commands/outdated.md) | Check for outdated dependencies |

### Toolchain Management
Commands for managing the Haskell toolchain.

| Command | Description |
|---------|-------------|
| [toolchain](@/docs/commands/toolchain.md) | Manage GHC, Cabal, and other tools |
| [bhc-platform](@/docs/commands/bhc-platform.md) | Manage BHC Platform curated snapshots |

### Utilities
Additional utility commands.

| Command | Description |
|---------|-------------|
| [clean](@/docs/commands/clean.md) | Remove build artifacts |
| [fmt](@/docs/commands/fmt.md) | Format Haskell code |
| [lint](@/docs/commands/lint.md) | Run linter |
| [watch](@/docs/commands/watch.md) | Watch mode for auto-rebuild |
| [doctor](@/docs/commands/doctor.md) | Diagnose environment issues |

## Global Options

These options are available for all commands:

```
-v, --verbose     Enable verbose output
-q, --quiet       Suppress non-essential output
    --color       Control color output [always, auto, never]
    --no-color    Disable colored output
-h, --help        Print help information
-V, --version     Print version information
```

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | General error |
| 2 | Usage error (invalid arguments) |
| 3 | Configuration error |
| 4 | Toolchain error |
| 5 | Build/test failure |
