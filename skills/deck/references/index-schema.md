# Deck Index Schema

Every deck gets a living CSV index. This file defines the schema, validation rules,
and refresh discipline. The index is the source of truth for slide order and node IDs.

---

## CSV Schema

```
Order,Act,Slide Title,Type,Background,Status,Figma Node ID,Notes
```

### Column Definitions

| Column       | Type    | Allowed values                                                       | Rules                                                |
|--------------|---------|----------------------------------------------------------------------|------------------------------------------------------|
| Order        | integer | 1, 2, 3, … (no gaps)                                                 | Reorder by renumbering; no skipped integers          |
| Act          | string  | Free text, e.g. "1 — Case for Change"                                | Group contiguous slides; same string = same section  |
| Slide Title  | string  | Free text                                                            | Should match the Figma frame name                    |
| Type         | enum    | cover, manifesto, stat, card-cols, anchor, from-to, flow, deep-dive, table | Must match a type in slide-types.md           |
| Background   | enum    | Dark, Cream, Magenta panel, Black panel, Gold panel                  | Used for visual rhythm audit                         |
| Status       | enum    | Draft, In progress, Final                                            | All Final before calling deck done                   |
| Figma Node ID| string  | "112:2" format                                                       | Refresh only at checkpoints — IDs change on rebuild  |
| Notes        | string  | Free text, one line                                                  | Record what changed, what's pending                  |

---

## Example Rows

```csv
Order,Act,Slide Title,Type,Background,Status,Figma Node ID,Notes
1,1 — Case for Change,Cover,cover,Dark,Final,112:2,Spark burst mark kept
2,1 — Case for Change,Executive Summary,card-cols,Dark,Final,113:2,Shift line threads attention
3,1 — Case for Change,The Belief,manifesto,Magenta panel,Final,94:2,Trust is the new performance channel
4,1 — Case for Change,The Market,stat,Cream,Final,127:18,#1 / 63% / 49% — eMarketer 2025
5,2 — Why Us,The POSE Stack,anchor,Dark,Final,151:2,Pill collisions fixed; 5 layers
```

---

## Validator (Node.js / PowerShell)

Run this before calling the deck done. Returns all violations.

```javascript
// Node.js: node validate-index.js path/to/index.csv
const fs = require('fs');
// Quote-aware CSV split: handles commas inside double-quoted fields (e.g. slide titles with commas).
function parseCSVRow(line) {
  const cols = []; let cur = ''; let inQ = false;
  for (let i = 0; i < line.length; i++) {
    const c = line[i];
    if (c === '"') { inQ = !inQ; }
    else if (c === ',' && !inQ) { cols.push(cur.trim()); cur = ''; }
    else { cur += c; }
  }
  cols.push(cur.trim());
  return cols;
}
const rows = fs.readFileSync(process.argv[2], 'utf8')
  .split('\n').slice(1)
  .filter(r => r.trim())
  .map(r => parseCSVRow(r));

const TYPES = new Set(['cover','manifesto','stat','card-cols','anchor','from-to','flow','deep-dive','table']);
const BGS   = new Set(['Dark','Cream','Magenta panel','Black panel','Gold panel']);
const STATI = new Set(['Draft','In progress','Final']);

const violations = [];
let prevOrder = 0;

rows.forEach((cols, i) => {
  const [order, act, title, type, bg, status, nodeId, notes] = cols;
  const lineNum = i + 2;

  const n = parseInt(order);
  if (isNaN(n))                       violations.push(`L${lineNum}: Order not a number: "${order}"`);
  else if (n !== prevOrder + 1)        violations.push(`L${lineNum}: Order gap — expected ${prevOrder + 1}, got ${n}`);
  prevOrder = n;

  if (!title?.trim())                  violations.push(`L${lineNum}: Empty Slide Title`);
  if (!TYPES.has(type))                violations.push(`L${lineNum}: Unknown Type "${type}"`);
  if (!BGS.has(bg))                    violations.push(`L${lineNum}: Unknown Background "${bg}"`);
  if (!STATI.has(status))              violations.push(`L${lineNum}: Unknown Status "${status}"`);
  if (!nodeId?.match(/^\d+:\d+$/))     violations.push(`L${lineNum}: Node ID format invalid: "${nodeId}"`);
});

if (violations.length) {
  console.error('Index violations:\n' + violations.join('\n'));
  process.exit(1);
} else {
  console.log(`Index OK — ${rows.length} slides`);
}
```

```powershell
# PowerShell quick-check: count non-Final rows
$rows = Import-Csv "path\to\index.csv"
$pending = $rows | Where-Object { $_.Status -ne 'Final' }
Write-Host "Pending: $($pending.Count) / $($rows.Count) slides"
$pending | Select-Object Order, 'Slide Title', Status
```

---

## Anchor Density Check

```javascript
const anchors = rows.filter(r => r.Type === 'anchor').length;
const total   = rows.length;
const ratio   = total / anchors;
console.log(`Anchors: ${anchors} / ${total} slides — ratio 1:${Math.round(ratio)}`);
if (ratio > 8) console.warn('WARNING: anchor density low — add an anchor slide');
if (ratio < 4) console.warn('WARNING: too many anchors — feels like a wall-of-text deck');
```

---

## Refresh Rules

| When to refresh Node ID column | When NOT to refresh |
|-------------------------------|---------------------|
| After a clean PDF checkpoint  | After a text-only edit |
| After rebuilding a slide layout | After a copy sweep |
| After adding/removing slides  | Mid-session between ops |
| Before handing off the deck   | During an iterative build loop |

**Why:** Every time a frame is duplicated or rebuilt, Figma assigns new IDs.
If you refresh mid-session, the index immediately goes stale again.
Refresh once, at a stable state, after taking a PDF + snapshot.

---

## Background Rhythm Validator

A healthy deck alternates backgrounds for visual rhythm. No run of >5 Dark slides.

```javascript
const bgs = rows.map(r => r.Background);
let streak = 1;
bgs.forEach((bg, i) => {
  if (i === 0) return;
  streak = bg === bgs[i-1] ? streak + 1 : 1;
  if (streak > 5 && bg === 'Dark')
    console.warn(`Slides ${i-3}–${i+1}: ${streak} consecutive Dark slides — consider a Cream or panel accent`);
});
```
