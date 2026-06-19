+++
title = "Using hx with AI Agents"
weight = 7
+++

hx is built to be driven by AI coding agents, not just humans. It ships four agent-facing surfaces: an **MCP server**, an **`AGENTS.md`** guide, a **Claude Code skill**, and an **`llms.txt`** project map. Use whichever your tooling supports.

## MCP server (recommended)

[`hx mcp`](@/docs/commands/mcp.md) starts a [Model Context Protocol](https://modelcontextprotocol.io) server over stdio. Point any MCP client at it:

```json
{
  "mcpServers": {
    "hx": { "command": "hx", "args": ["mcp"] }
  }
}
```

The agent then has tool-call access to `hx_build`, `hx_check`, `hx_test`, `hx_run`, `hx_lock`, `hx_sync`, `hx_fmt`, `hx_lint`, `hx_doctor`, and the dependency tools (`hx_add`, `hx_remove`, `hx_info`, `hx_tree`, `hx_outdated`). Each returns the command's output and an `isError` flag.

## Claude Code skill

The repo includes a ready-to-use [Agent Skill](https://docs.anthropic.com/en/docs/claude-code/skills) at `.claude/skills/hx/SKILL.md`. Copy it into your own `.claude/skills/`, and Claude loads it automatically when it detects an hx project — giving it the command map, exit codes, and workflows without any prompting.

## AGENTS.md and llms.txt

- **[`AGENTS.md`](https://github.com/arcanist-sh/hx/blob/main/AGENTS.md)** (repo root) — a concise operating guide for any agent: exit codes, the MCP server, core commands, common workflows, and behavioral gotchas.
- **[`llms.txt`](https://arcanist.sh/hx/llms.txt)** — a structured project map for LLMs, linking the docs, agent guide, and source.

## What an agent should know

**Decide success/failure from the exit code, not by scraping output:**

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | General error |
| 2 | Usage error (bad arguments) |
| 3 | Configuration error (`hx.toml`) |
| 4 | Toolchain error — run `hx doctor` |
| 5 | Build/test failure |
| 6 | Plugin hook failure |

**Gotchas:**

- The first `hx build` may install a GHC (hundreds of MB). `hx doctor` returns exit `4` and prints how to fix a missing toolchain.
- `hx build --native` is experimental — a fast path for single-package, `base`-only projects; it falls back to cabal otherwise.
- Project-local plugins (`.hx/plugins/*.scm`) never run on a freshly cloned repo. Don't run `hx plugins trust` on a project you don't trust.

## A typical agent loop

```bash
hx doctor                       # exit 4 => fix the toolchain first
hx add aeson ">=2.0"            # change dependencies
hx build                        # exit 5 => read the structured error + fix
hx fmt --check && hx lint && hx test   # quality gate
```
