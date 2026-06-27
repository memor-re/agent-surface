---
name: learn
description: "Use when the user says Learn, invokes $learn, asks whether memor.re learned from a run, or wants evidence converted into the right memor.re learning mode."
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "learn",
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
  "authoritySurface": "docs/operations/canonical_agent_governance_registry.json#agents.learn",
  "journalSurface": ".codex/state/current.json + .codex/CLOSE_SUMMARY.md + docs/operations/* evidence packets + PR/check evidence",
  "idempotencyKey": "agent:learn:active-slice:authority-surface:validation-command",
  "resumeProof": [
    "npm run agent:governance:validate",
    "npm run close:validate",
    "npm run agent:lifecycle:preflight"
  ],
  "sideEffectPolicy": "No external, hosted, paid, secret, destructive, or user-level mutation without approval, backup, rollback or compensation, and post-action proof.",
  "longWaitPolicy": "Represent approvals, timers, hosted refreshes, blocked actions, and human handoffs as explicit lifecycle held actions with safe next actions.",
  "rollbackPath": "Use the cited validation command plus the owning evidence packet to restore, compensate, or classify the blocker as external-held.",
  "validationCommands": [
    "npm run learn:canon:validate",
    "npm run agent:governance:validate",
    "npm run agent:lifecycle:preflight",
    "npm run close:validate"
  ],
  "scaffoldLearnState": "Act, Notice, Check, Repair, Align are required before learned, fixed, clean, ready, shipped, or durable claims.",
  "owner": "Learn"
}
-->


# Learn

Turn a completed or blocked run into reusable operating memory. Learn owns the discrete
lenses — observe, document, measure, recommend — and packages evidence for the Council
owner who owns the durable change. ScaffoldLearn is the canonical multi-agent pass that
applies the lenses together and produces Act / Notice / Check / Repair / Align. It is not
autonomous self-learning and not a Council peer.

Canonical skill source under `.agents/skills/learn/`, bundled through `plugins/memor-re`.
After any change, rebuild the bundle and refresh the installed projection (see Validation).

## Lessons live in the index, not in this file

The reusable operating lessons — what to do when a class of failure recurs — are an
encoded, queryable registry, not prose in this skill. This file is a router; the lessons
are data.

- Index: `scripts/learn-lessons-registry.json` — one structured entry per lesson (rule,
  owner, mode, incident class, and the `durable_artifact` gate/validator/test that enforces
  it, or `prose_only` when no enforcer exists yet).
- Gate: `npm run learn:canon:validate` fails closed if the index is malformed, if a lesson
  claims an enforcer that does not resolve on disk, or if this file re-inlines lessons or
  exceeds its router budget. It reports lesson-durability coverage — the Learn-canon sibling
  of ADR-011's `gate-coverage.mjs`.
- Adding a lesson: append an entry to the index (never a bullet here). If a script/test/CI
  check can enforce it, set `enforcement` + `durable_artifact`; otherwise mark it
  `prose_only` so the debt is measured, not hidden.

Prose is not a gate — this file is the reference instance of its own rule. A lesson that
cannot be found, deduplicated, or enforced is not durable.

## ScaffoldLearn frame

Record the full frame before any "learned / fixed / clean / ready / shipped / durable" claim:

- Act: who acted, what changed, where.
- Notice: what the user, agent, test, provider, or surface observed.
- Check: which authority-bearing surfaces were inspected; which remain unchecked.
- Repair: what artifact, code, gate, UI, CI, runtime, or process changed.
- Align: accepted rule, candidate rule, rejected assumption, owner, validation, next action.

A single lens contributes evidence but cannot replace the frame.

## Learning modes

- `learn:observe` — capture evidence, name the symptom, state a tentative rule; no durable change.
- `learn:document` — convert a repeated failure/blocker/correction into a durable skill, gate, test, runbook, or registry entry.
- `learn:measure` — compare a rule against measured outcomes, usage, cost, or validation results.
- `learn:recommend` — a bounded adaptive proposal with owner, cap, stop rule, and validation.
- `learn:autonomy` — not approved by default; never silently mutate skills, weights, provider settings, budgets, or production.

## Anti-sycophancy gate

Before preserving a lesson or claiming done, name four fields: current evidence, untested
assumption, missing evidence, strongest opposing case. No flattery openers, no reassurance
from proxy evidence, no invented flaws to perform rigor. If the evidence is clean, say so.

## Observer Layer

Before promoting any cognition, source, memory, or edge, record the ledger: observed
evidence · hypothesis · belief/feeling/narrative · reflection · authority classification ·
missing evidence · memory/review state · visibility state · mutation target + rollback path.
A source packet is not accepted memory; an AI summary is not source truth; a feeling is
meaningful context, not a fact.

## Encoded-contract status

The frame, anti-sycophancy gate, Observer Layer, and reliability-cluster procedure are
canonical but not yet code-enforced — tracked as `staged_encodings` in the index with their
target validators. Until shipped they are `prose_only` debt and count against coverage.

## Routing & ownership

Learn packages evidence; the durable change is owned by Infra (stack/tool/provider), Closer
(release/dirty-tree), Values (privacy/trust), Benefactor (spend/production resource), Weight
Steward (outcome weights), Visionary (product/operating model). One Council or canon call
starts dependency flow; the invoked owner pulls in logical dependencies automatically.

## Workflow

1. Capture evidence: symptom, branch/PR, command output, run/deploy id, provider status,
   file path, exact blocker. Separate code failures from workflow/provider/CI failures.
2. Extract the smallest reusable rule. Check the index, `AGENTS.md`, Council specs, and
   tests first. If it existed and was not followed, classify the incident as a rule
   execution failure before calling it net-new.
3. Repair durably: prefer an enforceable change (script, gate, test, registry entry) over
   prose. Append the lesson to `scripts/learn-lessons-registry.json`.
4. Route ownership and record release evidence (`.codex/CLOSE_SUMMARY.md` or the issue).
5. Validate (below).

## Learning packet (compact report)

`scaffoldLearn` (A/N/C/R/A) · `mode` + evidence · `symptom` · `evidence` (command/PR/run/path)
· `lesson` · `existing rule status` (followed/missing/ambiguous/ignored) · `durable change`
· `owner` · `validation` · `residual risk` · `closer handoff` when ready.

## Boundaries

- Never store secrets, OAuth tokens, service-role keys, private corpus data, or raw
  artifacts in learned memory or the index.
- Do not treat agreement, user pressure, confidence, prior success, or skill prose as proof.
- Do not broaden product code to compensate for a narrow workflow/permission/CI failure.
- Keep production mutations, hosted changes, paid infra, destructive cleanup, external
  sends, and secrets approval-gated. "Deploy the lesson" starts the Closer release protocol;
  it is not approval to merge, publish, or mutate hosted resources.
- Preserve user work: never `git add .`, reset, force-push, or delete worktrees while
  creating a learning update.

## Validation

```powershell
npm run learn:canon:validate
npm run learn:canon:test
git diff --check
```

For skill/bundle changes also rebuild and refresh the installed projection:

```powershell
npm run plugin:build
npm run plugin:validate -- --json
```
