---
name: deck
description: "Presentation deck builder — Figma plugin API, slide-type templates, index schema, quality gates. Invoke before any deck session. Works with any client, any brand, any Figma file."
argument-hint: "<figma-file-key or 'new'> [brand-kit-name]"
---

<!-- memor-re-durable-execution-contract:v1
{
  "agentId": "deck",
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
  "authoritySurface": "docs/architecture/canonical_agent_governance_registry.json#agents.deck",
  "journalSurface": "hosted memory ledger (memory_list_recent + memory_search) is the orientation source of truth; docs/operations/* evidence packets and PR/check evidence are the proof surface. Host-app shell directories are never orientation or journal authority.",
  "idempotencyKey": "agent:deck:active-slice:authority-surface:validation-command",
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
  "owner": "Council"
}
-->

# /deck

Presentation deck craft for Figma. Every rule here is a hard gate, not a suggestion.
Prose belongs in the speaker notes. Code belongs on the slide.

## Session Start — Run Before Anything Else

```
☐ 1. Confirm Figma file key (from URL or project memory)
☐ 2. Load or define brand-kit constants (see references/brand-kit.md)
☐ 3. Read or create the living index (CSV — see references/index-schema.md)
☐ 4. Get POV line approved before building any slide
☐ 5. Run CHECKPOINT_PRE before any bulk operation
```

Do not skip step 4. A deck without an approved POV line will require rework on every slide.

---

## Figma Plugin API — Core Patterns

All Figma mutations run via `use_figma` with `execute_code`. Load the full reference at
`references/figma-api.md`. The patterns below are the minimum every deck session needs.

### Brand Color Constants

```javascript
// Copy into every plugin execution block — do NOT hardcode hex strings
const C = {
  NEAR:  {r: 0.039, g: 0.039, b: 0.051},  // #0A0A0D  dark bg
  BAR:   {r: 0.090, g: 0.090, b: 0.110},  // ~#171719 card bg
  MAG:   {r: 0.886, g: 0.000, b: 0.345},  // #E20058  magenta accent
  GOLD:  {r: 0.682, g: 0.616, b: 0.345},  // #AE9D58  gold accent
  CREAM: {r: 0.980, g: 0.973, b: 0.957},  // #F9F8F4  primary text
  WHITE: {r: 1.000, g: 1.000, b: 1.000},
  BLACK: {r: 0.000, g: 0.000, b: 0.000},
};
const ALPHA = (color, a) => ({...color, a});  // add opacity
```

### Frame Addressing

```javascript
// Slides are 1920×1080 frames at x = (slot - 1) * 2040, y = 0
const SLOT_W = 2040;
const getFrame = (slot) =>
  figma.currentPage.findOne(n => n.type === 'FRAME' && Math.abs(n.x - (slot - 1) * SLOT_W) < 5);

// By node ID (preferred after index is loaded)
const getNode = (id) => figma.getNodeById(id);
```

### Surgical Text Edit — Use This for Copy Changes

```javascript
// Never rebuild a slide to change words. Find the node, load the font, set characters.
async function setText(nodeId, newText) {
  const node = figma.getNodeById(nodeId);
  if (!node || node.type !== 'TEXT') throw new Error(`No TEXT at ${nodeId}`);
  await figma.loadFontAsync(node.fontName);
  node.characters = newText;
  return node.name;
}
```

### Ground-Truth Node Query — Use Before Any Positional Arithmetic

```javascript
// List every direct child of a frame with type, name, x, y, w, h
function frameInventory(frameId) {
  const frame = figma.getNodeById(frameId);
  return frame.children.map(n => ({
    id: n.id, name: n.name, type: n.type,
    x: Math.round(n.x), y: Math.round(n.y),
    w: Math.round(n.width), h: Math.round(n.height),
  }));
}
```

### Type-Size Validator

```javascript
// Run on any frame before calling it done
function auditTypeSizes(frameId) {
  const frame = figma.getNodeById(frameId);
  const violations = [];
  frame.findAll(n => n.type === 'TEXT').forEach(n => {
    const sz = n.fontSize;
    const nm = n.name.toLowerCase();
    if ((nm.includes('headline') || nm.includes('title')) && sz < 54)
      violations.push({node: n.name, size: sz, rule: 'headline >= 54'});
    if (nm.includes('sub') && sz < 24)
      violations.push({node: n.name, size: sz, rule: 'sub >= 24'});
    if ((nm.includes('body') || nm.includes('text')) && sz < 21)
      violations.push({node: n.name, size: sz, rule: 'body >= 21'});
    if ((nm.includes('chip') || nm.includes('tag') || nm.includes('label')) && sz < 18)
      violations.push({node: n.name, size: sz, rule: 'chip >= 18'});
  });
  return violations;
}
// Usage: return auditTypeSizes('FRAME_NODE_ID');
```

### Snapshot Page (Checkpoint)

```javascript
// Clone current page as a frozen snapshot before any bulk operation
function snapshotPage(label) {
  const srcPage = figma.currentPage;
  const snap = figma.duplicatePage(srcPage);
  snap.name = `Checkpoint — ${label}`;
  figma.currentPage = srcPage; // stay on working page
  return snap.name;
}
```

### PDF Export

```
// Via MCP tool — not plugin code
download_assets(fileKey, "0:1", "pdf")
// fileKey = the string from the Figma URL (?file=XXXXX or /design/XXXXX/)
// "0:1" = page node (first page)
```

---

## Type Scale — Hard Limits

Do not negotiate these. Dark-background legibility comes from size + weight, not color choice.

