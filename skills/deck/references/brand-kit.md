# Brand Kit Definition

Every deck session starts by locking a brand kit. This file defines the kit schema,
the default lean-dark kit (used for Spark / Publicis dark-brand work), and the process
for defining a new client kit from scratch.

---

## Kit Schema

```javascript
const KIT = {
  name: 'lean-dark',

  colors: {
    bg:      {r: 0.039, g: 0.039, b: 0.051},
    card:    {r: 0.090, g: 0.090, b: 0.110},
    accent1: {r: 0.886, g: 0.000, b: 0.345},
    accent2: {r: 0.682, g: 0.616, b: 0.345},
    text:    {r: 0.980, g: 0.973, b: 0.957},
    muted:   {r: 0.980, g: 0.973, b: 0.957},  // at 0.6–0.72 opacity
  },

  fonts: {
    display:  {family: 'Playfair Display', style: 'Black Italic'},
    headline: {family: 'Playfair Display', style: 'Bold'},
    body:     {family: 'Inter', style: 'Regular'},
    ui:       {family: 'Inter', style: 'Medium'},
    strong:   {family: 'Inter', style: 'SemiBold'},
  },

  scale: {
    headline:  62,
    sub:       26,
    body:      22,
    chip:      18,
    footnote:  16,
  },

  layout: {
    slideW:   1920,
    slideH:   1080,
    slotW:    2040,
    margin:   80,
    cardGap:  32,
    innerPad: 32,
    ruleH:    2,
    cardR:    8,
  },

  panels: {
    manifesto: {r: 0.886, g: 0.000, b: 0.345},
    statement: {r: 0.000, g: 0.000, b: 0.000},
    highlight: {r: 0.682, g: 0.616, b: 0.345},
    light:     {r: 0.961, g: 0.953, b: 0.937},
  },
};
```

---

## Lean-Dark Kit (Default — Spark / Publicis Dark Brand)

Pre-filled constants block. Copy as-is for any dark-brand deck.

```javascript
const C = {
  NEAR:  {r: 0.039, g: 0.039, b: 0.051},
  BAR:   {r: 0.090, g: 0.090, b: 0.110},
  MAG:   {r: 0.886, g: 0.000, b: 0.345},
  GOLD:  {r: 0.682, g: 0.616, b: 0.345},
  CREAM: {r: 0.980, g: 0.973, b: 0.957},
};
const FONTS = {
  DISPLAY:  {family: 'Playfair Display', style: 'Black Italic'},
  HEADLINE: {family: 'Playfair Display', style: 'Bold'},
  BODY:     {family: 'Inter', style: 'Regular'},
  MEDIUM:   {family: 'Inter', style: 'Medium'},
  SEMIBOLD: {family: 'Inter', style: 'SemiBold'},
  BOLD:     {family: 'Inter', style: 'Bold'},
};
const SLOT_W = 2040;
const M      = 80;
const INNER  = 32;
const GAP    = 32;
const CARD_R = 8;
```

---

## Defining a New Client Kit

When starting a deck for a new client, answer these questions before touching Figma.

```
☐ 1. Primary background color (hex or RGB)
☐ 2. Card/elevated surface color (darker or lighter than bg?)
☐ 3. Primary accent color (brand color used for rules, CTAs, highlights)
☐ 4. Secondary accent (optional — for tags, gold, muted emphasis)
☐ 5. Primary text color (cream, white, or dark?)
☐ 6. Headline font family + weight
☐ 7. Body font family + weight
☐ 8. Is the slide background dark or light? (determines legibility strategy)
```

### Legibility rules by background type

**Dark bg:** Bold weight + large type is the primary legibility lever.
Colored text is usable when weight >= SemiBold and size >= 24. Never use thin weight
on dark at any color. "Red on black fails" = weight/size failure, not a color failure.

**Light bg:** Contrast via darkness of text. Use 90%+ opacity text.
Accent colors must pass WCAG AA (>= 4.5:1) for body copy on the bg color.

### Font availability check

```javascript
const testFonts = [
  {family: 'Playfair Display', style: 'Bold'},
  {family: 'Inter', style: 'Regular'},
  // add client fonts here
];
const missing = [];
for (const font of testFonts) {
  try {
    await figma.loadFontAsync(font);
    console.log(`OK: ${font.family} ${font.style}`);
  } catch (e) {
    missing.push(`${font.family} ${font.style}`);
    console.error(`MISSING: ${font.family} ${font.style}`);
  }
}
if (missing.length) throw new Error(`Fonts unavailable — abort before slide work: ${missing.join(', ')}`);
```

---

## Kit Lock

Before building any slides, record the approved kit in the project memory file:

```markdown
## Kit: lean-dark
- bg: #0A0A0D
- card: #171719
- accent1: #E20058 (magenta)
- accent2: #AE9D58 (gold)
- text: #F9F8F4 (cream)
- headline: Playfair Display Bold / Black Italic
- body: Inter Regular / Medium
- approved by: [name] on [date]
```

Do not change the kit mid-deck without a checkpoint (PDF + snapshot) and explicit approval.
