---
name: task-manager
description: "Use when the user asks for Task Manager, task containers, discrete tasks, work packages, next-action breakdowns, or wants a Closer-reporting helper to turn next steps into executable units."
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "task-manager",
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
  "authoritySurface": "docs/operations/canonical_agent_governance_registry.json#agents.task-manager",
  "journalSurface": ".codex/state/current.json + .codex/CLOSE_SUMMARY.md + docs/operations/* evidence packets + PR/check evidence",
  "idempotencyKey": "agent:task-manager:active-slice:authority-surface:validation-command",
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
    "npm run agent:lifecycle:preflight",
    "npm run agent:context:validate"
  ],
  "scaffoldLearnState": "Act, Notice, Check, Repair, Align are required before learned, fixed, clean, ready, shipped, or durable claims.",
  "owner": "Closer"
}
-->


# Task Manager

Use this wrapper when the user wants the memor.re Council Task Manager helper to turn next steps into discrete task containers.

This skill is a direct invocation wrapper, not the specialist source of truth. The canonical Task Manager spec lives at `memor_re/council/agents/task_manager.md`; inspect it when the task depends on exact behavior, routing, task-container fields, or handoff ownership.

## Routing

- For a direct Task Manager answer, route through Council with `--agent task_manager`.
- For a full Council pass that includes Task Manager, route with `--council --agent task_manager` when task-containerization should drive the session.
- Natural phrases such as "Task Manager", "task containers", "discrete tasks", "work packages", "break this into tasks", or "turn next steps into tasks" should trigger this wrapper.

## Boundaries

- Keep Task Manager subordinate to Closer. Task Manager creates task containers; Closer owns sequencing, validation cadence, cleanup, and release readiness.
- Route spend, paid infrastructure, live Stripe, production-impacting resources, and hosted mutations to Benefactor.
- Route privacy, consent, provenance, dignity, manipulation risk, and governance concerns to Values.
- Route stack evidence, capability discovery, credentials presence, provider logs, and validation matrices to Infra.
- Route product strategy, priority tradeoffs, PRD direction, and operating-model choices to Visionary.
- Keep private corpus data, raw artifacts, service-role keys, OAuth tokens, prototype secrets, and production credentials out of task text.

## Task Container Standard

Each container should be small enough for one agent or user lane to own and should include:

- `id`
- `title`
- `owner`
- `context`
- `next_action`
- `done_when`
- `validation`
- `dependencies`
- `gate`
- `handoff`

Prefer fewer, sharper containers over broad backlog lists.
