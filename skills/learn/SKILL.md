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
    "npm run agent:governance:validate",
    "npm run agent:lifecycle:preflight",
    "npm run close:validate"
  ],
  "scaffoldLearnState": "Act, Notice, Check, Repair, Align are required before learned, fixed, clean, ready, shipped, or durable claims.",
  "owner": "Learn"
}
-->


# Learn

Use this memor.re Learn capability to turn a completed or blocked run into reusable operating memory. Learn owns discrete learning lenses such as observe, document, measure, and recommend. ScaffoldLearn is not one selected lens and it is not a menu; it is the canonical multi-agent learning pass that applies the relevant lenses together, routes the canonical Council owners, diagnoses Act, Notice, Check, Repair, and Align, then hands the result to the Observer Layer for durable reflection. It is not autonomous self-learning and it is not a Council peer.

This is a memor.re canonical skill source. It belongs under `.agents/skills/learn/`, should be bundled through `plugins/memor-re`, and should be imported through the memor.re product skill pipeline when it needs to appear in cloud/user skill surfaces. Do not rely on a personal/global Codex-only copy as the source of truth.

Council ownership stays explicit: Infra owns stack/tool/provider lessons, Closer owns release and dirty-tree lessons, Values owns privacy and trust lessons, Benefactor owns spend and production-resource lessons, and Weight Steward owns durable outcome-weight learning. Learn packages the evidence into the right mode for those owners.

## Learning Modes

Learn modes are internal lenses, not the full learning methodology. ScaffoldLearn is the user-facing and Council-facing learning model: the combined Learn, Council, Ghosthunter, Clean, Infra, Closer, Values, Stabilizer, Benefactor, Visionary, and other implicated owner pass that produces Act, Notice, Check, Repair, and Align. Use the mode names below as implementation states only. Do not present them as basic learning tiers, a picker, or a substitute for the whole ScaffoldLearn pass.

- `learn:observe`: capture evidence, name the symptom, and state a tentative rule without changing durable sources.
- `learn:document`: convert a repeated failure, blocker, release lesson, or user correction into a durable skill, runbook, test, issue template, or handoff rule.
- `learn:measure`: use measured outcomes, usage, cost, validation results, or forecast error to compare whether an existing rule is working.
- `learn:recommend`: produce a bounded adaptive recommendation with owner, cap, stop rule, and validation after enough measured evidence exists.
- `learn:autonomy`: not approved for memor.re by default. Do not silently mutate skills, weights, provider settings, budgets, hosted resources, or production behavior without explicit approval.

If older docs or code say "tier", translate that language into modes before reporting to the user. Do not present Learn as "Tier 2" in user-facing skill metadata.

Every learning event must produce or reference a ScaffoldLearn frame. A single mode can contribute evidence, but it cannot replace the complete frame:

- Act: who acted, what changed, and where.
- Notice: what the user, agent, test, provider, or product surface observed.
- Check: which authority-bearing surfaces were inspected and which remain unchecked.
- Repair: what artifact, code, gate, UI, CI, runtime behavior, or process changed.
- Align: the accepted rule, candidate rule, rejected assumption, owner, validation, and next action.

## Anti-Sycophancy Evidence Gate

Every ScaffoldLearn pass must check whether the agent agreed too early. Before preserving a lesson, Learn must name the current evidence, the untested assumption, the missing evidence, and the strongest opposing case.

- Act: identify where the agent agreed, reassured, claimed done, or softened a user challenge.
- Notice: separate the user's report, tool output, provider evidence, local proxy evidence, and agent interpretation.
- Check: test the strongest opposing case before accepting the convenient answer.
- Repair: make the failed rule harder to skip through a validator, structured packet, runtime check, CI check, product UI gate, or explicit candidate gate.
- Align: record whether the incident was a missing rule, ambiguous rule, or rule execution failure.

Do not reassure from proxy evidence. Do not use flattery openers. Do not retreat because Dontae pushes back unless new evidence, reasoning, or constraints change the answer. Do not invent flaws to perform rigor. If no real flaw exists, say so plainly with evidence and name what remains uncertain.

## Observer Layer

The Observer Layer is the Visionary-led ScaffoldLearn checkpoint for every memory, every surface, every integration, and every learning loop. It separates what appeared from what it means, asks how the system should grow from the learning experience, and decides what becomes durable so memor.re can support any surface, any memory, any time, any place, any integration without flattening human experience into false authority.

