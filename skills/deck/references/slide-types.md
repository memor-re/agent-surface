# Slide Type Builders

Concrete builder code for each slide type. Every builder assumes the constants block
from `figma-api.md` is already loaded. Adapt x/y/w/h to your brand kit's margin system.

---

## Layout Constants (adjust per brand)

```javascript
const M = 80;           // margin (left/right)
const T = 80;           // top margin for content
const COL_GAP = 32;     // gap between columns
const CARD_R = 8;       // card corner radius
const RULE_H = 2;       // magenta rule thickness
const INNER = 32;       // padding inside cards
```

---

## manifesto — Full-Bleed Statement Slide

Use for: POV statements, section openers, deck close.
One sentence. Playfair. Big. Centered.

```javascript
async function buildManifesto(slot, {headline, subline = '', bgColor = C.MAG, textColor = C.CREAM}) {
  const frame = getFrame(slot);
  for (const child of [...frame.children]) child.remove();
  frame.fills = [{type: 'SOLID', color: bgColor}];

  const headlineW = SLIDE_W - M * 4;

  await figma.loadFontAsync(FONTS.DISPLAY);
  if (subline) await figma.loadFontAsync(FONTS.MEDIUM);

  const hl = figma.createText();
  hl.name = 'headline';
  hl.fontName = FONTS.DISPLAY;
  hl.fontSize = 62;
  hl.lineHeight = {value: 72, unit: 'PIXELS'};
  hl.textAlignHorizontal = 'CENTER';
  hl.textAutoResize = 'HEIGHT';
  hl.resize(headlineW, 100);
  hl.characters = headline;
  hl.fills = [{type: 'SOLID', color: textColor}];
  hl.x = (SLIDE_W - headlineW) / 2;
  hl.y = subline ? 360 : 420;
  frame.appendChild(hl);

  if (subline) {
    const sl = figma.createText();
    sl.name = 'subline';
    sl.fontName = FONTS.MEDIUM;
    sl.fontSize = 24;
    sl.textAlignHorizontal = 'CENTER';
    sl.textAutoResize = 'HEIGHT';
    sl.resize(headlineW, 40);
    sl.characters = subline;
    sl.fills = [{type: 'SOLID', color: {...textColor, a: 0.72}}];
    sl.x = hl.x;
    sl.y = hl.y + hl.height + 24;
    frame.appendChild(sl);
  }

  return `manifesto built at slot ${slot}`;
}
```

---

## stat — 2×2 Number Grid

Use for: market proof, 4 data points with payoff lines.
Playfair Black for numbers. Inter body below each.

```javascript
async function buildStatGrid(slot, {title, stats, payoff = ''}) {
  // stats: [{number: '#1', label: 'in paid social reach'}] × 4
  const frame = getFrame(slot);
  for (const child of [...frame.children]) child.remove();

  await figma.loadFontAsync(FONTS.HEADLINE);
  await figma.loadFontAsync(FONTS.DISPLAY);
  await figma.loadFontAsync(FONTS.BODY);
  await figma.loadFontAsync(FONTS.MEDIUM);

  const titleNode = figma.createText();
  titleNode.name = 'title';
  titleNode.fontName = FONTS.MEDIUM;
  titleNode.fontSize = 18;
  titleNode.characters = title.toUpperCase();
  titleNode.letterSpacing = {value: 2, unit: 'PIXELS'};
  titleNode.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.5}}];
  titleNode.x = M; titleNode.y = T;
  frame.appendChild(titleNode);

  const hRule = figma.createRectangle();
  hRule.name = 'h-divider';
  hRule.x = M; hRule.y = SLIDE_H / 2 - 1;
  hRule.resize(SLIDE_W - M * 2, 1);
  hRule.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.12}}];
  frame.appendChild(hRule);

  const vRule = figma.createRectangle();
  vRule.name = 'v-divider';
  vRule.x = SLIDE_W / 2 - 1; vRule.y = T + 60;
  vRule.resize(1, SLIDE_H - T - 60 - 120);
  vRule.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.12}}];
  frame.appendChild(vRule);

  const positions = [
    {col: 0, row: 0}, {col: 1, row: 0},
    {col: 0, row: 1}, {col: 1, row: 1},
  ];
  const qW = (SLIDE_W - M * 2) / 2;
  const qH = (SLIDE_H - T - 60 - 120) / 2;

  for (let i = 0; i < Math.min(stats.length, 4); i++) {
    const {col, row} = positions[i];
    const qX = M + col * qW;
    const qY = T + 60 + row * qH;
    const {number, label} = stats[i];

    const num = figma.createText();
    num.name = `stat-${i}-number`;
    num.fontName = FONTS.DISPLAY;
    num.fontSize = 80;
    num.characters = number;
    num.fills = [{type: 'SOLID', color: C.CREAM}];
    num.x = qX + INNER; num.y = qY + INNER;
    frame.appendChild(num);

    const lbl = figma.createText();
    lbl.name = `stat-${i}-label`;
    lbl.fontName = FONTS.BODY;
    lbl.fontSize = 21;
    lbl.lineHeight = {value: 28, unit: 'PIXELS'};
    lbl.characters = label;
    lbl.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.72}}];
    lbl.textAutoResize = 'HEIGHT';
    lbl.resize(qW - INNER * 2, 40);
    lbl.x = qX + INNER; lbl.y = qY + INNER + 96;
    frame.appendChild(lbl);
  }

  if (payoff) {
    const pay = figma.createText();
    pay.name = 'payoff';
    pay.fontName = FONTS.DISPLAY;
    pay.fontSize = 24;
    pay.textAlignHorizontal = 'CENTER';
    pay.textAutoResize = 'HEIGHT';
    pay.resize(SLIDE_W - M * 2, 40);
    pay.characters = payoff;
    pay.fills = [{type: 'SOLID', color: C.GOLD}];
    pay.x = M; pay.y = SLIDE_H - 100;
    frame.appendChild(pay);
  }

  return `stat grid built at slot ${slot}`;
}
```

