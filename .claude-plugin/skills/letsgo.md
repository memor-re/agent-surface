---
name: letsgo
description: Ship agreed memor.re/platform work end-to-end. Use when the user says phrases like "letsgo", "ship it", "commit push deploy", "migrate to production", "I agree, push it live", "make the dev changes production", or asks Codex to refresh context, commit, push, run migrations, deploy, and verify production using the repository's established release path.
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "letsgo",
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
  "authoritySurface": "docs/operations/canonical_agent_governance_registry.json#agents.letsgo",
  "journalSurface": ".codex/state/current.json + .codex/CLOSE_SUMMARY.md + docs/operations/* evidence packets + PR/check evidence",
  "idempotencyKey": "agent:letsgo:active-slice:authority-surface:validation-command",
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
    "npm run release:preflight-pr -- <pr-number>",
    "npm run agent:lifecycle:closeout -- --pr <pr-number> --authority-map <path>"
  ],
  "scaffoldLearnState": "Act, Notice, Check, Repair, Align are required before learned, fixed, clean, ready, shipped, or durable claims.",
  "owner": "Closer"
}
-->


# Letsgo

Use this skill as an execution mode for moving already-agreed work from local/dev state toward release. Treat it as permission to proceed through routine validation, commit, push, PR, deployment-readiness, and verification steps while preserving user changes and stopping for unsafe or ambiguous production actions.

This is a memor.re platform skill source. It belongs under `.agents/skills/letsgo/` and should be imported through the platform skill pipeline when it needs to appear in memor.re product skill surfaces. Do not rely on a personal/global Codex-only copy as the source of truth.

## Operating Rules