Learn must not collapse memory, thought, belief, feeling, narrative, projection, and later reflection into one accepted truth. A captured statement can be meaningful without becoming source truth, accepted memory, runtime authority, or user identity.

Before Learn preserves, promotes, disputes, routes, or acts on a cognition, source record, memory, relationship edge, provider event, stale client record, extension event, local alert, AI output, user report, public claim, or runtime conflict, create or verify an observation ledger:

- Observed evidence: the exact surface, command, file, table, event, screenshot, alert, PR, provider status, or user report.
- Thought or hypothesis: what the agent thinks might explain the evidence.
- Belief, feeling, or narrative: what the user or agent is worried the evidence means.
- Reflection: what changed after stepping back from the first interpretation.
- Authority classification: active authority, guardrail, projection, historical provenance, archive, adjacent symptom, or delete-candidate.
- Missing evidence: which surface would prove or disprove the hypothesis.
- Memory/review state: candidate, accepted, disputed, corrected, revoked, deleted, archived, or retained as provenance only.
- Visibility state: private, shared, public, operator-only, or unknown.
- Mutation target and rollback path: the exact object to change, backup location, restore proof, and stop condition.

When a user reports an extension invocation surface event such as `/`, `$`, `@`, a plugin picker, browser extension, desktop command palette, mobile action, or Windows Security/Defender alert, record the alert as observed evidence. Do not conclude the trigger caused dirty files, old plugins, malicious behavior, or runtime authority drift until process, signer, event-log, app, registry, client-state, and repo evidence have been checked. Route the incident to Infra for runtime/tool evidence, Ghosthunter for residue, Values for user-trust/privacy, Stabilizer for user load, and Closer for release/cleanup gates.

The Observer Layer applies across memory formation, import, capture, search, graph, AI interpretation, public publishing, agent planning, sync, deletion, and cleanup. A source packet is not yet accepted memory. An AI summary is not source truth. A feeling is not a fact, but it is still meaningful context. A stale Codex mobile project entry is not a live project until Git, filesystem, Codex client state, and official app projection evidence agree. A local database row is not a user belief, a user belief is not source truth, and source truth is not complete until the consuming surface has been checked.

## Boundaries

- State the ScaffoldLearn frame and the participating Learn lenses before claiming capability. Use `learn:observe`, `learn:document`, `learn:measure`, or `learn:recommend` as evidence lenses; treat `learn:autonomy` as unapproved unless the user grants a specific, bounded mutation path. Do not present one lens as the whole ScaffoldLearn result.
- When Learn is called directly, Learn owns evidence-to-memory packaging while activating logical dependencies automatically. Route the owner split to Visionary, Infra, Closer, Benefactor, Values, Stabilizer, Design, Clean, Ghosthunter, Letsgo, Weight Steward, or Superpowers when the learned rule changes their operating boundary.
- Do not store secrets, OAuth tokens, service-role keys, private corpus data, raw artifacts, production credentials, or sensitive user data in learned memory.
- Do not treat a lesson as true until it is backed by local files, command output, provider status, CI logs, PR evidence, or an explicit user decision.
- Do not treat agreement, user pressure, internal confidence, prior success, token usage, local ledger estimates, or skill prose as proof. Route the evidence through the Anti-Sycophancy Evidence Gate before any "learned", "fixed", "ready", or "spending" claim.
- Do not broaden product code to compensate for a narrow workflow, permission, provider, or CI-policy failure.
- Keep production mutations, hosted-resource changes, paid infrastructure, destructive cleanup, external sends, and secret handling approval-gated.
- Treat "deploy the lesson", "residual risk: update", and similar requests as permission to start the Closer release protocol for the durable change, not as permission to merge, publish to production, mutate hosted resources, handle secrets, or spend money.
- Preserve user work. Never run `git add .`, reset, force-push, delete worktrees, or overwrite unrelated local changes while creating a learning update.

## Workflow

1. Capture evidence:
   - Record the concrete symptom, affected branch or PR, command output, run ID, deployment ID, provider status, relevant file path, and exact blocker.
   - Separate code failures from workflow failures, provider configuration failures, rate limits, missing scopes, CI policy failures, and stale context.

