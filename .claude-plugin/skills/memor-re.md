---
name: memor-re
description: "Front door to the entire memor.re canon: route into Council, broadcast a learning to every agent, or set the memory mode. Use when the user types /memor-re, names memor.re as the system to act through, wants the whole stack rather than one specialist, or wants something remembered across the canon. For a single decision routed to one owner, use council instead."
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "memor-re",
  "registryRuleId": "durable-execution-agent-contract",
  "requiredProperties": [
    "journaled_steps",
    "idempotency_and_dedupe",
    "resume_and_replay",
    "durable_timers_and_signals",
    "authority_scoped_ground_truth",
    "rollback_or_compensation",
    "crash_restart_recovery",
    "split_brain_fencing"
  ],
  "authoritySurface": "docs/architecture/canonical_agent_governance_registry.json#agents.memor-re",
  "journalSurface": "hosted memory ledger (memory_list_recent + memory_search) is the orientation source of truth; docs/operations/* evidence packets and PR/check evidence are the proof surface. Host-app shell directories are never orientation or journal authority.",
  "idempotencyKey": "agent:memor-re:active-slice:authority-surface:validation-command",
  "resumeProof": [
    "npm run agent:governance:validate",
    "npm run close:validate",
    "npm run agent:lifecycle:preflight"
  ],
  "sideEffectPolicy": "No external, hosted, paid, secret, destructive, or user-level mutation without approval, backup, rollback or compensation, and post-action proof.",
  "longWaitPolicy": "Represent approvals, timers, hosted refreshes, blocked actions, and human handoffs as explicit lifecycle held actions with safe next actions.",
  "rollbackPath": "Use the cited validation command plus the owning evidence packet to restore, compensate, or classify the blocker as external-held.",
  "validationCommands": [
    "npm run agent:governance:validate",
    "npm run agent:lifecycle:preflight"
  ],
  "scaffoldLearnState": "Act, Notice, Check, Repair, Align are required before learned, fixed, clean, ready, shipped, or durable claims.",
  "owner": "Council"
}
-->


# memor.re

This is the product front door to the whole canon, not a specialist. `council` routes one decision to one owner; `memor-re` opens the entire stack. Use it for the three things a single specialist cannot own: routing into the bench, teaching the whole canon, and setting how memory is captured.

Do not treat this file as the source of truth for the specialists or the hierarchy. The canonical hierarchy lives in `memor_re/council/runtime/hierarchy.py` and `roster.py`; inspect it before making routing claims.

## The canon (hierarchy)

```
memor.re                     product front door (this skill)
└── council                  bench / router
    ├── board (6)            visionary · closer · benefactor · stabilizer · infra · values
    │   ├── $design → visionary       capability team
    │   └── $clean  → closer          capability team
    ├── emerging             task_manager → closer
    └── control              ghosthunter (ghost_checker) · weight_steward   (cross-cutting, NOT the board)
```

Canonical specialists own decisions; their domain teams (helpers, skills, tools) are subordinate, never Council peers (see `docs/planning/2026-06-03-council-hierarchy-routing.md`). Control agents like `ghosthunter` are reached *through* Council, never as peers of it.

## Session boot (required — every fresh chat, every surface)

> **SPLIT-BRAIN GUARD — read before anything else.**
> The local `MEMORY.md` file auto-loaded by the IDE/Claude Code auto-memory feature is **NOT memor.re**.
> It is a stale local file index on disk. Its presence in context does NOT mean orientation is complete.
> **"memor.re" = the hosted MCP system (mcp__memor-re-hosted__\*). "Local files" = filesystem. Never conflate them.**
> Call the hosted MCP tools FIRST — even when `MEMORY.md` is already visible in context. The session starts cold until hosted memor.re responds.

Before routing or acting, orient this session from **hosted memor.re** (not local files). This applies on every surface, whether AGENTS.md is present or not, and regardless of what local files appear in context:

