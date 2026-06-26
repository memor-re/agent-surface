---
name: ui-optimization-loop
description: "Use for memor.re UI self-learning, UI optimization, repeated UI/design iteration, Refero-backed design loops, design brief improvement, screenshot review, product surface polish, usability metrics, scan-comfort review, and deciding what a UI system should learn from prior iterations. This skill turns Refero research, Council review, user feedback, screenshots, telemetry, and validation results into a bounded learning loop. It does not replace refero-design for visual research; use refero-design first when visual direction is part of the task."
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
  "authoritySurface": "docs/operations/canonical_agent_governance_registry.json#agents.ui-optimization-loop",
  "journalSurface": ".codex/state/current.json + .codex/CLOSE_SUMMARY.md + docs/operations/* evidence packets + PR/check evidence",
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
  "owner": "Design"
}
-->


# UI Optimization Loop

Use this skill when the question is not only "make the UI better" but "make the UI learn from prior work."

This skill creates a repeatable optimization loop. It does not mean the product has autonomous model training, silent UI mutation, or self-modifying production behavior.

## Core Decision

Treat UI learning as a dedicated reusable skill, not as Closer's identity.

- Discover the current Council specialist roster from `memor_re/council/agents/*.md`, excluding `README.md`, `implementation_plan.md`, `ghost_checker.md`, and `quick_reference.md`.
- Do not treat Visionary, Closer, Benefactor, Stabilizer, and Values as the complete permanent set; they are the current implemented specialists and the roster may grow.
- Visionary currently owns the product/design thesis and decides what outcome matters.
- Closer currently owns cadence, sequencing, handoff, and making sure the loop closes.
- Stabilizer currently owns scan comfort, fatigue, accessibility, and repeated-use clarity.
- Benefactor currently owns scope, infrastructure, maintenance burden, and metered-resource risk.
- This skill owns the method for turning evidence into the next UI decision.

Council specialists may create subordinate helper agents for UI research, screenshot review, QA, instrumentation analysis, or implementation support. Those helpers are never peers to core Council specialists and never own the product decision. They operate under a named Council owner, with a narrow brief, acceptance criteria, validation, and a handoff back to that owner.

Only update a Council specialist identity when routing or ownership keeps failing. Do not put a full optimization method inside an agent identity file.

## Learning Tiers

Classify the current capability before claiming "self-learning":

1. Evidence-backed iteration: design brief, Refero lock, screenshots, Council review, and human feedback.
2. Documented learning loop: prior decisions are converted into hypotheses, acceptance criteria, and follow-up checks.
3. Measured optimization: behavioral, usability, accessibility, or task-completion data informs ranked changes.
4. Controlled adaptive recommendations: the system recommends UI changes based on bounded data and requires review before implementation.
5. Autonomous UI mutation: the system changes production UI without review. This is not approved for memor.re.

The current memor.re design process should be described as tier 1 unless a documented loop or measured product data exists.

## Required Inputs

Read only what is relevant:

- `AGENTS.md` and `.codex/CLOSE_SUMMARY.md`
- the current design brief under `docs/design/briefs/`
- the active Refero reference lock or review packet under `docs/design/`
- paid Preline Pro docs, blocks, templates, and plugins for standard product UI primitives; use Preline Community Figma only for methodology and editable design-system reference
- relevant app files under `apps/web/app/` and `apps/web/lib/`
- prior screenshots, browser smoke notes, or user feedback
- available analytics or telemetry already present in the app
- validation commands required by the repo instructions

Final memor.re platform product design artifacts belong in the canonical platform-design Figma file:

`https://www.figma.com/design/rrXwJRJKJ6EJsqb3WncSoe/platform-design?m=auto&t=hlTR7pLMsyNb85B8-6`

Temporary Figma files are scratch/provenance only. Before handoff, migrate or recreate final screens, loop proof, and decision manifests into `platform-design`; if blocked, record the held action and keep local PNG proof.

If Refero research is needed, use `refero-design` first and carry its reference lock into this loop.

For memor.re product UI, Preline Pro is the default implementation-source lens before custom layout work. Start with official Preline docs, framework guidance, blocks, templates, and headless plugins:

`https://preline.co/docs/`
`https://preline.co/docs/frameworks.html`
`https://preline.co/blocks/`
`https://preline.co/templates/`
`https://preline.co/plugins/`

Preline Pro is a code/template/block source, not a Pro Figma file. Use Preline UI Figma Community for methodology, visual-system reference, and editable comparison frames when Figma work is required:

`https://www.figma.com/design/0OQdb2zsXr3kLSZA7rfCXC/Preline-UI-Figma--Community-?t=FezOrN5GkUWfbr5n-0`

If Pro block/template/plugin access, Figma access, or package integration is blocked, record that limitation and use the repo's existing Preline integration plus official Preline docs as the fallback baseline. Do not let the fallback become generic custom SaaS UI.

## Workflow

1. Establish the baseline.
   - Identify the target surface, workflow, route, and user goal.
   - Name the current visual/reference lock.
   - State whether the current process is tier 1, 2, 3, or 4.

