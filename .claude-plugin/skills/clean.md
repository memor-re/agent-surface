---
name: clean
description: "Use when a memor.re/platform worktree or branch is dirty and needs a release sweep: classify local changes, identify safe cross-merge candidates, preserve user work, validate, update handoff, and create a focused ready-to-push commit without pushing."
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "clean",
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
  "authoritySurface": "docs/architecture/canonical_agent_governance_registry.json#agents.clean",
  "journalSurface": "hosted memory ledger (memory_list_recent + memory_search) is the orientation source of truth; docs/operations/* evidence packets and PR/check evidence are the proof surface. Host-app shell directories are never orientation or journal authority.",
  "idempotencyKey": "agent:clean:active-slice:authority-surface:validation-command",
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
    "npm run git:source-truth:audit:prs"
  ],
  "scaffoldLearnState": "Act, Notice, Check, Repair, Align are required before learned, fixed, clean, ready, shipped, or durable claims.",
  "owner": "Clean"
}
-->


# Clean

Use this skill after `$handoff` has prepared context, or whenever a dirty tree must become shippable without losing useful work. The job is to turn ambiguous local or branch state into one of four outcomes: ship, split, park, or deliberately ignore.

This is a memor.re platform skill source. It belongs under `.agents/skills/clean/`, should be bundled through `plugins/memor-re`, and should be imported through the memor.re product skill pipeline when it needs to appear in cloud/user skill surfaces. Do not rely on a personal/global Codex-only copy as the source of truth.

This is not a Council specialist canon file. Use Council ownership when a product decision is unclear, Closer when sequencing is the main problem, and Benefactor when push, hosted-resource, paid-infrastructure, production, destructive-cleanup, or spend risk needs explicit review.

## Boundaries

- Never run `git add .`; stage explicit files only.
- Never use `git reset --hard`, `git checkout --`, force-push, delete branches/worktrees, remove files, or overwrite user changes unless the user explicitly requested that exact operation.
- Do not push, deploy, publish `/skills`, mutate production Supabase/Clerk/Vercel/DNS/Stripe, change paid infrastructure, expose secrets, or perform hosted/destructive cleanup unless the current instruction explicitly approves the target action and environment.
- Keep private corpus data, raw artifacts, service-role keys, OAuth tokens, prototype secrets, and production credentials out of commits, skills, docs, and chat.
- Do not try to control Codex/OpenAI context compaction. Treat model context management as platform-owned; preserve cleanup state through source-linked handoffs, `.codex/state/current.json`, `.codex/CLOSE_SUMMARY.md`, and focused evidence packets.
- When Clean is called directly, Clean owns dirty-tree classification while activating logical dependencies automatically. Pull Closer for release sequencing, Infra for provider/tool evidence, Benefactor for hosted-resource or spend risk, Values for secret/privacy risk, Stabilizer for repeated-failure cognitive load, Ghosthunter for stale active-doc residue, Learn for durable lessons, Letsgo for release movement, and Superpowers for branch-finishing discipline when implicated.
- For local app cleanup, prefer uninstallers, official vendor cleanup tools, and exact registry/process evidence before folder deletion. Manual folder or registry deletion is a last-mile cleanup step for named targets after backup, admin/user custody when needed, and post-action validation.
- If unrelated dirty changes touch the same files as the current slice, read them carefully and work with them. Stop only when preserving them makes the cleanup impossible.

## Workflow

1. Orient:
   - Read `AGENTS.md`, `.codex/CLOSE_SUMMARY.md`, and any route/package instructions that govern the changed files.
   - Run `git status --short --branch`, inspect recent commit style, and check `git diff --name-status`, `git diff --stat`, relevant `git diff -- <path>`, and untracked files.
   - Run `npm run git:source-truth:audit` before broad branch cleanup. Treat its branch/worktree/close-state report as the live inventory; older dirty-branch packets are provenance only.
   - Discover the current callable tool, plugin, CLI, browser, and skill surface before claiming the cleanup is blocked.

