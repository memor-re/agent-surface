---
name: gtm
description: "Use for any memor.re go-to-market work: launch positioning, messaging and launch copy, launch readiness, channel and AEO strategy, pricing narrative, or competitive framing against the AI-memory category. Invoke whenever the user mentions GTM, go-to-market, launch, positioning, messaging, the 7/1 launch, or how memor.re wins, even if they do not say 'gtm'. Routes to the Visionary owner and runs the encoded GTM gates (positioning-check, message-lint, readiness) so launches are validated against the playbook, not vibes."
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "gtm",
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
  "authoritySurface": "docs/operations/canonical_agent_governance_registry.json#agents.gtm",
  "journalSurface": "hosted memory ledger (memory_list_recent + memory_search) is the orientation source of truth; docs/operations/* evidence packets and PR/check evidence are the proof surface. Host-app shell directories are never orientation or journal authority.",
  "idempotencyKey": "agent:gtm:active-slice:authority-surface:validation-command",
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

# GTM

Use this skill to own memor.re go-to-market work. GTM is a Visionary capability, not a Council board peer: `/gtm` routes the decision to Visionary and runs the encoded gates so a launch is validated against the playbook, never against opinion.

The thinking lives in data and code, not in this file. Read the corpus and strategy, then run the gates.

## What `/gtm` does

1. **Route** go-to-market decisions to Visionary, pulling in Values for consent and Closer for release acceptance when their boundary is touched.
2. **Run the gates** below against the launch artifacts. A launch is ready only when all three pass.
3. **Cite the corpus** instead of inventing tactics. The CMO playbook, the web expansion, and the three rising-altitude learning passes are encoded as data.

## Run the gates

```bash
node scripts/gtm-positioning-check.mjs   # positioning is complete and differentiated from the dev agent-memory category
node scripts/gtm-message-lint.mjs        # launch copy matches the voice rules (no em dash, no slop, no negation framing)
node scripts/gtm-readiness.mjs           # 7/1 go/no-go: messaging, KPI stack, channels, AEO, consent gate, positioning
```

`gtm-readiness.mjs` composes the other two. Exit 0 means GO. Add `--json` for machine-readable output.

## Data surfaces

- `docs/research/gtm-corpus.json` — the playbook, value drivers, KPI stack, channel mix, pricing heuristics, and the web expansion (PLG, AEO, positioning, the AI-memory category, pricing, MCP distribution).
- `docs/research/gtm-learning-passes.json` — three rising-altitude passes (tactical, operating-model, category and moat) with the synthesis of how memor.re wins.
- `docs/strategy/gtm-positioning.json` — the chosen category, anti-positioning guard, and the three moats.
- `docs/strategy/gtm-launch-plan.json` — the launch readiness manifest the readiness gate validates.

## Consent is the product

memor.re is a memory product, so trust is the conversion layer. The readiness gate requires a Values-owned consent gate: scoped, reversible, audited memory (ADR-004), no copy that implies ambient capture without consent. Bring in Values for any public claim about what memor.re remembers.

## Change rules

- Keep this skill a thin front door. The durable logic is the gates and the data, not prose here.
- Change the playbook by editing the corpus data, then let the gates enforce it.
- Add a GTM lesson by appending to `scripts/learn-lessons-registry.json` then running `npm run learn:canon:fix`. Point each lesson at a gate as its durable artifact.
- The positioning frame is load-bearing: if `gtm-positioning-check.mjs` fails because memor.re collapsed into the developer agent-memory category, fix the positioning, not the gate.
