# Figma Plugin API Cookbook

Executable patterns for slide deck work. All code runs inside `use_figma` `execute_code`.
Every block is copy-paste ready — add project-specific IDs/values at the top.

---

## Constants Block — Paste First in Every Session

```javascript
const SLOT_W = 2040;     // frame pitch (1920 + 120 gap)
const SLIDE_W = 1920;
const SLIDE_H = 1080;

const C = {
  NEAR:  {r: 0.039, g: 0.039, b: 0.051},
  BAR:   {r: 0.090, g: 0.090, b: 0.110},
  MAG:   {r: 0.886, g: 0.000, b: 0.345},
  GOLD:  {r: 0.682, g: 0.616, b: 0.345},
  CREAM: {r: 0.980, g: 0.973, b: 0.957},
  WHITE: {r: 1.000, g: 1.000, b: 1.000},
  BLACK: {r: 0.000, g: 0.000, b: 0.000},
};

const FONTS = {
  HEADLINE: {family: 'Playfair Display', style: 'Bold'},
  DISPLAY:  {family: 'Playfair Display', style: 'Black Italic'},
  BODY:     {family: 'Inter', style: 'Regular'},
  MEDIUM:   {family: 'Inter', style: 'Medium'},
  SEMIBOLD: {family: 'Inter', style: 'SemiBold'},
  BOLD:     {family: 'Inter', style: 'Bold'},
};
```

---

## Frame Operations

### Get frame by slot number
```javascript
const getFrameBySlot = (slot) =>
  figma.currentPage.findOne(n =>
    n.type === 'FRAME' && Math.abs(n.x - (slot - 1) * SLOT_W) < 5
  );
```

### Get frame by node ID (preferred after index is loaded)
```javascript
const getFrameById = (id) => {
  const node = figma.getNodeById(id);
  return (node && node.type === 'FRAME') ? node : undefined;
};
```

### Create a new 1920×1080 frame at slot N
```javascript
async function createFrame(slot, name) {
  const f = figma.createFrame();
  f.name = name;
  f.resize(SLIDE_W, SLIDE_H);
  f.x = (slot - 1) * SLOT_W;
  f.y = 0;
  f.fills = [{type: 'SOLID', color: C.NEAR}];
  figma.currentPage.appendChild(f);
  return f;
}
```

### List all frames in slot order
```javascript
const frames = figma.currentPage.children
  .filter(n => n.type === 'FRAME')
  .sort((a, b) => a.x - b.x)
  .map((f, i) => ({slot: i + 1, id: f.id, name: f.name, x: f.x}));
return JSON.stringify(frames, null, 2);
```

---

## Node Inventory

### All direct children of a frame (ground-truth before any positional work)
```javascript
function frameInventory(frameId) {
  const frame = figma.getNodeById(frameId);
  return frame.children.map(n => ({
    id: n.id, name: n.name, type: n.type,
    x: Math.round(n.x), y: Math.round(n.y),
    w: Math.round(n.width), h: Math.round(n.height),
  }));
}
return JSON.stringify(frameInventory('FRAME_ID'), null, 2);
```

### Find all TEXT nodes in a frame
```javascript
const frame = figma.getNodeById('FRAME_ID');
const textNodes = frame.findAll(n => n.type === 'TEXT')
  .map(n => ({id: n.id, name: n.name, text: n.characters.slice(0, 80), size: n.fontSize}));
return JSON.stringify(textNodes, null, 2);
```

### Find node by name (partial match)
```javascript
const node = figma.currentPage.findOne(n => n.name.includes('SEARCH_TERM'));
return node ? {id: node.id, name: node.name, type: node.type} : 'not found';
```

---

## Text Operations

### Surgical text edit (copy change only — do NOT rebuild for this)
```javascript
async function setText(nodeId, newText) {
  const node = figma.getNodeById(nodeId);
  if (!node || node.type !== 'TEXT') throw new Error(`No TEXT at ${nodeId}`);
  await figma.loadFontAsync(node.fontName);
  node.characters = newText;
  return {id: node.id, name: node.name, newText};
}
return await setText('TEXT_NODE_ID', 'Replacement copy here');
```

### Create a text node with full style spec
```javascript
async function makeText(parent, {text, font, size, color, x, y, w, align = 'LEFT', name = 'text'}) {
  await figma.loadFontAsync(font);
  const t = figma.createText();
  t.name = name;
  t.fontName = font;
  t.fontSize = size;
  t.characters = text;
  t.fills = [{type: 'SOLID', color}];
  t.textAlignHorizontal = align;
  if (w) { t.textAutoResize = 'HEIGHT'; t.resize(w, 40); }
  t.x = x; t.y = y;
  parent.appendChild(t);
  return t;
}
```

### Batch text edit across multiple nodes
```javascript
const edits = [
  {id: 'NODE_A', text: 'New headline copy'},
  {id: 'NODE_B', text: 'New body copy'},
];
for (const {id, text} of edits) {
  const node = figma.getNodeById(id);
  if (node?.type === 'TEXT') {
    await figma.loadFontAsync(node.fontName);
    node.characters = text;
  }
}
return `Updated ${edits.length} nodes`;
```

---

## Rectangle / Card Operations

