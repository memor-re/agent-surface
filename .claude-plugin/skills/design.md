---
name: design
description: "Use for any memor.re UI/UX, product screens, dashboards, data visualization, Figma work, prototypes, interaction flows, visual systems, frontend polish, or design-system decisions. Owns research-first design methodology (visual direction, UI patterns, flow references), design optimization loops (iterative review against references, screenshot critique, usability metrics), and Preline Pro / brand-identity integration."
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "design",
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
  "authoritySurface": "docs/operations/canonical_agent_governance_registry.json#agents.design",
  "journalSurface": "hosted memory ledger (memory_list_recent + memory_search) is the orientation source of truth; docs/operations/* evidence packets and PR/check evidence are the proof surface. Host-app shell directories are never orientation or journal authority.",
  "idempotencyKey": "agent:design:active-slice:authority-surface:validation-command",
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


# Design

Use this skill as memor.re's UI/UX operating layer. Design supports Visionary's product judgment and Infra's implementation constraints. It is not a Council specialist canon file; route product approval through Visionary with Council guidance, and route stack/tool feasibility through Infra.

This is a memor.re platform skill source. It belongs under `.agents/skills/design/`, should be bundled through `plugins/memor-re`, and should be imported through the memor.re product skill pipeline when it needs to appear in cloud/user skill surfaces. Do not rely on a personal/global Codex-only copy as the source of truth.

## Source Canon

Before designing memor.re product UI, inspect the current design canon:

- `docs/design/README.md`
- `docs/design/briefs/relationship_graph_home_workspace_b_prime.md`
- current route/product brief, supplied Figma file, screenshot, or user request
- `.codex/CLOSE_SUMMARY.md` when the work changes release state or next steps

Final memor.re platform product design artifacts belong in the canonical platform-design Figma file:

- `https://www.figma.com/design/rrXwJRJKJ6EJsqb3WncSoe/platform-design?m=auto&t=hlTR7pLMsyNb85B8-6`
- Temporary Figma files may be used for scratch exploration, rate-limit recovery, or proof generation, but final editable screens, proof contact sheets, and decision manifests must be migrated or recreated in `platform-design` before handoff.
- If migration is blocked by Figma access, rate limits, or file permissions, record the held action and keep local PNG proof in the repo until the canonical file can be updated.

For memor.re product UI, paid Preline Pro code assets are the implementation baseline:

- Start with official Preline docs, framework guidance, blocks, templates, and headless plugins before inventing bespoke product UI: `https://preline.co/docs/`, `https://preline.co/docs/frameworks.html`, `https://preline.co/blocks/`, `https://preline.co/templates/`, and `https://preline.co/plugins/`.
- Treat Preline as the first lens for standard UI primitives: page shells, workspace headers, cards, tables, list groups, tabs, accordions, dropdowns, overlays/modals, forms, input groups, buttons, badges, dividers, pagination, drawers, status panels, and empty states.
- Preline Pro is a code/template/block source, not a Pro Figma file. Translate its HTML/Tailwind/JavaScript patterns into the existing Next.js App Router app with `preline/non-auto`, Tailwind CSS, `@tailwindcss/forms`, memor.re tokens, density, privacy/provenance language, and lucide icons.
- Use the Preline UI Figma Community file for methodology, visual-system reference, and editable comparison frames when Figma work is required: `https://www.figma.com/design/0OQdb2zsXr3kLSZA7rfCXC/Preline-UI-Figma--Community-?t=FezOrN5GkUWfbr5n-0`.
- Do not silently fall back to generic custom SaaS UI. If Pro block/template/plugin access, Figma access, or package integration is blocked, record the held action and keep safe local implementation or PNG proof moving.

## Modes (ADR-013)

Design operates in three modes, absorbed from prior standalone skills per ADR-013. Use the smallest mode that fits; modes compose when the task spans both research and optimization.

### Design.ReferenceLink (absorbed from refero-design)

Research-first visual direction. Run before any design implementation. Mandatory when the task involves visual language, layout, component decisions, or style systems.

**Three research layers:**
1. Styles — visual direction and taste (color, type, spacing, motion, density)
2. Screens — concrete UI patterns from shipped product screens
3. Flows — multi-step journey logic and sequencing

**Non-negotiables:**
- Research precedes implementation. Never rely on model training data for design taste.
- Study several strong references, synthesize a new direction. Do not copy one source.
- Do not average references into a safe middle. Choose a dominant direction; preserve its sharp traits.
- Create or update an interaction-control ledger for menus, sidebars, filters, sorts, row limits, pagination, and component add/remove decisions.
- Complete loop 1 and loop 2 per element/control before moving on. Do not batch.

Use Refero MCP tools (`styles`, `screens`, `flows`) when available. When unavailable, use bundled craft references in `references/` and keep the same reference-lock workflow. Store reference locks in the interaction-control ledger before implementation begins.

### Design.Optimize (absorbed from ui-optimization-loop)

