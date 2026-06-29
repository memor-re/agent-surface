---
name: handoff
description: Validate completed work against the repo close summary and return a consistent closeout. Use for this repo when a task, epic, issue, plan, deployment, DNS change, migration, agent build, or docs update is complete or blocked, especially before final user responses across chats/apps.
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "handoff",
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
  "authoritySurface": "docs/operations/canonical_agent_governance_registry.json#agents.handoff",
  "journalSurface": "hosted memory ledger (memory_list_recent + memory_search) is the orientation source of truth; docs/operations/* evidence packets and PR/check evidence are the proof surface. Host-app shell directories are never orientation or journal authority.",
  "idempotencyKey": "agent:handoff:active-slice:authority-surface:validation-command",
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
    "npm run close:validate"
  ],
  "scaffoldLearnState": "Act, Notice, Check, Repair, Align are required before learned, fixed, clean, ready, shipped, or durable claims.",
  "owner": "Closer"
}
-->


# Handoff

Use this skill before the final response for completed or blocked work in this repository.

The goal is continuity across context windows. Every closeout should be evidence-based, readable, and anchored to `.codex/CLOSE_SUMMARY.md` or the canonical close-state router for the active company repo.

## Council Roster Source

For this repo, treat the current memor.re Council specialist roster as dynamic. Discover specialists from `memor_re/council/agents/*.md`, excluding `README.md`, `implementation_plan.md`, `ghost_checker.md`, and `quick_reference.md`. Do not hardcode Visionary, Closer, Benefactor, Stabilizer, and Values as the complete permanent set in handoffs, closeouts, or next-agent instructions. If the roster grows, closeout language should refer to current specialist specs or documented ownership rather than stale fixed lists.

## Live Deploy And Auth Closeout

For memor.re production work, do not collapse repo, app, auth, database, and local Codex skill state into one claim. Verify each layer explicitly before saying work is live.

Use this sequence:

1. Confirm Git source of truth:
   - `git status --short --branch`
   - `git log -1 --oneline origin/main`
   - PR merge state or direct-push commit when relevant
2. Confirm Vercel production state for the shipped commit:
   - `vercel list memor-re-platform --format json --meta githubCommitSha=$(git rev-parse origin/main)`
   - `vercel inspect <deployment-url-or-id>`
   - HEAD or browser checks for `https://memor.re`, `https://www.memor.re`, `https://memor-re-platform.vercel.app`, and important auth routes
3. Confirm Supabase target before production reads or writes:
   - Production host is `lircukkittufkfegvbol.supabase.co`
   - Local `.env.local` may point to development and must not be treated as production
   - If Supabase CLI lacks an access token, use authenticated Supabase MCP and verify with read queries
4. Confirm Clerk target before production auth changes:
   - `clerk auth login`
   - `clerk doctor --json`
   - Verify the CLI is linked to the production instance, not only development
5. Confirm production permissions and approvals:
   - Production Supabase mutations, Clerk production changes, DNS, Stripe/live payments, paid infrastructure, hosted cleanup, and destructive operations remain approval-gated unless the user explicitly approved the target action and environment
6. For skills, verify all three layers separately:
   - Repo-local Codex source: `.agents/skills/<name>/SKILL.md` parses as strict YAML and focused discovery/import dry-run passes
   - Production `/skills`: `agent_skills` row exists and latest `agent_skill_versions` row is published with expected hash/frontmatter/resources
   - Local global Codex sync: `%USERPROFILE%\.memor-re\skill-sync\config.json` and an active production sync device/connection exist before claiming production `/skills` mirrored into `%USERPROFILE%\.codex\skills`

Do not recommend a marketplace for a repo-local `$skill` discovery problem. Marketplace is for distributing plugin bundles; repo-local discovery is governed by `.agents/skills`, strict skill metadata, and the active Codex project/session.

## Required Validation

Before final response:

1. Inspect the current close summary:
   - Prefer `.codex/CLOSE_SUMMARY.md` in the active repo
   - If the active repo points to a canonical company repo, inspect that close-state router too when relevant
2. Inspect actual changed files and repo state:
   - `git status --short --branch`
   - relevant `git diff -- <paths>`
   - relevant created files
3. Confirm the close summary reflects the true current state:
   - completed work
   - blockers
   - next steps
   - validation actually run
   - links or paths the next chat should open
4. Update the close summary when the task changed project state or next steps.
5. Run relevant validation commands. Report only checks that actually ran.
6. Do not claim pushed, deployed, migrated, or updated external resources unless the command/tool succeeded.

