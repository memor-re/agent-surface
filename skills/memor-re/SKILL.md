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
  "authoritySurface": "docs/operations/canonical_agent_governance_registry.json#agents.memor-re",
  "journalSurface": ".codex/state/current.json + .codex/CLOSE_SUMMARY.md + docs/operations/* evidence packets + PR/check evidence",
  "idempotencyKey": "agent:memor-re:active-slice:authority-surface:validation-command",
  "resumeProof": [
    "npm run agent:governance:validate",
    "npm run close:validate"
  ],
  "sideEffectPolicy": "No external, hosted, paid, secret, destructive, or user-level mutation without approval, backup, rollback or compensation, and post-action proof.",
  "longWaitPolicy": "Represent approvals, timers, hosted refreshes, blocked actions, and human handoffs as explicit lifecycle held actions with safe next actions.",
  "rollbackPath": "Use the cited validation command plus the owning evidence packet to restore, compensate, or classify the blocker as external-held.",
  "validationCommands": [
    "npm run agent:governance:validate",
    "npm run openai:plugin:validate"
  ],
  "scaffoldLearnState": "Act, Notice, Check, Repair, Align are required before learned, fixed, clean, ready, shipped, or durable claims.",
  "owner": "Visionary"
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

Canonical specialists own decisions; their domain teams (helpers, skills, tools) are subordinate, never Council peers (see `docs/superpowers/plans/2026-06-03-council-hierarchy-routing.md`). Control agents like `ghosthunter` are reached *through* Council, never as peers of it.

## Session boot (required — every fresh chat, every surface)

Before routing or acting, orient this session from hosted memory. This applies whether the surface has AGENTS.md or not:

1. Call `workspace.status` — check `write_scope.status`. If `"read_only"`, surface the `write_scope.action` message to the user before they try to save anything.
2. Call `memory.list_recent` (limit: 20) — returns the user's active memory ledger.
3. Call `memory.search` with terms matching the current task or conversation — surfaces relevant prior context.
4. On Clerk OAuth surfaces, omit `installation_id` — the server resolves it. On legacy token surfaces, supply it from `.claude/session-orient.json` or `~/.claude/memor-re-state.json`.

These three calls are your orientation packet. Never route, plan, or implement before running them — a cold start wastes the user's continuity and defeats the product. A SCOPE_DENIED on memory.save is a product failure, not an error — proactively tell the user at session start.

## What `/memor-re` does

1. **Route** — hand a request to Council, which owns specialist routing to the board. Prefer this over guessing an owner. `python -m memor_re.council.runtime --no-persist --json "<prompt>"`.
2. **Broadcast a learning** — "teach every agent X": write it to memory and thread it into each board owner's context so the whole canon carries it forward, not one chat.
3. **Memory mode** — set or show the active capture mode: `manual` (save on ask), `suggested` (propose for review), or `auto_reviewed` (remember-everything: milestones + trend rollups with review and undo). The lens is `council` (full bench) or a single board owner (e.g. `visionary` captures product/strategy milestones, `values` flags consent). See the memory design at `docs/superpowers/specs/2026-06-25-memor-re-canonical-plugin-surface-design.md`.

## Routing rule

- One decision, one owner → defer to `council` (or `/council`).
- Whole-canon learning, memory-mode change, or "act as memor.re across the stack" → handle here, then delegate execution to the smallest capable owner.
- A control/cleanup task (stale residue, deprecated names) → route through Council to `ghosthunter`; do not surface it as a top-level peer.

## Change rules

- Keep this skill a thin front door. It does not duplicate specialist logic; it routes, broadcasts, and sets mode.
- Memory writes follow ADR-004: scoped, private, reversible, audited. Auto-capture (`auto_reviewed`) is consent-gated and Values-owned; never enable ambient privileged write by default.
- Verify the hierarchy against `hierarchy.py` / `roster.py` before changing routing or tool surfaces.

## Validation

```powershell
npm run openai:plugin:validate
npm run agent:governance:validate
```
