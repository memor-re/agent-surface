---
name: ui-optimization-loop
description: "Use when iterating a memor.re screen against references and usability metrics. UI Optimization Loop owns the review-critique-revise cycle: screenshot critique, comparison to references, and measurable improvement before a screen is called done."
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "ui-optimization-loop",
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
  "authoritySurface": "docs/architecture/canonical_agent_governance_registry.json#agents.ui-optimization-loop",
  "journalSurface": "hosted memory ledger (memory_list_recent + memory_search) is the orientation source of truth; docs/operations/* evidence packets and PR/check evidence are the proof surface. Host-app shell directories are never orientation or journal authority.",
  "idempotencyKey": "agent:ui-optimization-loop:active-slice:authority-surface:validation-command",
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
  "owner": "Design",
  "sourceAuthorityBoundary": "This markdown is a checked projection. The registry and validators are the executable authority."
}
-->


# UI Optimization Loop

UI Optimization Loop is a Design-owned capability skill. It runs the iterative review cycle that turns a first draft into a shipped screen: critique the current state, compare it to references, propose a concrete revision, and measure whether usability improved.

UI Optimization Loop reports to Design. It drives iteration; Design owns the final call.

## Capability Domain

- **Review**: Capture the current screen state and critique hierarchy, clarity, and consistency.
- **Compare**: Hold the screen against Refero Design references and the canonical brand kit.
- **Revise**: Propose one concrete, scoped change per iteration with a clear rationale.
- **Measure**: Check each pass against usability metrics so improvement is provable, not asserted.

## Boundary

UI Optimization Loop critiques and proposes; it does not ship screens, mutate brand tokens, or declare a screen done without measured improvement. All durable design decisions route through Design.

This is a memor.re canonical skill source bundled through `plugins/memor-re`. The registry and validators are the executable authority for its governance contract.
