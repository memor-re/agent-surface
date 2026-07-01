---
name: looper
description: "Autonomous waterfall session driver. Takes topic variables [topic] and runs sustained sweeps across all surfaces (env, agents, tools, connectors, plugins, skills, api, cli, mcp, container profiles, terminals, local and hosted). 10-min intervals, minimum 5-hour runtime. Replaces /agent-development, /skill-development, /run-skill-generator, /skill-creator as topic-parameterized instances of one universal pattern. Use when the user wants sustained autonomous infra/security/durability/feature work with plain-language blocker prompts surfaced to Dontae."
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "looper",
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
  "authoritySurface": "docs/architecture/canonical_agent_governance_registry.json#agents.looper",
  "journalSurface": "hosted memor.re ledger (memory_list_recent + memory_search) is the orientation source of truth. Local MEMORY.md is never an orientation source.",
  "idempotencyKey": "agent:looper:active-slice:topic:surface:tick",
  "resumeProof": [
    "npm run agent:governance:validate",
    "npm run agent:lifecycle:preflight"
  ],
  "sideEffectPolicy": "No external, hosted, paid, secret, destructive, or user-level mutation without approval, backup, rollback, and post-action proof.",
  "longWaitPolicy": "Surface blocker as a plain one-sentence writing prompt to the user. Continue to next layer. Never stop on a single blocker.",
  "rollbackPath": "Use the cited validation command plus the owning evidence packet to restore, compensate, or classify as external-held.",
  "validationCommands": [
    "npm run agent:governance:validate",
    "npm run agent:lifecycle:preflight"
  ],
  "scaffoldLearnState": "Act, Notice, Check, Repair, Align are required before learned, fixed, clean, ready, shipped, or durable claims.",
  "owner": "Infra"
}
-->

# Looper

The canonical autonomous waterfall agent. One skill, topic-parameterized. Replaces fragmented /agent-development, /skill-development, /run-skill-generator, /skill-creator instances — those are all looper with `[topic]` set.

The `[ ]` notation marks variable slots. Fill them at invocation time.

```
/looper [infra] [security] [durability]
/looper [skill-development] [plugin]
/looper [agent-development] [independence]
```

This is how un+ will code with memor.re: topic-driven autonomous sessions that sweep every surface, never re-invent, always orient from hosted memor.re first.

## Session boot (every tick — not optional)

Before any sweep work:

1. `mcp__memor-re-hosted__workspace_status` — verify connected, check write scope.
2. `mcp__memor-re-hosted__memory_list_recent` — load active ledger (last 10 entries).
3. `mcp__memor-re-hosted__memory_search` query `"session handoff active blockers [topic]"` — recall prior context.
4. `gh pr list --limit 10` + `gh issue list --label state:ready --limit 10` — get live GitHub state.

**Never read local MEMORY.md. Never write local memory files. Hosted memor.re is the only memory canon.**

## Waterfall pattern (every tick)

Read → Research → Collate → Compare → Compile → Act → Blocker-surface → Continue.

Each tick sweeps the declared layers in order. Skip layers not relevant to the declared topic. Report delta only — not everything you checked, only what changed.

### Layer template (customize per topic)

```
LAYER [N] — [SURFACE NAME]
- What to check: [specific artifacts, commands, URLs]
- Pass condition: [what green looks like]
- Fail action: file issue OR surface blocker prompt to Dontae
```

### Canonical layers for [infra] [security] [durability]

**LAYER 1 — ENV & CREDENTIALS**
- Check active env vars: `MEMOR_RE_*`, `SUPABASE_*`, `CLERK_*`, `OPENAI_*`, `MIGADU_*`, `PROJECTS_SYNC_TOKEN`
- No secrets in repo, no plaintext in logs, no uncommitted `.env` changes
- Windows: verify HKCU registry for key hygiene