1. Call `workspace_status` (`mcp__memor-re-hosted__workspace_status`) — check `write_scope.status`. If `"read_only"`, surface the `write_scope.action` message immediately so the user knows memor.re save is unavailable.
2. Call `memory_list_recent` (`mcp__memor-re-hosted__memory_list_recent`, limit: 20) — returns the user's active memor.re ledger.
3. Call `memory_search` (`mcp__memor-re-hosted__memory_search`) with terms matching the current task — surfaces prior context from hosted memor.re.
4. On Clerk OAuth surfaces, omit `installation_id` — the server resolves it. On legacy token surfaces, let the auth helper (`resolveInstallationId()` in `scripts/lib/memor-re-mcp.mjs`) resolve it; do not reach into local home-directory auth/state material by hand.

> **ANTI-PATTERN (never substitute):** Reading local `MEMORY.md`, any `.claude/` file, or any filesystem path as a replacement for steps 1–3. Those are IDE-surface local artifacts. The session does not have memor.re orientation until the hosted MCP tools have responded.

These three MCP calls are your **memor.re orientation packet** — name them that, not "memory orientation." Never route, plan, or implement before running them. A cold start wastes the user's continuity and defeats the product. A SCOPE_DENIED on memor.re save is a product failure, not an error — proactively tell the user at session start.

## What `/memor-re` does

1. **Route** — hand a request to Council, which owns specialist routing to the board. Prefer this over guessing an owner. `python -m memor_re.council.runtime --no-persist --json "<prompt>"`.
2. **Broadcast a learning** — "teach every agent X": write it to memory and thread it into each board owner's context so the whole canon carries it forward, not one chat.
3. **Memory mode** — set or show the active capture mode: `manual` (save on ask), `suggested` (propose for review), or `auto_reviewed` (remember-everything: milestones + trend rollups with review and undo). The lens is `council` (full bench) or a single board owner (e.g. `visionary` captures product/strategy milestones, `values` flags consent). See the memory design at `docs/design/2026-06-25-memor-re-canonical-plugin-surface-design.md`.
4. **Start a lane-scoped session** — a fresh chat that names a lane or intent (`/memor-re learn lane`, `/memor-re product feedback`, `/memor-re feature idea`, or any id/alias in `scripts/lane-domains.json`) is oriented straight into that lane's scope. Run `node scripts/lane-start.mjs "<lane-or-intent>"` after the session-boot calls above. It resolves the intent to a lane, checks for a conflicting worktree already on that lane's branch and an active hosted lease (`scripts/agent-run-lease.mjs inspect`), reports whether the tree is clean, and prints `owner` / `scope` (domain globs) / `backlog_lanes` plus a "safe to proceed" or a named blocker. See `docs/planning/lane-kickoffs.md` ("Lane-scoped session start") for the full contract, including the honesty caveat: this INSPECTS for conflicts, it does not itself hold a lease, so a session that will mutate files still binds one via `scripts/agent-run-lease.mjs bind` before its first write.

## Routing rule

- One decision, one owner → defer to `council` (or `/council`).
- Whole-canon learning, memory-mode change, or "act as memor.re across the stack" → handle here, then delegate execution to the smallest capable owner.
- A control/cleanup task (stale residue, deprecated names) → route through Council to `ghosthunter`; do not surface it as a top-level peer.
- A lane name or lane-shaped intent ("learn lane", "product feedback", "feature idea", or any `scripts/lane-domains.json` id/alias) → run `lane-start` (above) before doing anything else in that chat.

## Change rules

- Keep this skill a thin front door. It does not duplicate specialist logic; it routes, broadcasts, and sets mode.
- Memory writes follow ADR-004: scoped, private, reversible, audited. Auto-capture (`auto_reviewed`) is consent-gated and Values-owned; never enable ambient privileged write by default.
- Verify the hierarchy against `hierarchy.py` / `roster.py` before changing routing or tool surfaces.

## Validation

```powershell
npm run openai:plugin:validate
npm run agent:governance:validate
```
