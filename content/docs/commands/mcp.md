+++
title = "hx mcp"
weight = 55
+++

Run an MCP (Model Context Protocol) server so AI agents can drive hx.

## Synopsis

```bash
hx mcp
```

## Description

`hx mcp` starts a [Model Context Protocol](https://modelcontextprotocol.io) server that speaks JSON-RPC 2.0 over stdio. Point an MCP client (Claude, an editor, etc.) at it and the agent gets tools for building, testing, running, locking, diagnosing, and managing dependencies — each backed by the `hx` CLI. Output is returned without ANSI colors.

It takes no options; it reads requests on stdin and writes responses on stdout.

## MCP client configuration

```json
{
  "mcpServers": {
    "hx": { "command": "hx", "args": ["mcp"] }
  }
}
```

## Tools

| Tool | Runs | Arguments (`*` = required) |
|------|------|----------------------------|
| `hx_doctor` | `hx doctor` | — |
| `hx_build` | `hx build` | `release`, `native`, `jobs` |
| `hx_check` | `hx check` | — |
| `hx_test` | `hx test` | `pattern` |
| `hx_run` | `hx run` | `args[]` |
| `hx_lock` | `hx lock` | `update` |
| `hx_sync` | `hx sync` | `force` |
| `hx_fmt` | `hx fmt` | `check` |
| `hx_lint` | `hx lint` | `fix` |
| `hx_add` | `hx add` | `package*`, `constraint`, `dev` |
| `hx_remove` | `hx rm` | `package*` |
| `hx_info` | `hx info` | `package*`, `versions` |
| `hx_tree` | `hx tree` | `depth` |
| `hx_outdated` | `hx outdated` | `direct` |

Every tool also accepts an optional `cwd` (the project directory to run in; defaults to the server's working directory). Each call returns the command's combined stdout/stderr plus an `isError` flag, which is `true` when the underlying command exits non-zero.

## How it works

The server implements the core MCP methods — `initialize`, `tools/list`, `tools/call`, and `ping` — and dispatches every tool call to the same `hx` binary as a subprocess. It holds no state between calls, so it is safe to start one server per workspace.

## See Also

- [Using hx with AI agents](@/docs/guides/ai-agents.md) — the full agent surface (MCP, AGENTS.md, skill, llms.txt)
- [hx doctor](@/docs/commands/doctor.md) — what `hx_doctor` reports