### Create a solid-fill rectangle
```javascript
function makeRect(parent, {x, y, w, h, color, name = 'rect', radius = 0}) {
  const r = figma.createRectangle();
  r.name = name;
  r.x = x; r.y = y;
  r.resize(w, h);
  r.fills = [{type: 'SOLID', color}];
  r.cornerRadius = radius;
  parent.appendChild(r);
  return r;
}
```

### Create a card (BAR background + rounded corners)
```javascript
function makeCard(parent, {x, y, w, h, name = 'card'}) {
  return makeRect(parent, {x, y, w, h, color: C.BAR, name, radius: 8});
}
```

### Create a horizontal rule (magenta accent line)
```javascript
function makeRule(parent, {x, y, w, color = C.MAG, name = 'rule'}) {
  return makeRect(parent, {x, y, w, h: 2, color, name});
}
```

---

## Group / Frame Operations

### Wrap children in a group
```javascript
function groupNodes(nodes, parent, name) {
  const g = figma.group(nodes, parent);
  g.name = name;
  return g;
}
```

### Create a named sub-frame (for card slots, column containers)
```javascript
function makeSubFrame(parent, {x, y, w, h, color, name}) {
  const f = figma.createFrame();
  f.name = name;
  f.resize(w, h);
  f.x = x; f.y = y;
  f.fills = color ? [{type: 'SOLID', color}] : [];
  f.clipsContent = false;
  parent.appendChild(f);
  return f;
}
```

---

## Color Fills

### Solid fill helper
```javascript
const solid = (color, opacity = 1) => [{type: 'SOLID', color, opacity}];
```

### Gradient fill (top-to-bottom)
```javascript
const gradientFill = (topColor, bottomColor) => [{
  type: 'GRADIENT_LINEAR',
  gradientTransform: [[0, 1, 0], [-1, 0, 1]],
  gradientStops: [
    {position: 0, color: {...topColor, a: 1}},
    {position: 1, color: {...bottomColor, a: 1}},
  ],
}];
```

---

## Snapshot and Export

### Clone current page as checkpoint (run before bulk ops)
```javascript
function checkpoint(label) {
  const src = figma.currentPage;
  const snap = figma.duplicatePage(src);
  snap.name = `Checkpoint — ${label}`;
  figma.currentPage = src;
  return snap.name;
}
return checkpoint('pre-Council-rev');
```

### Get screenshot of a specific frame
```
// Via MCP tool:
get_screenshot(fileKey, nodeId)
```

### Export page to PDF
```
// Via MCP tool:
download_assets(fileKey, "0:1", "pdf")
```

---

## Validation Helpers

### Type-size audit (returns violations, empty array = pass)
```javascript
function auditTypeSizes(frameId) {
  const frame = figma.getNodeById(frameId);
  const violations = [];
  frame.findAll(n => n.type === 'TEXT').forEach(n => {
    const sz = n.fontSize;
    const nm = n.name.toLowerCase();
    if ((nm.includes('headline') || nm.includes('title')) && sz < 54)
      violations.push({node: n.name, id: n.id, size: sz, rule: '>= 54'});
    if (nm.includes('sub') && sz < 24)
      violations.push({node: n.name, id: n.id, size: sz, rule: '>= 24'});
    if ((nm.includes('body') || nm.includes('copy')) && sz < 21)
      violations.push({node: n.name, id: n.id, size: sz, rule: '>= 21'});
    if ((nm.includes('chip') || nm.includes('tag') || nm.includes('label')) && sz < 18)
      violations.push({node: n.name, id: n.id, size: sz, rule: '>= 18'});
  });
  return violations;
}
return JSON.stringify(auditTypeSizes('FRAME_ID'), null, 2);
```

### Collision check — chips vs card bottom edge
```javascript
function checkCollisions(frameId) {
  const frame = figma.getNodeById(frameId);
  const cards = frame.findAll(n => n.name.toLowerCase().includes('card'));
  const chips = frame.findAll(n => n.name.toLowerCase().includes('chip') || n.name.toLowerCase().includes('pill'));
  const hits = [];
  chips.forEach(chip => {
    const chipBounds = chip.absoluteBoundingBox;
    if (!chipBounds) return;
    cards.forEach(card => {
      const cardBounds = card.absoluteBoundingBox;
      if (!cardBounds) return;
      const chipBottom = chipBounds.y + chipBounds.height;
      const cardBottom = cardBounds.y + cardBounds.height;
      if (chipBounds.y < cardBottom && chipBottom > cardBounds.y && Math.abs(chipBottom - cardBottom) < 16)
        hits.push({chip: chip.name, card: card.name, overlap: chipBottom - cardBottom});
    });
  });
  return hits;
}
return JSON.stringify(checkCollisions('FRAME_ID'), null, 2);
```

### Empty lower-third check
```javascript
// Returns frames where nothing exists below y=720 (lower third of 1080 slide)
function auditLowerThird(frameIds) {
  return frameIds.filter(id => {
    const frame = figma.getNodeById(id);
    const nodes = frame.findAll(n => {
      const bounds = n.absoluteBoundingBox;
      return bounds ? bounds.y > 720 : n.y > 720;
    });
    return nodes.length === 0;
  });
}
```