2. Extract the lesson:
   - State the smallest reusable rule learned from the evidence.
   - Check whether the rule already exists in `AGENTS.md`, Council specs, skills, docs, tests, workflows, or provider runbooks before calling it new.
   - If a rule already existed and was not followed, classify the incident first as a rule execution failure.
   - Select the memor.re learning mode from current evidence. Use `learn:observe` for tentative or one-off evidence, `learn:document` for durable operating memory, `learn:measure` when measured outcomes exist, and `learn:recommend` only for bounded adaptive proposals with validation.
   - Identify where the lesson should live: skill, CI workflow, test, issue template, GitHub label, autolink, handoff note, runbook, Council owner spec, or product code.
   - Prefer an enforceable change over prose when a script, check, test, template, or label can prevent recurrence.
   - Treat repeated authority, provider, credential, dependency, design-proof, closure, privacy, spend, or production mistakes as candidate code-gate work until a validator, runtime check, UI gate, or CI check exists.

3. Route ownership:
   - Use Infra for provider/tool capability inventory, dashboard settings, CI policy, deployments, auth status, env posture, and validation packets.
   - Use Closer for release sequencing, dirty-tree discipline, branch closure, blocker packets, and final handoff.
   - Use Clean for turning a dirty or ambiguous tree into ship, split, park, or ignore.
   - Use Letsgo only after the learned change is scoped, validated, and ready to move through the normal release path.
   - When residual risk requires a durable skill, template, runbook, or workflow update, route Learn -> Skill Creator for authoring and then Closer for the release/deploy protocol. Closer names the decider, domain gate, held action, evidence needed, and stale/override path before any push, PR readiness, merge, production deploy, or hosted publication.
   - Use Council or a named specialist when the lesson changes product values, privacy posture, spending, ownership, or production operating rules.
   - Use Weight Steward when the lesson is about changing default Council weights, specialist selection, or outcome-quality scoring.

4. Write the durable memory:
   - For a new repeated workflow, create or update `.agents/skills/<skill>/SKILL.md` plus `agents/openai.yaml`; update bundling scripts if the skill should ship in `plugins/memor-re`.
   - For a learned skill or workflow update that the user asks to "deploy", create the smallest repo-local change first, validate it, and hand it to Closer as a release packet. Do not treat that wording as approval for production skill publication or other hosted mutations unless the target, owner, rollback, and approval gate are explicit.
   - For release evidence, update `.codex/CLOSE_SUMMARY.md` or an issue with the current branch, commit, checks, deployment, blockers, and residual risk.
   - For GitHub operating memory, add labels, templates, autolinks, rules, or workflow checks only when they directly prevent the repeated failure.
   - For app behavior, add a focused test before or with the implementation when the lesson describes a regression risk.

5. Validate:
   - Run `git diff --check`.
   - For skills, run:

     ```powershell
     npm --prefix apps/web run skills:import -- --repo https://github.com/memor-re/platform.git --ref <branch-or-main> --path .agents/skills/<skill-name> --user dry-run-placeholder --dry-run
     npm --prefix packages/agent-skills test
     ```

   - Run web lint/build only when the learned change touches `apps/web`, package manifests, routing, deployment config, or shared TypeScript behavior.
   - Verify any provider, CI, or deployment lesson against current status before marking it closed.

## Learning Packet

When reporting what memor.re learned, keep it compact:

- `scaffoldLearn`: Act, Notice, Check, Repair, and Align.
- `mode`: current learning mode and evidence.
- `symptom`: what failed or slowed the run.
- `evidence`: exact command, run, PR, deployment, provider status, or file path.
- `lesson`: the reusable rule.
- `existing rule status`: followed, missing, ambiguous, or ignored.
- `durable change`: what was updated so the lesson persists.
- `owner`: skill, Council agent, workflow, issue, or provider owner.
- `validation`: checks that prove the learned change works.
- `residual risk`: what still needs user, provider, credential, or production follow-up.
- `closer handoff`: when the lesson is ready to ship, name the decider, gate, held action, evidence needed, and stale/override path.

## Operating Lessons

