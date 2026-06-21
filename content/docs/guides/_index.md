+++
title = "Guides"
weight = 50
template = "section.html"
sort_by = "weight"
+++

Step-by-step tutorials and workflow guides for common tasks.

## Tutorials

- **[CI/CD Setup](@/docs/guides/ci-cd.md)** — Set up continuous integration
- **[Migrating from Stack](@/docs/guides/migrating-from-stack.md)** — Move a Stack project to hx
- **[Migrating from Cabal](@/docs/guides/migrating-from-cabal.md)** — Adopt hx in a Cabal project
- **[Creating a Library](@/docs/guides/creating-library.md)** — Build and publish a library
- **[Workspaces](@/docs/guides/workspaces.md)** — Multi-package projects

## Workflows

- **[Development Workflow](@/docs/guides/development-workflow.md)** — Day-to-day development
- **[Using hx with AI Agents](@/docs/guides/ai-agents.md)** — MCP server, AGENTS.md, skill, and llms.txt
- **[Troubleshooting](@/docs/guides/troubleshooting.md)** — Common issues and solutions

## Quick Reference

### New Project

```bash
hx new my-project
cd my-project
hx build
hx test
```

### Existing Project

```bash
cd existing-project
hx import --from cabal   # or: --from stack
hx lock
hx build
```

### Daily Development

```bash
hx watch test          # TDD workflow
hx build --release     # Production build
hx doctor              # Check environment
```
