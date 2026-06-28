# memor.re agent surface

Cross-surface memory and Council skills for AI coding tools. Open a fresh session on a Claude surface — memor.re brings your context with you.

## Install

```
/plugin install memor-re
```

Or install directly:

```
claude plugin install memor-re github:memor-re/agent-surface
```

## What's included

20 skills across the memor.re Council bench:

`/benefactor` `/clean` `/closer` `/council` `/deck` `/design` `/ghosthunter` `/handoff` `/infra` `/integrator` `/learn` `/letsgo` `/memor-re` `/negotiator` `/refero-design` `/stabilizer` `/task-manager` `/ui-optimization-loop` `/values` `/visionary`

## MCP endpoint

`https://memor.re/api/memor-re/mcp`

Authentication: Clerk OAuth 2.1 (RFC 9728). No per-container token required — the server auto-provisions your workspace on first connection.

## How memory works

1. Install memor.re on a Claude surface (Claude Code CLI, Claude.ai, Claude Desktop)
2. Work — memories accumulate across sessions
3. Open a new session on any Claude surface — `memory.list_recent` surfaces your prior context automatically
4. Your memory follows you across Claude Code CLI, Claude.ai, and Claude Desktop

## Architecture

This is the public plugin shell. Authentication and private data live server-side behind Clerk OAuth. No secrets are bundled here.

- Source of truth: [memor-re/platform](https://github.com/memor-re/platform) (private)
- Distribution model: [ADR-009](https://github.com/memor-re/platform/blob/main/docs/decisions/ADR-009-provider-agnostic-public-plugin-distribution.md)
- Session continuity: [ADR-012](https://github.com/memor-re/platform/blob/main/docs/decisions/ADR-012-cross-surface-session-continuity.md)

## License

MIT