- Plain language is valid user intent. Treat `$`, `@`, and `/` as precision or audit syntax, not as required ceremony. If the action is low-risk and the target is clear, infer the right skill or Council owner and proceed; if it crosses merge, production deploy, hosted mutation, secrets, spend, destructive cleanup, or provider/DNS changes, stop and name the held action plus evidence needed.
- Dates are not blockers by default. If Dontae says he wants to ship, review, or revisit something on a date, learn that as scheduling intent and create periodic check-ins for pacing visibility; do not convert it into a hold unless he explicitly says to pause until that date or the named action is independently approval-gated.
- One Council or canon call is enough to start dependency flow. Learn should persist the rule that the invoked owner must pull in logical dependencies automatically, leave an evidence trail, and continue safe local/read-only work until a real approval gate appears.
- Fresh-chat amnesia is a workflow failure. Durable lessons must land in a canonical source future chats read, such as `AGENTS.md`, Council specs, skills, tests, or repo workflows; local-only chat edits are not learned until committed and routed through the release path.
- Fresh implementation chats should not inherit stale branch state. Learn the rule as: refresh or verify `origin/main`, create or attach to an isolated branch/worktree before edits, and preserve read-only planning chats as branch-optional.
- Fresh-worktree dependency misses are part of the same lesson. An isolated worktree without `node_modules` should be bootstrapped with `npm ci --ignore-scripts --no-audit --no-fund` before treating `tsx`, `js-yaml`, local CLIs, or package scripts as unavailable.
- Generated-artifact validation is not parallel-safe. Run build or import generation commands before validators that read their output; do not run commands such as `npm run plugin:build` and `npm run plugin:validate` at the same time, because validators can observe a half-recreated bundle and produce false missing-file blockers.
- Disambiguate overloaded words from context. "Learn this" after a run, blocker, incident, release, or agent workflow means durable operating memory; "help me learn Spanish", "teach me React", or "I want to learn a new skill" means educational help unless the user explicitly asks to persist a memor.re lesson.
- Human-readable labels are required for status references. Use existing fields such as PR title, issue title, check name, commit message, deployment name, and preview URL before raw numbers or IDs; raw identifiers alone are not acceptable handoff language.
- Residual notes must be translated into user-action language. When reporting a warning, annotation, skipped check, pending follow-up, deprecation, or non-blocking risk, say: what it means in plain English, whether Dontae has to do anything now, whether it matters now or later, who owns the follow-up, and the date or trigger. Do not leave terms such as "Node 20 deprecation annotation", "non-blocking", "residual risk", "current gate", or "provider surface" standing alone without that translation.
- Clustered reliability failures require a cluster lesson, not only separate incident records. When multiple Codex, plugin, MCP, local-tooling, or agent-surface symptoms happen in one session, package the learning with confirmed root causes, adjacent symptoms, confidence level, Council owner split, support owner, validation after unblock, and the exact rule or eval that prevents recurrence.
- Scope category is part of the lesson. Minor UI/copy/style work can use narrow discovery and focused validation; normal product features need route/data/auth/design checks; critical platform, agent, sync, auth, deploy, local-tooling/mobile, local automation, or hidden-automation issues need adjacent-surface audits and a Closer/Stabilizer retrospective before final closure; destructive/admin/vendor cleanup needs vendor-first tools, backups, exact target approval, and post-action proof.
- Retired local-control lessons must keep runnable toolchain checks in repo validation such as `npm run tools:doctor`, not deleted local-control skills. Do not reference deleted wrapper scripts as live repair paths. Print command output before remediation, and treat package-upgrade listings as diagnostic evidence, not blanket approval.
- PowerShell cleanup commands must be boring and parse-safe. Do not pipe directly from `foreach`, `if`, `try`, or other script blocks in one-liners; assign rows to a named array first, then pipe that array. For literal Windows paths, prefer `-LiteralPath` and `-SimpleMatch`. For Python or Node stdin, use PowerShell here-strings or checked-in scripts instead of bash heredocs. For destructive cleanup, run inspect, path-guard, backup, mutate, and verify as separate steps.
- Do not preserve OS setting, file-association, or app lifecycle mutations as default memor.re behavior.
- Named tool or research skills require their own readiness evidence before fallback. When a user invokes `$firecrawl`, `$firecrawl-research-papers`, or another external research/tool skill, read the skill and run its status or readiness command before declaring it unavailable. For Firecrawl, run `firecrawl --status`; a missing `FIRECRAWL_API_KEY` environment variable is not proof of unavailability because the CLI can be authenticated through stored credentials. If a named skill such as `$research` is not installed, say that explicitly and map to the closest available skill, such as `$firecrawl-research-papers`, before continuing.
- After a production release has passed checks and Dontae explicitly grants scoped Closer/Infra/sysadmin/Letsgo movement for a production data import, absence of an attached signed-in session is not a blocker by itself. First attempt the production-safe server-auth route through Clerk CLI or Backend API, app connector, MCP, authenticated CLI, or ignored local env credential; stop only for provider denial, missing scope, ambiguous target environment, MFA or user-custody requirement, secret exposure risk, destructive/remapping action, or a write beyond the approved scope.
- Stale-branch incidents are release workflow lessons, not only Git cleanup. Before learning from, deleting, or deprecating a branch that caused cross-chat failures, prove extraction status with PR/main evidence, `git cherry -v`, and a compact accepted/superseded/rejected ledger for patch-unique commits. If the user says not to park a branch, switch the lesson from parking to extracting or superseding every safe delta before closure.
- CodeRabbit release evidence must include the latest review body and comment threads when CodeRabbit is invoked. A green or successful status context is not enough if the latest review is `COMMENTED` or contains actionable findings; treat those comments as blockers until fixed, explicitly superseded, or waived with evidence.
- Unexpected repo changes are signals before they are blockers. When Git reports an unrelated change, name the actor chain, inspect the actual diff, classify it as task work, user work, generated artifact, line-ending/index noise, or unsafe unknown, then repair only the unrelated noise. Do not include unrelated noise in the slice, and do not freeze safe work just because an inspectable signal appeared.
- Use `scaffoldReduce` when explaining technical incidents across ages or experience levels. Reduce concept jumps by adding intermediate rungs, naming the actor instead of saying "the computer", using concrete objects for younger audiences, and only introducing terms such as Git, index, line endings, worktree, or `npm ci` when the rung can carry them.
- Use `scaffoldLearn` as the default learning frame for memor.re and Council. Start with the simplest true sentence, add one layer at a time, name actor/action/evidence/decision, store the memory, and aim for alignment. The compact recall is: Act, notice, check, repair, align.
- If an established guideline exists and was not followed, classify the incident first as rule execution failure, not net-new learning. The repair must make the rule harder to skip through a validator, runtime check, CI check, product UI gate, structured evidence contract, or explicit candidate gate.
- Prose is not a gate. When a repeated miss involves authority, providers, credentials, dependencies, closure language, design proof, privacy, spend, or production state, route a code-gate plan before treating the lesson as durable.
- Action-memory is required after repair. When an agent fixes a workflow miss after user correction, such as deleting merged branch residue, moving a workspace back to current main authority, resolving provider-surface confusion, or repairing a stale checkout, the repair is not learned until a ScaffoldLearn memory exists. The memory must name Act, Notice, Check, Repair, and Align; cite command, PR, branch, provider, or file evidence; classify whether the rule was missing, ambiguous, followed, or ignored; assign the Council owner; and point to the durable source, test, gate, or handoff that future agents will read.
- Authentication blockers should try the phone path first when the provider supports it. For GitHub, CodeRabbit, Vercel, Supabase, Clerk, Cloudflare, or similar provider auth, prefer mobile approval, GitHub Mobile, push confirmation, or phone-delivered verification before passkeys, web-only prompts, fallback tokens, or asking Dontae to return to a PC; never handle OAuth fallback tokens, secrets, or recovery codes in chat.
- OpenAI API usage is a governed capability, not always a blocked spend event. When Dontae or Sys Admin approves a capped usage grant, existing-key OpenAI API consumption may proceed inside the cap with local usage evidence. Learn must route the durable lesson to Benefactor for cap economics, Infra for key/provider/ledger evidence, and Closer for release discipline. Purchases, Codex auto top-up, cap increases, key creation or rotation, production env changes, hosted mutations, and secret exposure remain separate gates. Codex/ChatGPT usage-limit pressure is not evidence that API burst spend is the right fix.
- Cross-surface provider ambiguity is a durable learning failure when an agent collapses app/account state, developer/API console state, repo-local config/env/ledger state, hosted resource state, connector/session state, or automation/runner state into one generic platform answer. Preserve the surface map, evidence required for each surface, owner split, and exact UI, CLI, connector, or repo path that resolves the user's actual blocker.
- Underusing approved capacity is also a workflow failure. If Dontae has funded credits or approved a cap to accelerate work, preserve the rule that agents should route that capacity to the right surface and use it deliberately, rather than downgrading defaults or slowing execution from a conservation assumption.
- Clean-base failures must be learned as release-sequencing failures, not CodeRabbit failures. Code review can find bad code, but it cannot prove the PR started from the right tree; persist the rule that `release:assert-clean-pr-base -- -Fetch` gates review movement, and stale branches are rebuilt from fresh `origin/main` before review.
- A new worktree from `origin/main` is only as fresh as the local remote-tracking ref. Before `git worktree add ... origin/main`, run `git fetch origin main` or compare `git ls-remote origin refs/heads/main` to `git rev-parse origin/main`; if they differ, refresh first and then create or fast-forward the worktree.
- Repeated fresh-worktree validation misses are delivery workflow lessons. If a clean worktree lacks local `node_modules` and validation fails on repo dev dependencies such as `tsx` or `js-yaml`, route the durable change to Clean and Letsgo as isolated worktree bootstrap guidance instead of treating the CLI, skill, or repo path as unavailable.
- Local system cleanup is a cluster lesson, not a folder-deletion lesson. When uninstall blockers, broken app icons, leftover folders, relaunched helpers, graphics overlays, and stale registry records appear together, inventory installed products, uninstall registry keys, services, launch registration, process paths, vendor databases, and official vendor cleanup tools before deleting. Prefer vendor uninstallers or signed vendor cleaner tools; use manual folder or registry deletion only for exact targets after backup, admin/user custody, and post-action validation.
- Broken icons are smoke, not proof of the fire. Repair missing `DisplayIcon` values only after proving the app is still installed and the icon target exists; classify intentionally iconless runtime/MSI/driver entries separately from user-facing apps. HKCU icon fixes can be local user repair, while HKLM icon fixes require admin custody and backup.
- Codex mobile continuity must stay on official Codex channels. Do not create custom local launch, sync, lifecycle, or database paths to make mobile commands affect Windows. If mobile approval or continuation is needed, use the official Codex conversation/session path or provider mobile approval flows; any local user-machine evidence belongs outside memor.re.
- Cross-surface Codex session parity is a source-of-truth lesson, not only a UI or plugin lesson. When an agent-readable thread is missing from official app/session surfaces, mobile, project views, or search, Learn must preserve the exact thread ids, visible surfaces checked, local index state, archive/project labels, plugin state, and canonical thread API evidence. Treat plugin cache-lock fixes as adjacent infra unless they also prove session-list reconciliation across surfaces.
- When repeated tool, plugin, MCP, local-tooling, or workflow failures create user cognitive load, Learn should package the cluster with Stabilizer support, not only technical fixes. Stabilizer names what is stable, what is not the user's job to hold, the root-cause owner, the next stabilizing action, and the next check-in; Infra/Closer/Clean still own root cause and release movement.
- A safe operating home is where memory, values, and active authority align. Learn should preserve painful or useful history as provenance without letting retired instructions keep controlling present decisions. When a cleanup finds old guidance, classify each reference by authority: active rule, guardrail, historical provenance, archive, or delete-candidate. The lesson is not learned until active surfaces stop routing through retired authority while the evidence remains recoverable.
- Authority-aware broad-source learning is the default for repeated reliability, privacy, spend, sync, plugin, MCP, local-tooling, relationship, organizational, or provenance failures. Learn must map every authority-bearing actor and surface before preserving a lesson: people, communities, organizations, institutions, agents, tools, nodes, systems, datasets, interfaces, providers, configs, logs, process trees, plugin caches, MCP clients/profiles, and repo docs. Classify exposure risk separately from proven use, repair the relaunch or decision authority rather than only killing symptoms, and preserve user-facing receipts without copying secrets or private raw artifacts.
- The `.codex` home is provenance and runtime authority, not the project workspace. When learning from Review, branch creation, or session failures, classify `$CODEX_HOME` as home/config/cache evidence, remove or deprecate any saved-project authority for that path, and preserve the folder unless exact cleanup approval exists. Future repo work must route through the platform repo target or a platform repo worktree.
- Skill-state learning is not shipped until the source, bundle, and installed projection agree. When a Learn change touches `.agents/skills`, `plugins/memor-re`, Codex plugin cache instructions, or `config.toml` marketplace revision state, verify the installed/cache revision against source truth, rebuild the memor.re plugin bundle, refresh or name the stale local projection, and include that evidence in the Closer packet. Dontae should not have to remind agents to do this after skill updates.
- Human-language copy is part of the lesson, not polish after the lesson. When a learned rule protects user files, settings, privacy, authority, or trust, preserve the user-facing sentence pattern: name the object in human terms, say what lives there or why it matters, state the protective action, then offer the safe next step. Technical labels can support the explanation, but they should not carry it.
- Observer Layer evidence is required before cleanup claims. When a surface makes stale history feel active, such as a mobile project list showing dead worktree roots, Learn must separate observed client projection, agent hypothesis, user concern, source authority, and mutation target before repair. The repair is not learned until the source, projection, backup, and rollback path are all named.
- New epics and feature requests need a product packet before agents implement. The durable lesson from user-scoped plugin activation is that PRD + spec + wireframe artifacts reduce ambiguity and speed agentic automation. For every non-trivial epic or feature, route Visionary to own the product PRD, Infra to own the technical spec or implementation plan, Design to own wireframes or interaction notes for user-facing surfaces, Values to own privacy/consent/provenance gates, and Closer to own acceptance criteria, validation gates, release sequencing, and handoff path. Tiny direct fixes can use the same sections inside an issue or PR body instead of separate files.