UI learning loop. Run after loop 1 and loop 2 are complete, when the question is "what should the UI learn from prior iterations?" — not "make this look better."

**Learning tiers (name the current tier before claiming self-learning):**
1. Evidence-backed iteration — brief, Refero lock, screenshots, Council review, human feedback
2. Documented learning loop — prior decisions become hypotheses, acceptance criteria, follow-up checks
3. Measured optimization — behavioral or task-completion data informs ranked changes
4. Controlled adaptive recommendations — system recommends UI changes; review required before implementation
5. Autonomous UI mutation — **not approved for memor.re**

For each optimization loop: name the current tier, record what changed and why, state the acceptance check that would confirm the change worked, and attach rendered proof. Do not advance a tier without evidence that the prior tier's acceptance check passed.

### Design.Render (from frontend-design)

Implementation layer. Translates reference-locked and optimized designs into Next.js App Router components using Preline Pro blocks, Tailwind CSS, `@tailwindcss/forms`, memor.re tokens, and lucide icons. Route to Infra for stack feasibility and validation gate before production movement.

---

Use `refero-design` as the default research and reference-lock method (invokes `Design.ReferenceLink`). Use Figma MCP when the user mentions Figma, provides a Figma URL, or needs editable design/prototype work. Use Build Web Apps for concept-first frontend surfaces and rendered UI QA. Use Build Web Data Visualization when charts, maps, graphs, timelines, visual evidence, or dashboards shape the experience.

## Design Sensibility

Use `references/design-sensibility.md` as a compact source-backed lens. Treat named people as principle sources, not personalities to imitate.

Default posture:

- Jony Ive / LoveFrom: ideas are fragile; writing, sketching, models, prototypes, and practical collaboration protect them. Simplicity means clarifying essence, not stripping away useful affordances.
- Nikita Bier: every tap is scarce. Screens should respect attention, make the next action obvious, and avoid wasting interaction calories.
- Danielle Morrill: category-defining product work comes from evidence, customer development, useful data, and the willingness to stop doing plausible but weak things.
- memor.re canon: precise private relationship intelligence, Preline-first product UI, visible provenance, governed AI capabilities, compact linework, scarce accent, and no agent theater.

## Two-Loop Gate

Do not mark memor.re UI/UX work production-ready until every changed design element has completed two loops. The unit is the element/control/workflow being changed, not the whole task batch. Do not run "loop 1 for ten items, then loop 2 for ten items" as a batch. Run loop 1 and loop 2 for the first element, store its learning, then move to the next element.

When Dontae explicitly defines a design loop as the final rendered screen, the loop unit becomes the full rendered screen, not a QA slice or individual component. A full-screen loop must touch every visible component relationship on each target route, including hierarchy, controls, copy, density, responsive behavior, empty/error states, privacy/provenance cues, and Preline baseline fit. If Dontae asks for 10, 20, or another explicit number of UI design loops, produce that many full rendered-screen loops and PNG proof for each loop before claiming the design phase is handled.

A design element can be a route section, graph panel, menu, collapsible sidebar, filter group, sort control, row-limit control, pagination control, table/list, modal, form, empty state, mobile sheet, or component add/remove decision.

Each element loop contains:

1. Wireframes
2. Mockups & Prototypes
3. Sketching & Wireframing
4. User Interface Design
5. UI / UX Design Layout Testing

Loop 1 should answer structure:

- user job, route, data shape, permissions, provenance, and primary action
- large- and small-viewport wireframes
- Preline block/template/plugin baseline
- rough interaction flow and empty/error/review states
- first layout test for scanability, touch targets, density, and responsive collapse

Loop 2 should answer fidelity:

- mockup or Figma prototype, or equivalent rendered concept when Figma is not the right surface
- final reference lock and decision ledger
- component inventory, design tokens, icon/media/data-viz plan, and copy hierarchy
- rendered layout test across large and small viewports
- accessibility, contrast, focus, reduced motion, text wrapping, overflow, and visual drift checks

Run more loops when the first two still show drift, weak hierarchy, inaccessible states, generic AI styling, unclear provenance, or unresolved implementation risk.

## Loop Regression Gate

More loops are not the default success path. Before a third or later loop, record why another loop is expected to create new evidence instead of reworking taste. If the loop repeats the same failure, broadens scope, burns review time without better validation, or weakens provenance, privacy, or workflow clarity, regress it.

Regression means one of:

- restore or preserve the last validated baseline and mark the current hypothesis rejected
- narrow the work to one failing element/control with a new acceptance check
- convert the blocker into a bounded Council issue with owner, evidence, and validation-after-unblock
- stop the design loop and ask Visionary, Infra, Closer, or Benefactor for the decision they own

Benefactor owns resource-waste review when repeated UI loops consume material time, token/provider usage, hosted resources, or maintenance burden without improving the user outcome. Closer owns the sequence after regression; the interaction-control record must say what was reverted, preserved, rejected, and what proof would justify reopening the loop.