2. Classify every dirty item:
   - Current slice: belongs in the commit.
   - User or other-agent slice: preserve and leave unstaged unless explicitly included.
   - Generated artifact: keep only when the repo workflow expects it.
   - Handoff/doc update: include when it records the true state of this cleanup.
   - Cross-merge candidate: useful work from another branch/ref that can be ported cleanly.
   - Local system residue: uninstall leftovers, broken icons, stale local automation, or vendor databases; classify with process, registry, service, task, and official-vendor-tool evidence before deleting.
   - Unsafe or unrelated: park, reject, or ask only when no safe assumption exists.

3. Evaluate cross-merge candidates before porting:
   - Use evidence such as `git branch -vv`, `git merge-base`, `git cherry -v main <branch>`, `git diff --name-status main...<branch>`, `git diff --stat main...<branch>`, and focused `git show --stat <sha>`.
   - Prefer small patch extraction, selective `git cherry-pick -n <sha>`, or manual porting over wholesale merges when the source branch carries stale docs, retired names, broad package changes, migrations, or UI churn.
   - Do not delete or mark a branch closed until the useful delta is committed, proven ancestor/patch-equivalent, intentionally superseded, or explicitly rejected with handoff evidence.
   - When a stale branch is causing repeated chat, checkout, or local-service failures, run extract-before-park: list every patch-unique commit, classify each as `accepted`, `already-on-main`, `superseded`, `unsafe`, or `rejected`, and preserve that ledger in the commit/PR/handoff before branch deprecation.
   - After a PR is merged or a branch's work is otherwise accepted into `origin/main`, run a post-merge residue check before final closeout: fetch/prune remotes, prove the merge commit or patch-equivalent tree is on `origin/main`, inspect active branch/worktree state, verify no local-only diff remains, delete or de-authorize the stale local branch only after proof, and write or reference the ScaffoldLearn memory for the cleanup action.
   - Before trusting a worktree's `.codex` state, treat `close-state ok` from `npm run git:source-truth:audit` as inventory only. Run `npm run close:sync` followed by `npm run close:validate`, or record an explicit `close:validate` confirmation, in the intended worktree before trusting the `.codex` state.
   - If the user pushes back on parking, stop using "park" as the default outcome. Pull out what can safely deploy, cite the main commit or merged PR that already covers anything superseded, and leave only truly unsafe or ambiguous work parked.

4. Decide the cleanup path:
   - Ship: validate and commit the focused slice.
   - Split: separate unrelated file groups into separate commits or leave them for another owner.
   - Park: leave incomplete or ambiguous work unstaged with a handoff note.
   - Ignore: leave known irrelevant noise untouched and state why.

5. Validate:
   - Run the smallest required checks for the changed area.
   - For `apps/web`, run `npm --prefix apps/web run lint`; add `npm --prefix apps/web run build` for routing, framework, deployment, build behavior, or shared TypeScript changes.
   - For skills, run focused `skills:import --dry-run`, `npm --prefix packages/agent-skills test`, plugin build/validate when bundled, and `git diff --check`.
   - When an isolated worktree is missing validation dependencies such as `tsx` or `js-yaml`, treat it as a worktree bootstrap gap, not proof that a CLI, skill, or repo path is unavailable. Run `npm ci --ignore-scripts --no-audit --no-fund` in that isolated worktree, rerun the intended checks, and keep generated or no-content index noise out of the commit.
   - If dependency installation or tooling refresh marks an unrelated tracked path as modified, inspect the diff before deciding. If there is no content diff or the change is only line-ending/index refresh noise, restore that path and state the actor chain plainly: the agent ran the install, Git reported the path, the diff proved no real content change, and Clean restored it.
   - Add Supabase/RLS validation before permission-sensitive or multi-user data changes.

6. Prepare the commit:
   - Update `.codex/CLOSE_SUMMARY.md` when the cleanup changes project state, branch status, blockers, validations, or next steps.
   - Stage explicit files only.
   - Commit with the repo convention: `<type>(<scope>): <imperative summary>`.
   - Keep push as the next gate unless the user explicitly asked to push or invoked a shipping path that authorizes it.

## Final Report

Report the cleanup outcome in practical release terms:

- Branch, commit hash, and commit message when committed.
- Dirty files left unstaged and why.
- Cross-merge candidates accepted, rejected, parked, or still needing review.
- Validation commands that actually ran.
- Whether the tree is ready for `$letsgo`, push, PR, deploy, production import, or another explicit approval gate.
