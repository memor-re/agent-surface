---
name: ghosthunter
description: Use when cleaning stale, hallucinated, source-contaminated, or deprecated-name residue from active memor.re docs, handoffs, specs, skills, and planning artifacts while preserving audit evidence.
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "ghosthunter",
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
  "authoritySurface": "docs/operations/canonical_agent_governance_registry.json#agents.ghosthunter",
  "journalSurface": ".codex/state/current.json + .codex/CLOSE_SUMMARY.md + docs/operations/* evidence packets + PR/check evidence",
  "idempotencyKey": "agent:ghosthunter:active-slice:authority-surface:validation-command",
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
    "npm run codex:residue:graph"
  ],
  "scaffoldLearnState": "Act, Notice, Check, Repair, Align are required before learned, fixed, clean, ready, shipped, or durable claims.",
  "owner": "Ghosthunter"
}
-->


# Ghosthunter

Use this skill to turn rough planning, transition, and handoff material into current memor.re architecture language.

The rule is simple: active docs should state the clean current truth. Protected archives and provenance manifests can preserve exact historical evidence when the literal source identity is needed for audit, regeneration, or rollback.

Ghosthunter's first question is not just "does this reference exist?" It is "does this reference still have authority?" Classify stale or inherited guidance as active rule, guardrail, historical provenance, archive, or delete-candidate before rewriting or deleting it.

When Ghosthunter is called directly, Ghosthunter owns stale active-doc residue while activating logical dependencies automatically. Pull Visionary for current product language, Infra for provider/tool truth, Closer/Clean for sequencing and commit readiness, Benefactor for resource-sprawl labels, Values for trust/agency wording, Stabilizer for cognitive-load reduction, Learn for durable lessons, and Superpowers for structured review when implicated.

## What To Remove From Active Docs

Treat a phrase, object, section, or file reference as residue when it is:

- A deprecated source name repeated outside an archive, provenance manifest, or recovery diff.
- A rejected or superseded concept preserved only because a prior artifact used it.
- A negative decision table that requires the old source to understand.
- A stale handoff claim that is no longer the current operating state.
- A model-invented object with no clear workflow, permission, UI surface, data lifecycle, or product capability.

## What To Preserve

Keep content when it supports:

- Confirmed architecture.
- MVP implementation requirements.
- Roadmap architecture.
- Real open questions or future decisions.
- Recovery paths to protected archives, provenance manifests, git history, or external systems.

If exact historical labels are required, keep them only inside the archive/provenance layer and point active docs to that layer with neutral language.

## Retired Local Infrastructure Workarounds

When a local infrastructure workaround is retired during crisis cleanup, remove active invocation and remediation surfaces instead of trying to make the event unknowable. Replace active docs with neutral guardrail language, keep only current official paths as options, and leave exact provenance to git history or a protected archive. Residual risk remains: historical records may still contain retired vocabulary, so never promise disappearance.

For plugin, MCP, local-tooling, mobile, or session-continuity incidents, old plugin names and cache paths are not active authority by themselves. Classify them as active only when current config, cache, process, state table, or thread evidence points to live use. Otherwise preserve them as historical provenance and write the active doc around the current official channel, plugin id, or thread id.

## Codex Continuity Residue

When a Codex session, project, plugin, MCP, or local-tooling/mobile continuity repair is in scope, Ghosthunter owns residue classification after Learn and Council name the lesson. The cleanup must distinguish:

- active authority: current config, official package state, active thread API records, active session files, mounted tools, or live process paths that still control behavior
- loaded helper: official helper processes, MCP servers, plugin child processes, or runtime shims that are active but subordinate to the app or tool surface
- guardrail: current rules that prevent unsafe cleanup, secret exposure, destructive deletion, or non-official sync paths
- historical provenance: archived sessions, backups, previous plugin ids, old logs, and stale chat artifacts that explain history but no longer route work
- delete-candidate: exact residue with no active authority, no needed provenance role, and a reversible cleanup path

For cross-surface session parity, the active session tree must not keep files that the local state store marks archived when the repair is being closed. Move only exact archived rollout files out of active paths, keep timestamped backups, and report the ids. Never generalize from one stale file to broad deletion.

For process trust issues, do not treat an iconless process, lowercase executable name, many child processes, or a stubborn Task Manager termination dialog as residue by itself. First classify by executable path, package identity, command line, signer or package signature, hash, parent/child role, and start time. Preserve official helpers as loaded helpers and send unresolved or mismatched processes to Infra or explicit user/admin custody instead of deleting.

## Workflow

1. Inspect the relevant files first.
2. Search active docs for deprecated names, stale state, "previous", "revised", "rejected", "do not create", and duplicate concept names.
3. Classify each reference by authority: active rule, guardrail, historical provenance, archive, or delete-candidate.
4. Identify the durable capability behind each term.
5. Rewrite active docs in positive product language.
6. Move long historical detail into archive or provenance files when it must remain recoverable.
7. Verify with targeted searches that active docs no longer carry the residue.

## Preferred Language

Use:

- "retired predecessor"
- "deprecated prototype corpus"
- "protected archive"
- "provenance evidence"
- "current Council"
- "confirmed architecture"

Avoid using literal retired source names in active docs unless the exact label is required for a command, manifest entry, or archive lookup.