**LAYER 2 — CONNECTORS & MCP**
- `mcp__memor-re-hosted__*` responding? Run `mcp__memor-re-hosted__workspace_status`
- Scan `.mcp.json` for drift vs `docs/contracts/connector-architecture-v1.json`
- Verify no conflation of local-council vs hosted-memory connectors (AGENTS.md rule `two-connector-architecture`)

**LAYER 3 — PLUGINS & SKILLS**
- `npm run plugin:freshness:check` — is the bundle current?
- `npm run plugin:validate` — schema and registry clean?
- Installed across surfaces? (CLI, desktop, Codex) — verify with `claude plugin list` or surface evidence

**LAYER 4 — AGENTS & TOOLS**
- `npm run agent:governance:validate`
- Forge gates green on main? `gh run list --limit 3`
- Open PRs: run `node scripts/check-pr-merge-ready.mjs [PR_NUMBER]` on each open PR

**LAYER 5 — INDEPENDENCE & OWNED MODEL**
- Local Ollama running? `ollama list`
- `npm run forge:sync -- --check` — no forge-sync drift
- Rogue LLM check: forge gate `cost-model: no rogue LLM call sites`

**LAYER 6 — REPO DURABILITY**
- `npm run agent:lifecycle:preflight`
- Recall index: forge gates `recall:index-fresh`, `recall:coverage`, `recall:no-split-brain`
- Stale/DIRTY PRs: check issue #946 auto-rebase state

**LAYER 7 — SECURITY**
- Forge security gates: `fail-open`, `auth-catch-swallows-failure`, `egress-allowlist`, `no-bearer-tokens`
- Supabase RLS on active tables
- Lease store: issue #947 intermittent 401 state

## Canonical layers for [skill-development] [agent-development]

**LAYER 1 — ORIENTATION**
- Read relevant PRDs and ADRs from `docs/planning/` and `docs/decisions/`
- Check existing skills: `Glob plugins/memor-re/skills/*/SKILL.md`
- Run `node scripts/issue-precheck.mjs "[topic]"` — never reinvent

**LAYER 2 — DESIGN**
- Is this an instance of an existing skill (use looper with that topic) or genuinely new?
- If new: file issue → PRD → SKILL.md → register in plugin → build → validate → PR

**LAYER 3 — BUILD**
- Write SKILL.md with the durable-execution-contract block
- Add skill name to the `bundledSkills` array in `scripts/build-memor-re-plugin.mjs`
- Register in `docs/architecture/canonical_agent_governance_registry.json` agents array
- `npm run plugin:build && npm run plugin:validate`

**LAYER 4 — SHIP**
- PR with co-change: skill file + plugin bundle update (co-change gate fires if you forget)
- Post `@coderabbitai resolve` after addressing reviews

## Blocker protocol

When a layer fails:
- Write one plain sentence to Dontae: `"I need [action] on [surface] to [outcome]. Risk: [risk]. Stop: [rollback]."`
- Continue to the next layer immediately.
- Save the blocker to hosted memor.re if it will block continuity.
- Never stop the session on a single blocker.

## Loop settings

- Default interval: 10 minutes (`/loop 10m /looper [topic]`)
- Minimum runtime: 5 hours
- Continue if relevant open issues remain
- Stop if: budget cap hit, user explicitly stops, or all layers are green for 3 consecutive ticks

## Naming (canon)

This is **looper**, not "the local loop." `/loop` is the system scheduler primitive. `/looper` is the memor.re autonomous waterfall agent. The `[ ]` are topic variables, not literal brackets.

When a user says "keep working on [X]" or "run a session on [X]", that's a looper invocation with `[topic]=X`.

## What looper is not

- Not a replacement for `/council` (single decisions → council)
- Not a replacement for `/integrator` (API/source connection design → integrator)
- Not a code-quality reviewer (that's `/code-review`)

## Validation

```powershell
npm run plugin:build
npm run plugin:validate
npm run agent:governance:validate
```