---

## card-cols — 3-Column Card Layout

Use for: parallel pillars, roles, features, job descriptions.
Each card: BAR bg, magenta rule, bold label, body, substance line (SO WHAT / EXAMPLE / WHO).

```javascript
async function buildCardCols(slot, {headline, cards}) {
  // cards: [{label, body, tag, substance}] — tag = 'SO WHAT' | 'EXAMPLE' | 'IN PRACTICE' | 'TEMPO' | 'WHO'
  if (!cards || cards.length !== 3) throw new Error(`buildCardCols requires exactly 3 cards, got ${cards?.length ?? 0}`);
  const frame = getFrame(slot);
  for (const child of [...frame.children]) child.remove();

  await figma.loadFontAsync(FONTS.HEADLINE);
  await figma.loadFontAsync(FONTS.SEMIBOLD);
  await figma.loadFontAsync(FONTS.BODY);
  await figma.loadFontAsync(FONTS.MEDIUM);

  const hl = figma.createText();
  hl.name = 'headline';
  hl.fontName = FONTS.HEADLINE;
  hl.fontSize = 58;
  hl.lineHeight = {value: 66, unit: 'PIXELS'};
  hl.characters = headline;
  hl.fills = [{type: 'SOLID', color: C.CREAM}];
  hl.x = M; hl.y = T;
  frame.appendChild(hl);

  const N = cards.length;
  const cardW = Math.floor((SLIDE_W - M * 2 - COL_GAP * (N - 1)) / N);
  const cardY = T + 66 + 40;
  const cardH = SLIDE_H - cardY - M;

  for (let i = 0; i < N; i++) {
    const {label, body, tag, substance} = cards[i];
    const cardX = M + i * (cardW + COL_GAP);

    const bg = figma.createRectangle();
    bg.name = `card-${i}-bg`;
    bg.x = cardX; bg.y = cardY;
    bg.resize(cardW, cardH);
    bg.fills = [{type: 'SOLID', color: C.BAR}];
    bg.cornerRadius = CARD_R;
    frame.appendChild(bg);

    const rule = figma.createRectangle();
    rule.name = `card-${i}-rule`;
    rule.x = cardX + INNER; rule.y = cardY + INNER;
    rule.resize(48, RULE_H);
    rule.fills = [{type: 'SOLID', color: C.MAG}];
    frame.appendChild(rule);

    const lbl = figma.createText();
    lbl.name = `card-${i}-label`;
    lbl.fontName = FONTS.SEMIBOLD;
    lbl.fontSize = 24;
    lbl.characters = label;
    lbl.fills = [{type: 'SOLID', color: C.CREAM}];
    lbl.x = cardX + INNER; lbl.y = cardY + INNER + RULE_H + 16;
    frame.appendChild(lbl);

    const bd = figma.createText();
    bd.name = `card-${i}-body`;
    bd.fontName = FONTS.BODY;
    bd.fontSize = 21;
    bd.lineHeight = {value: 30, unit: 'PIXELS'};
    bd.characters = body;
    bd.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.72}}];
    bd.textAutoResize = 'HEIGHT';
    bd.resize(cardW - INNER * 2, 100);
    bd.x = cardX + INNER; bd.y = cardY + INNER + RULE_H + 16 + 32;
    frame.appendChild(bd);

    if (tag && substance) {
      const tagNode = figma.createText();
      tagNode.name = `card-${i}-tag`;
      tagNode.fontName = FONTS.SEMIBOLD;
      tagNode.fontSize = 12;
      tagNode.letterSpacing = {value: 1.5, unit: 'PIXELS'};
      tagNode.characters = tag;
      tagNode.fills = [{type: 'SOLID', color: C.GOLD}];
      tagNode.x = cardX + INNER; tagNode.y = cardY + cardH - 80;
      frame.appendChild(tagNode);

      const subNode = figma.createText();
      subNode.name = `card-${i}-substance`;
      subNode.fontName = FONTS.MEDIUM;
      subNode.fontSize = 18;
      subNode.lineHeight = {value: 24, unit: 'PIXELS'};
      subNode.characters = substance;
      subNode.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.85}}];
      subNode.textAutoResize = 'HEIGHT';
      subNode.resize(cardW - INNER * 2, 50);
      subNode.x = cardX + INNER; subNode.y = cardY + cardH - 60;
      frame.appendChild(subNode);
    }
  }

  return `card-cols (${N} cards) built at slot ${slot}`;
}
```