- Refresh context before acting: read repository instructions, inspect `git status`, review the relevant diff, check recent commit style, identify the current branch and remote, and discover deployment or migration commands from repo files. On Windows, if the first `git status` or other baseline CLI command fails to resolve in a fresh shell, verify process PATH with a repaired/non-login process or `npm run tools:doctor` before treating the tool as unavailable.
- Preserve user work: never run `git add .`, never stage unrelated files, never reset or revert user changes, and never force-push unless the user explicitly requested that exact operation.
- Preserve no-write boundaries: if Dontae said read-only or do not write in the repo, do not create files, run `apply_patch`, commit, push, or open a PR until a later explicit write approval. Branch creation alone is not write permission. For parallel work, use a separate worktree and prove write-capable tools target that worktree; `apply_patch` is safe only when the active Codex session workspace is the intended checkout.
- Prefer established paths: use repo scripts, release docs, CI/CD config, hosting config, and migration tooling already present in the project. Do not invent a production process when the repo does not define one.
- Do not turn a desired ship date into a release blocker. Use dates as sequencing targets, pacing check-ins, and recheck points; stop only for failed validation, ambiguous targets, explicit hold language, or the protected approval gates named in this skill.
- Treat one Council or canon call as enough to preserve release flow. Keep the invoked owner accountable, activate Infra, Closer, Benefactor, Values, Stabilizer, Design, Clean, Ghosthunter, Learn, or Superpowers when implicated, and continue routine validation/release-readiness work until a real approval gate appears.
- Proceed without extra confirmation for routine local release steps when credentials are already configured, the target is clear, and validation passes or failures are fixed.
- Use any installed, authenticated, provisioned, and task-appropriate tool, agent, MCP server, CLI, plugin, app connector, provider connector, or repo-local validation path needed for validation, PR/check inspection, logs, runner status, preview evidence, local services, or release-readiness work without another sysadmin or permission check-in.
- Fresh worktrees may not have `node_modules`. Before calling validation blocked because `tsx`, `js-yaml`, or another repo dev dependency is missing, bootstrap the isolated worktree with `npm ci --ignore-scripts --no-audit --no-fund` and rerun the intended check. If the install fails, report the install, lockfile, network, or provider blocker instead of treating the tool as unavailable.
- Keep paid infrastructure, production mutations, hosted resource changes, destructive cleanup, secret handling, live Stripe, and production database migrations approval-gated unless the user's current instruction explicitly authorizes that exact action.
- When release movement needs provider authentication, try the phone/mobile approval path first when available. Use GitHub Mobile, push approval, or phone-delivered verification before passkeys, browser-only prompts, fallback tokens, or requiring a device switch; do not request or handle OAuth fallback tokens, secrets, or recovery codes in chat.
- Before pushing or merging learning from a local system cleanup cluster, verify the current GitHub issue gate, repo base, changed skill/plugin copies, local validation, and held actions. Do not close parent epics or GitHub print sinks just because the learning patch shipped.
- For memor.re skill updates, verify the full projection path before release movement: canonical `.agents/skills` source, generated `plugins/memor-re` bundle, installed Codex plugin cache or marketplace revision, and any local `config.toml` revision pin that affects what future chats load. Rebuild and validate the plugin bundle, refresh stale projections when safe, or report the exact stale surface and reload gate.
- Codex Review and branch-creation validation must originate from a platform repo workspace or platform repo worktree, never from `$CODEX_HOME` as a saved Codex project. Keep the home folder intact; deprecate only its project/workspace target authority and verify branch creation with `git rev-parse --show-toplevel`, `git status --short --branch`, and PR branch/remote evidence from the platform repo.
- Before pushing or opening a PR from a non-main branch, run `npm run release:assert-clean-pr-base -- -Fetch` when the script is available. If it reports that the branch is behind `origin/main`, based on an old merge base, or carrying unrelated committed work, stop using that branch as the PR source. Create a clean worktree from current `origin/main` and cherry-pick or manually port only the focused commit(s).
- Before creating that clean worktree, refresh or verify `origin/main`; `git worktree add ... origin/main` uses the local cached remote-tracking ref and does not fetch GitHub automatically.
- When the release request names a prior PR, branch, script, or guardrail, verify whether current `origin/main` already contains that work before reimplementing it. Use human-readable PR titles plus merge commits, `git branch -r --contains`, `git log`, or `gh pr view` evidence, then only implement the missing delta.
- When CodeRabbit is part of the requested release path, read the latest review state, body, and actionable comments before merge. Do not treat a successful status context as release approval when the review is `COMMENTED` or unresolved actionable comments remain.
- After a PR merge, successful push to `main`, or accepted release commit, do not close while the active workspace still sits on the merged feature branch. Run Clean's post-merge residue check, prove the work is on `origin/main`, remove or de-authorize stale local branch authority when safe, and create or reference the ScaffoldLearn action-memory for the repair. If local `main` is checked out in another worktree, a detached current `origin/main` state is acceptable for this workspace as long as the next branch starts from current main code.
- For Vercel production deploys, use a clean worktree based on `origin/main` and verify `.vercel/project.json` before `vercel deploy --prod`. The expected project is `memor-re-platform` under scope `memor-re`, with project id `prj_0b8Bd110c20zT8tgIfhs9DUEEJB7` and org id `team_ZqBMJCVBKyPx91UI9ccHfiKZ`. Run `vercel project inspect memor-re-platform --scope memor-re` and confirm `apps/web`, `npm run build`, `npm install`, and Node.js `24.x`. If the deploy worktree is unlinked, do not continue; Vercel can create a scratch project named after the worktree and waste resources.
- Ask a concise question or stop with a clear blocker if the production target is unknown, credentials are missing, checks fail without a clear fix, unrelated dirty changes conflict with the release, or a migration/destructive operation is irreversible and not clearly covered by repo workflow.
- If release movement is blocked by repeated Council/Letsgo instruction recursion, stale capability assumptions, or a tool-surface contradiction, do not broaden the product-code change. Route through `$council` recursive failure review, extract one bounded issue per blocker, and use `gate:sysadmin` only for admin-only settings, provider scopes, credentials, services, or dashboard controls that Infra plus the user must unblock.
- For OpenAI API usage grant work, use a clean branch or worktree from current `origin/main`; do not ship from a shared dirty checkout. Existing-key API usage inside an approved cap can be validated, committed, pushed, and PR'd. Stop for buying credits, enabling auto-recharge or Codex auto top-up, raising caps, creating or rotating keys, production env changes, hosted mutations, or secret exposure. Codex/ChatGPT usage-limit pressure must be verified separately and is not an API burst-spend approval.

