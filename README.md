# memor.re agent surface

Installable memor.re Council and product-operating skills for AI coding tools.

## Install

`
/plugin install memor-re
`

Or install directly:

`
claude plugin install memor-re github:memor-re/agent-surface
`

## What's included

20 skills across the memor.re Council bench:
/benefactor /clean /closer /council /deck /design /ghosthunter /handoff /infra /integrator /learn /letsgo /memor-re /negotiator /refero-design /stabilizer /task-manager /ui-optimization-loop /values /visionary

MCP endpoint: https://memor.re/api/memor-re/mcp (Clerk OAuth, no per-container token required)

## Architecture

This is the public plugin shell. Authentication and private data live server-side behind Clerk OAuth. No secrets are bundled here.

Source of truth: [memor-re/platform](https://github.com/memor-re/platform) (private)
Distribution model: [ADR-009](https://github.com/memor-re/platform/blob/main/docs/decisions/ADR-009-provider-agnostic-public-plugin-distribution.md)

## License

MIT