| Role        | Min size | Preferred | Weight          |
|-------------|----------|-----------|-----------------|
| Headline    | 54       | 58–64     | Bold or Black   |
| Sub-header  | 24       | 26        | SemiBold        |
| Body        | 21       | 22–24     | Regular/Medium  |
| Chip / tag  | 18       | 18–20     | Medium/SemiBold |
| Footnote    | 16       | 16        | Regular         |

**Never go below 16.** If content doesn't fit at 16+, cut content, not type.

---

## Slide Types

See `references/slide-types.md` for full builder code. Quick reference:

| Type         | When to use                                  | Layout pattern                         |
|--------------|----------------------------------------------|----------------------------------------|
| `manifesto`  | POV statement, section opener, close         | Full-bleed color panel, Playfair 60+   |
| `stat`       | Market proof, 2–4 numbers                   | 2×2 grid, Playfair Black numbers       |
| `card-cols`  | 3–4 parallel pillars (features, roles, etc.) | BAR cards, magenta rule, substance row |
| `anchor`     | Dense architecture / stack diagram           | Layered bars + chip rows + legend      |
| `from-to`    | Before/after shift, 3 rows                  | Two columns, left=old, right=new       |
| `flow`       | Sequential steps (1→2→3→N)                  | Horizontal arrow chain                 |
| `deep-dive`  | Single POSE job or topic                     | 3 columns + failure callout row        |
| `cover`      | Title slide                                  | Brand mark, deck title, date/client    |

---

## Index Schema

Maintain a CSV for every deck. Schema enforced — do not add/remove columns.
See `references/index-schema.md` for full spec and row validator.

```
Order,Act,Slide Title,Type,Background,Status,Figma Node ID,Notes
```

| Column        | Type                                                        | Required |
|---------------|-------------------------------------------------------------|----------|
| Order         | integer (1-based, no gaps)                                  | yes      |
| Act           | string ("1 — Case for Change", etc.)                        | yes      |
| Slide Title   | string                                                      | yes      |
| Type          | enum (see slide types above)                                | yes      |
| Background    | enum: Dark \| Cream \| Magenta panel \| Black panel \| Gold panel | yes |
| Status        | enum: Draft \| In progress \| Final                         | yes      |
| Figma Node ID | string (e.g. "112:2") — refresh only at clean checkpoints   | yes      |
| Notes         | string (free text, one line)                                | no       |

**Node IDs change on rebuilds.** Refresh the index at checkpoints, not per-edit.

---

## Copy Rules — Applied at Write Time

These are gates, not guidelines. Fail = rewrite before shipping.

```
☐ Headline IS the conclusion — not the topic. "Creators earn attention, paid takes it to scale" not "Shared Layer"
☐ No em dashes
☐ No "Statement. Punchy fragment." two-sentence headline pattern
☐ No negation framing ("what it's not") — state what it IS or use from→to
☐ No soft verb standing alone: "Listen" → "social listening and cultural intelligence"
☐ No abstract jargon: infrastructure / system / agentic / layer → concrete impact on feeds, trust, attention, culture
☐ Every verb names a real discipline or measurable action
☐ Never denigrate the client's core strength to elevate the new idea
☐ If a layer "fails," the failure is disconnection — not the layer being weak
☐ Ban list: "amplify", "leverage", "unlock", "drive" (unless no precise alternative exists)
```

---

## Quality Gates

### Gate: Before Any Build

```
☐ POV line approved in writing
☐ Brand kit constants confirmed
☐ Index exists (even as stubs)
☐ Snapshot page created (CHECKPOINT_PRE)
```

### Gate: Per Slide

```
☐ Headline makes an argument — not a label
☐ Lower third is not empty (substance line, chip row, or stat grid fills it)
☐ Type sizes pass auditTypeSizes()
☐ No chip/pill collisions with card edges (check y + height of chip vs card y + height)
☐ No "simple diagram saying nothing" — slide has a POV visible in the headline
```

### Gate: Before Calling Deck Done

```
☐ PDF export completes (download_assets) without error
☐ Snapshot page created (CHECKPOINT_POST)
☐ Index refreshed (Order column has no gaps, all Status = Final)
☐ Anchor slides: ~1 per 6 total slides (round down)
☐ Copy sweep: run each copy rule above across every headline
☐ Visual spot-check: screenshot slides 1, midpoint, last — verify brand colors, type sizes
```

---

## Process Rules

**Surgical > Rebuild.** Text change = setText(). Layout change = rebuild. Never mix.

**Ground-truth first.** Run frameInventory() before any positional arithmetic. Wrong slot offset silently clobbers a neighbor.

**When user repeats feedback 2–3×, it is a hard rule.** Encode in memory. Stop regressing.

**Density = more real content, never smaller type.** More chips, tags, notes, layers. Not 16px body copy.

**Cut beats thin slides.** One slide with a strong argument beats three slides with labels.

**Index IDs drift.** Only refresh Figma Node ID column at clean checkpoints (post-PDF, post-snapshot).

---

## Tool Routing

| Task                     | Tool                        |
|--------------------------|-----------------------------|
| Read/edit Figma nodes    | `use_figma` (execute_code)  |
| Export PDF               | `download_assets`           |
| Screenshot a slide       | `get_screenshot`            |
| Search node by name      | `use_figma` + `findAll`     |
| Index file (CSV)         | Write/Edit (local file)     |
| Snapshot page            | `use_figma` + duplicatePage |

---

## References

- `references/figma-api.md` — full Figma plugin API cookbook (node creation, font loading, color fills, group/frame ops, export)
- `references/slide-types.md` — builder code for every slide type (manifesto, stat, card-cols, anchor, flow, etc.)
- `references/index-schema.md` — index CSV spec, row validator, refresh rules
- `references/brand-kit.md` — how to define and lock a brand kit for a new client