## Workflow

1. Orient:
   - Read local instructions such as `AGENTS.md`, `CLAUDE.md`, `README.md`, release docs, deployment docs, and relevant package or tool config.
   - Inspect `git status --short --branch`, `git diff`, and recent commits.
   - Identify changed areas and decide what validation is required.

2. Validate and refresh:
   - Run the repo's required checks for the changed areas.
   - Refresh dependencies, generated clients, schemas, lockfiles, or build artifacts only when the repo workflow requires it.
   - For database or production migrations, run status/dry-run/introspection commands first when available, then apply through the documented production path only after the approval gate is satisfied.

3. Commit:
   - Stage explicit files in logical groups.
   - Commit with the repository's commit convention. If none exists, use a concise Conventional Commit.
   - Keep unrelated work unstaged and mention it if it affects release confidence.

4. Push:
   - Push the current branch to the correct remote/upstream.
   - If the repo's production path is PR-based, open or update the PR and make clear whether merge/deploy is still pending.
   - If production deploys on push to a protected branch, follow the repo's documented merge or release flow rather than bypassing protections.

5. Deploy or migrate:
   - Use only the repository's documented, approval-gated release path; never invent or run a deploy command the repo does not define. Direct provider CLIs (Vercel, Supabase) are gated provider operations governed by the Vercel/Supabase rules below, not repo-owned `npm run`/`scripts/` actions, so route them through those named gates.
   - For Vercel CLI production deploys, first prove the worktree is linked to `memor-re-platform`; do not rely on interactive auto-link prompts or a temporary directory name.
   - Capture deployment output, release IDs, migration names, production URLs, or dashboard links.
   - If the deployment command asks for interactive confirmation, answer only when the prompt clearly matches the intended safe production action.

6. Verify:
   - Run smoke checks appropriate to the app: health endpoints, production URL load, targeted tests, CLI status, logs, or browser checks for UI changes.
   - If verification fails, diagnose and fix when reasonable, then repeat validation, commit amend/new commit as appropriate, push, redeploy, and verify again.

## Recursive Or Instruction Blockers

If Letsgo cannot move because the governing instructions are fighting the available tool surface, stop the release movement without treating the product code as guilty.

Escalate through `$council` recursive failure review when:

- A Council or Letsgo rule creates a repeated stop/ask/retry loop after evidence exists.
- A rule blocks use of an authorized GitHub connector, `gh` CLI, plugin, provider connector, or repo-local validation command.
- A missing admin-only scope, credential, service, GitHub Projects permission, runner permission, or dashboard setting is the real blocker.
- The proposed workaround would touch broad product code to avoid a narrow instruction or operating-stack issue.

Report these as bounded issues with `title`, `symptom`, `evidence`, `gate`, `owner`, `smallest_unblock`, and `validation_after_unblock`. Use `gate:sysadmin` only when Infra plus the user must provide admin custody; keep Benefactor as the owner for spend, paid infrastructure, production mutations, hosted-resource mutations, destructive cleanup, live Stripe, and secret-handling gates.

## Final Report

Report only what happened:

- Commit hash and message.
- Branch and pushed remote.
- Validation commands and results.
- Migration/deployment commands and production target.
- Production URL, release ID, or status evidence.
- Any skipped checks, blockers, or manual follow-up still required.
