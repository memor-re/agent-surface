---
name: council
description: "Use when routing memor.re Council work, running or extending the memor_re/council package, updating agent specs, approvals, goals, evals, dirty-tree release discipline, or deciding which Council specialist owns a product or operating task."
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "council",
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
  "authoritySurface": "docs/architecture/canonical_agent_governance_registry.json#agents.council",
  "journalSurface": "hosted memory ledger (memory_list_recent + memory_search) is the orientation source of truth; docs/operations/* evidence packets and PR/check evidence are the proof surface. Host-app shell directories are never orientation or journal authority.",
  "idempotencyKey": "agent:council:active-slice:authority-surface:validation-command",
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
  "owner": "Visionary"
}
-->


# Council

Use this skill for the repo-local memor.re Council bench. The canonical Python package path is `memor_re/council/`.

Do not treat this file as the source of truth for the Council specialists themselves. It is a compact operating wrapper. Inspect the actual Council docs, runtime, and tests before making claims or edits.

## Required Context

Start with:

- `AGENTS.md`
- `git status --short --branch`
- `memor_re/council/agents/README.md`
- `memor_re/council/agents/implementation_plan.md`
- Relevant files under `memor_re/council/runtime/`, `memor_re/council/evals/`, or `memor_re/council/tests/`

For profile-specific behavior, read `memor_re/council/profiles/<profile-id>/context_manifest.json` first, then load only the listed profile docs needed for the route. Do not copy profile payloads into core agent specs.

## Ownership Routing

Use the smallest capable owner from the discovered Council roster.

Treat the bullets below as the current common-owner quick reference, not the permanent complete set. For the actual roster, inspect `memor_re/council/agents/*.md` or the runtime catalog before adding routing, UI, eval, or product-surface logic. Direct `--agent <name>` calls to any discovered specialist override keyword routing.

- Visionary: product-slice strategy, role or career strategy, operating models, positioning, large decisions.
- Closer: sequencing, dirty-tree cleanup, handoff, release readiness, admin, follow-through, task triage.
- Benefactor: spend, paid infrastructure, budgets, compensation, live Stripe, production migrations, hosted-resource changes.
- Stabilizer: routine, recovery, energy, travel, health-signal clarity, clinician question prep.
- Infra: read-only stack status, provider/tool capability inventory, env-key presence, deployment/auth/evidence packets for the next agent.
- Values: human dignity, privacy, consent, fairness, manipulation risk, public trust, and governance review.

Do not hard-code the six named specialists into new runtime or product surfaces unless the surface is explicitly showing today's canonical board. Use roster or catalog discovery for anything that should grow with Council.

Profile-specific health, career, and writing context belongs under `profiles/<profile-id>/`. Core Council specs should stay reusable and should not canonize the first user's private details.

## Capability Delegation

Council owns the decision and acceptance criteria; it should delegate execution to the smallest authorized capability in the current stack.

- If a tool, agent, MCP server, CLI, plugin, app connector, provider connector, or repo-local validation path is installed, authenticated, provisioned for the target environment, and appropriate for the task, call it without another sysadmin or permission check-in. Do not convert an authorized capability into a user-action blocker just because it is external to the repo.
- Use existing skills before inventing new procedure.
- Use `negotiator` for leverage framing, tactic diagnosis, threshold setting, and counter-draft language when a decision is stuck on negotiation posture rather than ownership.
- Use Superpowers-style multi-agent review when independent implementation, verification, or planning passes would materially improve quality.
- Use Weight Steward for Council weight-learning architecture. Start with structured weight events, deterministic outcome review, and bounded proposed deltas; do not require Ollama/local models for the first learning loop.
- Use provider connectors, CLIs, app connectors, and repo-local validation before manual UI handoff.
- Use Vercel tooling and Vercel skills for deployment, Next.js, Next Forge, and CMS work.
- Use Supabase tooling and skills for database, migration, RLS, logs, advisors, and type-generation work.
- Use Clerk CLI for auth, app linkage, environment, permission, role, and doctor checks.
- Use a local Docker runtime for container-dependent local services and Python for Council runtime/eval work.
- Use Stripe guidance for payment, billing, webhook, and subscription changes.

Treat the specialist roster and capability registry as growing systems, not closed lists. Check the currently available tools, plugins, skills, credentials, and CLIs before deciding a task is blocked. Keep spend, production mutations, hosted-resource changes, destructive cleanup, and secret handling behind explicit approval gates.

## Recursive Failure Review

When Council or `letsgo` is no longer serving the operating model because of repeated recursive failures, false blockers, stale capability assumptions, or instruction conflicts, Council must review its own wrapper before recommending broad codebase changes.

Trigger this review when:

- The same Council, Infra, Closer, Clean, or Letsgo blocker repeats after evidence has already been gathered.
- A Council or Letsgo rule tells the agent to stop, ask, or avoid a tool even though an authorized connector, CLI, plugin, provider connector, or repo-local validation path exists.
- Letsgo is blocked by governance ambiguity, missing capability ownership, or tool-surface confusion rather than a failing test, dirty-tree conflict, or real approval gate.
- The next proposed fix would touch a large product surface only to work around an instruction, routing, plugin, credential, or admin-scope issue.

Review path:

1. Inspect `AGENTS.md`, `git status --short --branch`, `.agents/skills/council/SKILL.md`, `.agents/skills/letsgo/SKILL.md`, and the relevant Council specialist spec before changing product code.
2. Identify the exact sentence, rule, missing capability, credential scope, service state, or admin-only setting causing the block. Cite the file path, line, command, provider response, or log evidence. If the capability is already installed, authenticated, provisioned, and appropriate, record the attempted extra check-in as a false blocker.
3. Classify the blocker as one of: instruction contradiction, stale capability assumption, missing admin or credential scope, real approval gate, validation failure, dirty-tree conflict, or product-code defect.
4. For instruction contradictions or stale capability assumptions, prefer a narrow Council or skill-rule fix over a broad product refactor.
5. For missing admin-only settings, provider scopes, runner/service permissions, GitHub Projects permissions, production credentials, or dashboard-only controls, create a sysadmin escalation issue instead of retrying or editing unrelated code.

Sysadmin escalation issues must be specific and bounded:

- `title`: one concrete unblock, not a general complaint.
- `symptom`: the observed failure or Letsgo stop condition.
- `evidence`: command output, provider response, file/line, or log path.
- `gate`: `gate:sysadmin` only when admin/user custody is required; otherwise use `gate:infra`, `gate:closer`, `gate:benefactor`, or `gate:none`.
- `owner`: Infra plus user for sysadmin gates; Benefactor remains the gate owner for spend, paid infrastructure, live Stripe, hosted-resource mutation, production mutation, destructive cleanup, and secret handling.
- `smallest_unblock`: the least invasive admin, credential, plugin, service, or instruction change needed.
- `validation_after_unblock`: the exact command, GitHub check, browser smoke, or runtime dry run that proves the block is cleared.

## Runtime Commands

Prefer dry-run routing before live execution:

```powershell
python -m memor_re.council.runtime --no-persist --json "Pressure-test this decision."
python -m memor_re.council.runtime --no-persist --agent visionary "Explore this before finance constrains it."
python -m memor_re.council.runtime --no-persist --council --agent visionary "Use the full Council, weighted toward Visionary."
python -m memor_re.council.runtime --no-persist --council --weight-preset benefactor-led "Use the full Council, weighted toward Benefactor."
python -m memor_re.council.runtime --no-persist --agent infra "Pull a stack status check and print it for the next agent."
python -m memor_re.council.evals.run_local
python -m unittest memor_re.council.tests.test_platform_runtime
```

Use `--execute` only when `OPENAI_API_KEY` is configured and the user wants a live generated answer. Use `--no-persist` for review, routing checks, or validation that should not write work logs or approvals.

Direct agent calls should be honored before keyword routing. Use `--agent <name>` for one specialist, repeat `--agent` to add supporting specialists, and use `--council` when the user asks for a full bench of analysis. Full Council calls use the profile's configured standard weight preset, not even weights. Combine `--council` with `--agent <name>` when that agent's value should drive the session. Use `--weight-preset <name>` for platform/profile presets such as `visionary-led` or `benefactor-led`. Use `--weight agent=value` only when the user asks for a specific weighting or the route needs a documented non-default emphasis.

## Change Rules

- Keep core specs reusable; move user-specific material into `profiles/<profile-id>/`.
- Keep approvals explicit for external sends, calendar changes, spend, medical/legal claims, live Stripe, hosted-resource mutations, and production migrations.
- For dirty-tree or release work, use Closer discipline: inspect status, preserve unrelated user changes, stage explicit files only, and validate before committing.
- For dependency installs, use the existing repo workflow and apply the dependency gate from `letsgo` when release work is involved.
- Keep conversation lanes explicit:
  - Main/planning conversations start from fresh `main` evidence, pull backlog, issues, projects, discussions, PRDs, and decide where work belongs. They may create plans, route issues, update project structure, or draft PRDs. They should not implement product code.
  - Branch/implementation conversations each own one feature/fix branch or worktree and execute one scoped slice with its own validation path.
  - Docs/planning edits should use a small planning branch or GitHub issues/discussions, not direct edits on `main`, unless the action is truly read-only planning.
  - Never switch a shared checkout to `main` while another agent is actively working in a branch in that same folder. Use a separate worktree or separate Codex thread/workspace instead.
- If a conversation is going off-lane, route to Closer. Closer should correct the lane and migrate the work to the right GitHub location, such as an issue, Discussion, Project lane, PRD brief, branch PR, or handoff, using the GitHub app connector first when it covers the operation and `gh`/REST/GraphQL for gaps.

## Reliability Cluster Rule

When two or more Codex, plugin, MCP, local-tooling, or agent-surface symptoms appear in one work session, Council must run an Infra-led cluster review instead of letting each symptom close independently. Examples include red `systemError` threads, mobile remote-start errors, missing agents/skills, inline diff preview failures, Markdown/source viewer ambiguity, MCP tool-list waiting, plugin warnings, or user trust language such as "root cause of everything" or "how did we miss this." Infra owns correlation evidence; Closer owns sequencing and handoff timing; Stabilizer owns cognitive-load containment and next check-in; Values owns hidden-state/privacy/agency concerns.

If Learn produces a reliability-cluster packet, Council must discuss the owner split before cleanup or closure: Visionary holds product/system intent, Infra holds tool and backend evidence, Values holds hidden-state and trust language, Stabilizer holds user-load containment, Ghosthunter holds residue classification, and Closer holds release sequencing. The Council packet should state which surfaces were proved, which remain unproved, and which owner receives the next action.

## Validation

For Council-only changes, run:

```powershell
python -m unittest memor_re.council.tests.test_platform_runtime
python -m memor_re.council.evals.run_local
git diff --check
```

Run web lint/build only when `apps/web` files, package manifests, routing, deployment config, or shared TypeScript surfaces changed.