## Reliability Cluster Learning Loops

When Dontae asks for "10 discrete learning loops" after a critical Codex, plugin, local-tooling, mobile, MCP, session, or sync failure, treat it as `learn:document`. The learned packet must pass back through Council for owner discussion, then through Ghosthunter for residue cleanup, then through Closer for release, plugin rebuild, validation, and handoff. Do not close the goal because one layer improved.

Run these loops in order:

1. Scope Fidelity Loop: restate the full user objective, including all named surfaces and success criteria. Do not shrink "fixed" to the first surface that looks repaired.
2. Surface Inventory Loop: enumerate the relevant official app UI, mobile UI, thread API, project picker, config files, SQLite or local state, session files, plugin cache, MCP/tool mount state, process tree, and logs.
3. Authority Classification Loop: classify every discovered actor or artifact as active authority, guardrail, loaded helper, historical provenance, archive, or delete-candidate.
4. Backend Continuity Loop: prove trusted project config, saved roots, active thread records, local session files, archive flags, and canonical thread API agree before claiming backend repair.
5. Frontend Parity Loop: prove official app Recents, project view, search, mobile Recents, mobile project view, and mobile search match the backend, or name the exact missing evidence and leave parity open.
6. Archive/Residue Loop: reconcile active files against archived records. Move only exact archived residue out of active paths, keep backups, and never promise historical disappearance.
7. Process Trust Loop: verify package identity, executable path, command line, Authenticode signature or package signature, signer, hash, and start time before calling a process official or suspicious.
8. Tool-Surface Loop: separate installed plugin, enabled plugin, mounted callable tool, MCP pipe, helper process, UI affordance, and connector availability. A present file or process is not proof the tool is usable in the thread.
9. Stabilizer/User-Load Loop: name what is stable, what is not Dontae's job to hold, the owner of remaining uncertainty, and the next validation step without making the user carry the map.
10. Closer/Release Loop: close only after source truth, backend state, UI proof, missing evidence, residue owner, updated skill source, rebuilt plugin bundle, validation output, and handoff packet are all explicit.

## Lessons From The #28/#29 Release

- A 16K-line sidebar diff is not proof the active PR is huge; compare `origin/main...HEAD` from the right branch before judging scope.
- A GitHub check failure can be an infrastructure policy failure, not a product-code failure. Inspect the workflow log before changing app code.
- Repos with action pinning policies require full commit SHAs for GitHub Actions; pin the action rather than overriding the gate.
- Auth routes returning `200` is not the same as production OAuth being complete. Check Clerk domain and OAuth provider status separately.
- Dirty root checkouts should not be used for focused shipping when a clean worktree from `origin/main` can preserve user work and reduce merge risk.