## Interaction-Control Evidence Loop

For product surfaces with menus, collapsible sidebars, filters, sorts, row limits, pagination, add/remove component decisions, or dense operational lists, each loop must leave an inspectable control record.

Record:

- element/control name and owner
- baseline screenshot or browser note before changes
- loop 1 output, loop 2 output, and the delta between them
- changed controls, removed components, added components, and preserved components
- expected user outcome for each control change
- what worked, what did not work, and what was rejected
- large- and small-viewport proof, including menu/collapse open states when relevant
- validation commands and any residual UX risk

Refero owns the reference lock and concrete interaction pattern research. Visionary owns whether the product/design thesis is right. Infra owns whether the implementation path, tool availability, and validation packet are credible.

## Copy And Language

Treat user-facing copy as an interaction control. When a screen, handoff, empty state, error, or agent response protects user files, settings, privacy, permissions, spend, or authority, write the plain-language version first. The copy should name the object in human terms, explain why it matters, state the protective action, and offer the safe next step. Technical labels, paths, IDs, and commands can appear after that only when they help the user inspect or verify the action.

## Mobile Review Artifacts

Dontae needs PNGs for design choices because local URLs, HTML decks, browser sessions, Markdown links, and Figma links may not be viewable from mobile. Any design-choice handoff must include PNG evidence before asking for review.

Use:

- a labeled PNG contact sheet when comparing multiple options
- individual PNGs for each major option or state
- large- and small-viewport PNGs when responsive behavior is part of the decision
- visible labels or adjacent captions so the choice is understandable from the PNG alone

Do not present visual design options as text-only choices when a rendered PNG can be produced. Links may accompany the PNGs, but they do not replace them.

## Approval And Handoff

After the loops:

1. Ask Visionary to approve the product/design thesis with Council guidance.
2. Ask Infra to confirm implementation constraints, tool availability, Figma/Preline/Next.js fit, and validation gates.
3. Update handoff when the task changes project state or release order.
4. Use `ghosthunter` to remove stale labels, deprecated design language, copied reference residue, and agent-theater framing from active docs.
5. Use `clean` to classify dirty files, validate, and prepare a focused commit. Push, deploy, production `/skills` publication, hosted mutations, destructive cleanup, and paid/resource-risk actions remain explicit gates.

When Design is called directly, Design owns the UI/UX evidence while activating logical dependencies automatically. Pull Visionary for product thesis, Infra for implementation and tool proof, Benefactor for loop/resource caps, Values for privacy/trust cues, Stabilizer for cognitive-load risk, Closer/Clean for handoff and release sweep, Ghosthunter for stale design residue, Learn for durable lessons, and Superpowers for structured design-build-verify collaboration.

## Tool Routing

- Figma MCP: exact Figma file/node inspection, editable mockups, prototypes, FigJam diagrams, slide decks, design-system asset search, and design-to-code context.
- Refero: research styles, screens, and flows; create a reference lock and decision ledger before implementation.
- Build Web Apps: concept-first frontend surfaces, Next.js/React implementation, Browser/IAB verification, screenshots, and visual QA.
- Build Web Data Visualization: chart choice, maps, graph layouts, dashboard evidence, timelines, visual encoding, accessibility, and visualization QA.
- Firecrawl or web search: current external design/product research; store scratch output under `.firecrawl/` when using Firecrawl and do not commit it.
- Manual or official virtual-PC review: use only outside memor.re when a local app or dashboard must be inspected directly; keep Design evidence in repo, Figma, and rendered artifacts.

## Quality Gates

Before production movement, confirm:

- two design loops completed and recorded
- interaction-control evidence recorded when menus, collapses, filters, sorts, row limits, pagination, or component add/remove choices changed
- PNG contact sheets or individual PNGs were provided for design choices, with large- and small-viewport PNGs when responsive behavior matters
- signed-in product surfaces have authenticated rendered proof before being called visually verified; Dontae has standing approval for local Clerk development auth env sync in temporary memor.re worktrees for design QA only, using `npm --prefix apps/web run env:sync:clerk-local` with `MEMOR_RE_APPROVE_LOCAL_SECRET_COPY=1`
- major choices trace to user context, memor.re canon, Refero/Figma/design research, or product evidence
- large- and small-viewport states were tested
- provenance, privacy, permission, sync, conflict, review, and rollback states are visible when relevant
- Preline was used before bespoke UI, except for graph/data primitives that Preline does not cover
- data visualizations use truthful encodings and visible caveats
- no nested cards, decorative blobs, purple-first AI styling, glassmorphism, graph wallpaper, agent mascots, or generic dashboard filler slipped in
- Visionary approval and Infra validation packet are ready for handoff

## Final Report

Report:

- loop artifacts completed
- PNG artifacts provided for design choices
- references and source canon used
- Figma/Refero/Build Web Apps/Data Viz tools used or why they were not needed
- Visionary and Infra approval status
- layout tests and viewports checked
- blockers, intentional deviations, and next production gate