2. Extract learning signals.
   - Qualitative: user feedback, design review notes, Council synthesis, screenshot comparison.
   - Behavioral: clicks, completion, drop-off, repeated actions, errors, time-to-action.
   - Usability: scan comfort, touch target size, keyboard reachability, text overflow, contrast, mobile density.
   - Implementation: Preline reuse, custom component risk, shared CSS risk, lint/build/test status.

3. Write one testable UI hypothesis.
   - Format: "If we change [surface/pattern], then [user outcome] should improve because [evidence]."
   - Avoid broad goals like "make it cleaner" unless the observable issue is defined.

4. Recommend the narrowest change set.
   - Name what to change.
   - Name what not to change.
   - Keep Preline responsible for standard UI primitives.
   - Keep custom work limited to memor.re-specific graph, provenance, and relationship-state logic.

5. Define validation.
   - Use source inspection for small copy/layout decisions.
   - Use Browser or Playwright screenshots for user-visible layout changes.
   - Check large and small viewports when responsive behavior changes.
   - Run `npm --prefix apps/web run lint` for web changes.
   - Run `npm --prefix apps/web run build` when routing, framework configuration, shared TypeScript, or deployment behavior changes.
   - Add targeted tests when logic, data contracts, permissions, or route behavior changes.

6. Persist the learning.
   - Update the relevant design brief, reference lock, review packet, or handoff.
   - Record date, source evidence, decision, rejected alternative, validation, and next check.
   - Do not update long-term user memory unless the user explicitly asks.

## Regression And Stop Rule

A loop continues only while each pass creates new evidence or moves a named acceptance check. It regresses when iteration stops serving the user outcome.

Trigger regression when:

- two consecutive passes miss the same acceptance criterion without a new diagnosis
- the next proposed pass broadens scope to avoid a narrow failure
- the loop consumes material review time, token/provider usage, hosted resources, or maintenance burden without better evidence
- the change degrades privacy, provenance, trust, accessibility, scan comfort, or workflow clarity
- the loop needs product, implementation, sequencing, or resource ownership that the current skill cannot decide

Regression action:

- name the last validated state and preserve or restore it
- mark the current hypothesis rejected or inconclusive with evidence
- narrow the next attempt to one element/control and one acceptance check, or convert it into a bounded Council issue
- route ownership: Visionary for product thesis, Infra for implementation evidence, Closer for sequence and handoff, Stabilizer for cognitive-load/accessibility concerns, Benefactor for resource waste or metered/hosted risk, Values for privacy/trust concerns

Dev time alone is not a cash-spend gate, but repeated agent-caused loops are still resource waste. Benefactor review should record the evidence, cap/checkpoint, owner, and stop path before approving another material loop.

## Per-Element Loop Rule

The loop unit is the changed element/control/workflow, not the whole task batch. For example, a relationship graph panel, sidebar collapse, filter group, sort control, row-limit selector, pagination control, and mobile sheet each need loop 1, loop 2, and a stored learning before that element is considered handled.

Do not run loop 1 across many elements and then return later for loop 2 across the same batch. Complete loop 1 and loop 2 for one element, persist what worked and what did not, then move to the next element.

Exception: when Dontae explicitly defines the design loop as the final rendered screen, the loop unit is the complete rendered screen across the target route set. In that mode, each loop must touch the whole screen and every visible component relationship: hierarchy, controls, copy, density, responsive collapse, empty/error states, privacy/provenance cues, and Preline baseline fit. Taking UI slices for QA is not a design loop. If 10, 20, or another explicit number of loops are requested, produce that many full-screen loops with PNG proof.

## Interaction-Control Ledger

When a UI loop touches menus, collapsible sidebars, filters, sorts, row limits, pagination, or component add/remove choices, the persisted record must include:

- element/control name and owner
- changed controls and the user outcome they were supposed to improve
- removed components, added components, and preserved components
- before and after screenshots or browser notes
- what worked, what did not work, and what was rejected
- validation results, viewport coverage, and next check

Without that record, describe memor.re's current process as evidence-backed iteration rather than a documented learning loop.

## Output Shape

Use this shape for optimization answers:

```text
Answer:
<real answer first>

Current capability:
<tier and evidence>

Ownership:
<current Council specialist / skill split>

Learning loop:
- Input:
- Hypothesis:
- Change:
- Validation:
- Persisted learning:
- Regression/stop:

Next action:
<one concrete next move>
```

For implementation tasks, add changed files and validation actually run.

## Guardrails

- Do not claim self-learning unless there is persisted evidence or measured feedback.
- Do not create autonomous production UI changes.
- Do not add analytics, external services, paid tools, production mutations, or new data collection without approval.
- Do not collect private corpus content as UI telemetry.
- Do not optimize vanity metrics at the expense of workflow clarity, provenance, privacy, or trust.
- Do not make Council specialists equal-weight UI decorations. They are decision lenses.
- Do not elevate helper agents above or beside core Council specialists. Use them as bounded workers under Council ownership.
- Do not average references into generic SaaS design. Keep the chosen reference lock sharp.
- Do not keep looping after the acceptance criteria stop improving; regress, narrow, or hand off the blocker.