## Design Approval Evidence

If the handoff asks Dontae, Visionary, or another reviewer for design approval, it must include image evidence before asking for approval.

Required for design approval handoffs:

- Include labeled PNG evidence as Markdown image embeds when the renderer supports images, or as clean Markdown links when embeds are not reliable.
- Include large- and small-viewport PNGs when responsive behavior, navigation, density, or touch behavior is part of the approval decision.
- Include a labeled PNG contact sheet when the approval compares multiple options.
- Pair each repo-hosted review doc or PNG with `[Phone GitHub app](<github-https-url>)` and `[Review URL](<github-https-url>)` links. Use the canonical GitHub HTTPS URL for both; it opens the GitHub mobile app when the phone is configured for universal links and remains the web review URL.
- Do not ask for design approval from text-only descriptions when a rendered PNG can be produced. If images cannot be produced, mark the design approval request blocked and explain the missing artifact.

## Final Output Contract

Start with the result.

Use this section order when applicable:

```text
Done: <short outcome>.

Completed:
- <concrete completed item>
- <concrete completed item>

Validated:
- `<command>` passed
- <smoke or external check> passed

Blocked:
<only include if blocked; one plain-language blocker>

Needs user action:
<only include if the user must do something manually>

Approval evidence:
<only include when design approval is required; must include PNG image/link evidence plus Phone GitHub app and Review URL links>

Next steps:
1. <next action>
2. <next action>
3. <next action>

Close summary:
<live GitHub HTTPS CLOSE_SUMMARY.md next-steps URL as a visible bare URL>
```

If work was pushed, use:

```text
Done and pushed: `<sha> <commit message>`.
```

If work was committed but not pushed, use:

```text
Done and committed: `<sha> <commit message>`.
```

If work was completed locally but not committed, use:

```text
Done locally: <short outcome>.
```

If blocked before completion, use:

```text
Blocked: <short blocker>.
```

## Legibility Rules

- Keep the first line under one sentence.
- Use human-readable names before raw identifiers. For PRs, issues, commits, checks, deployments, and external resources, print the existing title, label, check name, or commit message first, then include the number, short SHA, or ID only as secondary context. Do not write `PR #124` alone when the PR title or link is available.
- Put completed work before blockers.
- Put manual actions in `Needs user action`, not buried in prose.
- If design approval is required, include `Approval evidence` before `Next steps`.
- Use bullets for completed work and validation.
- Use numbered steps for next actions.
- Always include the live GitHub HTTPS close-summary URL as a visible bare URL for mobile review, even when also giving a Markdown link or local file path.
- Prefer the live GitHub close-summary URL over local paths. Local file paths are secondary only.
- Include deployment IDs, commit hashes, migration names, or external resource IDs when they are real.
- Do not paste long logs.
- Do not include validation that was skipped.
- If there are no blockers, omit `Blocked`.
- If there is no manual user action, omit `Needs user action`.
- Always include `Next steps`, even if the only next step is review or approval.
- Always include `Close summary`.

## Blocker Format

Use this shape for external blockers:

```text
Blocked:
<system/resource> is blocked because <specific reason>.

Needs user action:
<exact action the user should take>
```

For DNS records, use a compact table or aligned code block:

```text
Create/update these DNS records:

| Type | Name | Target |
| --- | --- | --- |
| CNAME | clerk.memor.re | frontend-api.clerk.services |
```

## Handoff Link Format

Prefer:

- A paired link row for review targets: `[Phone GitHub app](https://github.com/memor-re/platform/blob/main/.codex/CLOSE_SUMMARY.md#next-steps)` and `[Review URL](https://github.com/memor-re/platform/blob/main/.codex/CLOSE_SUMMARY.md#next-steps)`.
- A visible bare GitHub URL when the canonical repo is remote, defaulting to `https://github.com/memor-re/platform/blob/main/.codex/CLOSE_SUMMARY.md#next-steps`.
- A branch-specific GitHub URL when unmerged branch state is the most accurate handoff target.
- A local path only as a secondary convenience link when working in a local-only repo or when the GitHub URL is not yet valid

Use canonical GitHub HTTPS URLs for phone and review labels. Do not use unofficial `github://` URL-scheme links unless they have been explicitly requested and tested in the target client.

If `.codex/CLOSE_SUMMARY.md` is stale, update it before final response or explicitly say it could not be updated and why.