---

## from-to — Three-Row Shift Diagram

Use for: before/after, old model vs new model. Never use negation in the "from" column.

```javascript
async function buildFromTo(slot, {headline, rows}) {
  // rows: [{from: 'old state', to: 'new state'}] × 3
  if (!rows || rows.length !== 3) throw new Error(`buildFromTo requires exactly 3 rows, got ${rows?.length ?? 0}`);
  const frame = getFrame(slot);
  for (const child of [...frame.children]) child.remove();

  await figma.loadFontAsync(FONTS.HEADLINE);
  await figma.loadFontAsync(FONTS.BODY);
  await figma.loadFontAsync(FONTS.MEDIUM);
  await figma.loadFontAsync(FONTS.SEMIBOLD);

  const hl = figma.createText();
  hl.name = 'headline';
  hl.fontName = FONTS.HEADLINE;
  hl.fontSize = 58;
  hl.characters = headline;
  hl.fills = [{type: 'SOLID', color: C.CREAM}];
  hl.x = M; hl.y = T;
  frame.appendChild(hl);

  const headerY = T + 80;
  for (const [label, x, color] of [
    ['FROM', M, {...C.CREAM, a: 0.4}],
    ['TO', SLIDE_W / 2 + 40, C.MAG],
  ]) {
    const h = figma.createText();
    h.fontName = FONTS.SEMIBOLD;
    h.fontSize = 12;
    h.letterSpacing = {value: 2, unit: 'PIXELS'};
    h.characters = label;
    h.fills = [{type: 'SOLID', color}];
    h.x = x; h.y = headerY;
    frame.appendChild(h);
  }

  const arrow = figma.createRectangle();
  arrow.x = SLIDE_W / 2 - 1; arrow.y = headerY;
  arrow.resize(1, SLIDE_H - headerY - M);
  arrow.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.15}}];
  frame.appendChild(arrow);

  const rowH = (SLIDE_H - headerY - 40 - M) / rows.length;
  rows.forEach(({from, to}, i) => {
    const rowY = headerY + 40 + i * rowH;

    const fromNode = figma.createText();
    fromNode.fontName = FONTS.BODY;
    fromNode.fontSize = 22;
    fromNode.lineHeight = {value: 30, unit: 'PIXELS'};
    fromNode.characters = from;
    fromNode.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.5}}];
    fromNode.textAutoResize = 'HEIGHT';
    fromNode.resize(SLIDE_W / 2 - M - 40, 80);
    fromNode.x = M; fromNode.y = rowY;
    frame.appendChild(fromNode);

    const toNode = figma.createText();
    toNode.fontName = FONTS.MEDIUM;
    toNode.fontSize = 22;
    toNode.lineHeight = {value: 30, unit: 'PIXELS'};
    toNode.characters = to;
    toNode.fills = [{type: 'SOLID', color: C.CREAM}];
    toNode.textAutoResize = 'HEIGHT';
    toNode.resize(SLIDE_W / 2 - 40 - M, 80);
    toNode.x = SLIDE_W / 2 + 40; toNode.y = rowY;
    frame.appendChild(toNode);

    if (i < rows.length - 1) {
      const div = figma.createRectangle();
      div.x = M; div.y = rowY + rowH - 1;
      div.resize(SLIDE_W - M * 2, 1);
      div.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.08}}];
      frame.appendChild(div);
    }
  });

  return `from-to built at slot ${slot}`;
}
```

---

## anchor — Dense Architecture / Stack Diagram

Use for: POSE stack, operating system diagrams, layered architectures.
Rule: ~1 anchor per 6 slides total. Anchors have labeled rows, chip rows, legend.

```javascript
async function buildAnchor(slot, {title, layers, legend = []}) {
  // layers: [{label, color, chips: ['chip1','chip2',...], note}]
  // legend: [{color, label}]
  if (!layers || layers.length === 0) throw new Error('buildAnchor requires at least one layer');
  const frame = getFrame(slot);
  for (const child of [...frame.children]) child.remove();

  await figma.loadFontAsync(FONTS.SEMIBOLD);
  await figma.loadFontAsync(FONTS.MEDIUM);
  await figma.loadFontAsync(FONTS.BODY);

  const layerH = Math.floor((SLIDE_H - T - M - 40) / layers.length);
  const chipH = 28;

  layers.forEach(({label, color, chips, note}, i) => {
    const ly = T + 40 + i * layerH;

    const bar = figma.createRectangle();
    bar.name = `layer-${i}-bar`;
    bar.x = M; bar.y = ly;
    bar.resize(SLIDE_W - M * 2 - (legend.length ? 200 : 0), layerH - 8);
    bar.fills = [{type: 'SOLID', color}];
    bar.cornerRadius = 4;
    frame.appendChild(bar);

    const lbl = figma.createText();
    lbl.name = `layer-${i}-label`;
    lbl.fontName = FONTS.SEMIBOLD;
    lbl.fontSize = 20;
    lbl.characters = label;
    lbl.fills = [{type: 'SOLID', color: C.CREAM}];
    lbl.x = M + INNER; lbl.y = ly + INNER;
    frame.appendChild(lbl);

    let chipX = M + INNER;
    const chipY = ly + INNER + 32;
    chips.forEach(chip => {
      const chipBg = figma.createRectangle();
      const chipW = chip.length * 8 + 24;
      chipBg.x = chipX; chipBg.y = chipY;
      chipBg.resize(chipW, chipH);
      chipBg.fills = [{type: 'SOLID', color: {...C.NEAR, a: 0.6}}];
      chipBg.cornerRadius = chipH / 2;
      frame.appendChild(chipBg);

      const chipLabel = figma.createText();
      chipLabel.fontName = FONTS.MEDIUM;
      chipLabel.fontSize = 13;
      chipLabel.characters = chip;
      chipLabel.fills = [{type: 'SOLID', color: C.CREAM}];
      chipLabel.x = chipX + 12; chipLabel.y = chipY + 7;
      frame.appendChild(chipLabel);

      chipX += chipW + 8;
    });

    if (note) {
      const noteNode = figma.createText();
      noteNode.fontName = FONTS.BODY;
      noteNode.fontSize = 16;
      noteNode.characters = note;
      noteNode.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.5}}];
      noteNode.x = M + INNER; noteNode.y = ly + layerH - 36;
      frame.appendChild(noteNode);
    }
  });

  if (legend.length) {
    const lX = SLIDE_W - M - 180;
    legend.forEach(({color: lc, label: ll}, i) => {
      const dot = figma.createRectangle();
      dot.x = lX; dot.y = T + 40 + i * 32;
      dot.resize(12, 12);
      dot.fills = [{type: 'SOLID', color: lc}];
      dot.cornerRadius = 6;
      frame.appendChild(dot);

      const lbl = figma.createText();
      lbl.fontName = FONTS.BODY;
      lbl.fontSize = 16;
      lbl.characters = ll;
      lbl.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.6}}];
      lbl.x = lX + 20; lbl.y = T + 40 + i * 32;
      frame.appendChild(lbl);
    });
  }

  const titleNode = figma.createText();
  titleNode.name = 'title';
  titleNode.fontName = FONTS.SEMIBOLD;
  titleNode.fontSize = 18;
  titleNode.letterSpacing = {value: 1.5, unit: 'PIXELS'};
  titleNode.characters = title.toUpperCase();
  titleNode.fills = [{type: 'SOLID', color: {...C.CREAM, a: 0.4}}];
  titleNode.x = M; titleNode.y = T;
  frame.appendChild(titleNode);

  return `anchor (${layers.length} layers) built at slot ${slot}`;
}
```
