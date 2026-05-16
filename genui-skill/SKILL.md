---
name: genui
description: Generate beautiful, interactive web artifacts (diagrams, charts, visualizations, mockups) from your CLI agent. Use when the user asks to show something visually, visualize, render, mock up, diagram, draw, or create any visual representation — especially for architecture diagrams, data flows, neural networks, component trees, interactive calculators, or dashboard widgets. Activates on phrases like "show me visually", "render this", "visualize how", "draw the architecture", "let me see", "mock this up", "create a dashboard", "show me what this looks like".
---

# genui

Generate self-contained HTML artifacts with live browser preview. The agent writes a single `.html` file in the user's current working directory, opens it automatically, and updates it on every change.

## Core Behavior

1. **Detect** — User wants something rendered visually
2. **Select module** — Choose the right pattern (diagram/chart/mockup/interactive)
3. **Generate** — Write complete HTML using design guidelines
4. **Save** — Write to `.genui/artifacts/artifact-<timestamp>.html`
5. **Index** — Update `.genui/index.html` gallery
6. **Open** — Launch in default browser via CLI
7. **Refresh** — Re-open when artifact changes

## Module Selection

| Request | Module | Output |
|---------|--------|--------|
| Architecture / flow diagram | `diagram` | SVG with nodes + arrows |
| Dashboard with charts | `chart` | HTML + Chart.js |
| Interactive with sliders | `interactive` | HTML + live JS |
| UI mockup / form | `mockup` | HTML components |
| File/component tree | `diagram` | D3 tree layout |
| 3D model | `art` | Three.js viewer |

When unsure, default to `diagram` — it's the most flexible.

## Artifact Directory

```
<cwd>/.genui/artifacts/
```

- Save as `artifact-<timestamp>.html`
- Add comment header: `<!-- genui: <name> | <description> -->`
- Maintain `index.html` gallery

## Design System (Figma × OpenCode)

Light theme only. Monospace typography with pastel color blocks.

### Colors

```css
:root {
  /* Block Colors (backgrounds) */
  --block-lime: #d4ff8a;
  --block-lilac: #e6c8ff;
  --block-cream: #fef7e6;
  --block-mint: #c8ffec;
  --block-pink: #ffd5ec;
  
  /* Monochrome (text + borders) */
  --ink: #201d1d;
  --canvas: #fdfcfc;
  --surface-soft: #f8f7f7;
  --surface-card: #f1eeee;
  --hairline: rgba(15,0,0,0.12);
  --hairline-strong: #646262;
  --body: #424245;
  --mute: #646262;
  
  /* Semantic */
  --accent: #007aff;
}
```

### Semantic Color Assignment (Diagram)

| Node represents | Color |
|-----------------|-------|
| Input / Start / End | gray |
| Process / Service | blue |
| Database / Storage | teal |
| Decision / Branch | amber |
| Error / Failure | red |
| Success / Output | green |
| User / Person | purple |
| External / API | coral |

### Typography

```css
body {
  font-family: 'JetBrains Mono', 'IBM Plex Mono', ui-monospace, monospace;
  font-size: 14px;
  line-height: 1.5;
  color: var(--ink);
  background: var(--canvas);
  margin: 0;
}
```

### Spacing

```css
--space-xs: 4px;
--space-sm: 8px;
--space-md: 12px;
--space-lg: 16px;
--space-xl: 24px;
--space-xxl: 32px;
```

### Shapes

- **Pill buttons**: `border-radius: 50px`
- **Cards**: `border-radius: 24px`
- **Components**: `border-radius: 8px`
- **Borders**: 1px hairline

---

## SVG Generation (Diagram Module)

### viewBox is Load-Bearing

```svg
<svg width="100%" viewBox="0 0 680 H">
```

**Never change 680.** It ensures coordinate units = CSS pixels 1:1. Center narrow content within 680.

### Box Width Formula

```
box_width = max(title_chars × 8, subtitle_chars × 7) + 24
```

**Example**: Title "Authentication" (14 chars) + Subtitle "JWT" (3 chars)
- 14 × 8 = 112px, 3 × 7 = 21px
- Minimum box width = 136px

### Pre-Built SVG Classes

```css
.t   { font: 500 14px sans-serif; }   /* Title */
.ts  { font: 400 12px sans-serif; }   /* Subtitle */
.th  { font: 500 14px sans-serif; }   /* Highlighted title */

.box  { fill: var(--surface-soft); stroke: var(--hairline); }
.node { cursor: pointer; }
.arr  { marker-end: url(#arrow); }

/* Color ramps */
.c-gray   { fill: #f5f5f5; stroke: #666; }
.c-blue   { fill: #e6f1fb; stroke: #0066cc; }
.c-teal   { fill: #e1f5ee; stroke: #008866; }
.c-purple { fill: #eeedfe; stroke: #6644cc; }
.c-coral  { fill: #faece7; stroke: #993322; }
```

### Arrow Marker (Include in Every SVG)

```svg
<defs>
  <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" 
          markerWidth="6" markerHeight="6" orient="auto-start-reverse">
    <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" 
          stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
  </marker>
</defs>
```

### ViewBox Safety Checklist

1. Lowest element: `max(y + height)` + 40px padding
2. Rightmost element: `max(x + width)` ≤ 680
3. No negative x or y coordinates
4. Check arrow intersections — use L-shaped detours if needed

### Text Rules

- **Sentence case** always. Never Title Case or ALL CAPS.
- SVG text never auto-wraps — use `<tspan>` for multi-line
- Max 5 words in subtitles

---

## HTML Generation (Interactive/Chart/Mockup Modules)

### Fragment Format

```html
<!-- No DOCTYPE, html, head, or body tags -->
<div style="display: flex; gap: 12px;">
  <label style="font-size: 14px;">Years</label>
  <input type="range" min="1" max="40" value="20">
  <span id="output" style="font-size: 14px; font-weight: 500;">20</span>
</div>
<script>
  document.querySelector('input').addEventListener('input', e => {
    document.getElementById('output').textContent = e.target.value;
  });
</script>
```

### Component Patterns

**Metric Card:**
```html
<div style="background: var(--surface-card); border-radius: 8px; padding: 16px;">
  <div style="font-size: 12px; color: var(--mute); text-transform: uppercase;">Revenue</div>
  <div style="font-size: 24px; font-weight: 500;">$1.2M</div>
</div>
```

**Slider:**
```html
<div style="display: flex; align-items: center; gap: 12px;">
  <label>Years</label>
  <input type="range" min="1" max="40" value="20" style="flex: 1;">
  <output style="min-width: 24px;">20</output>
</div>
```

### Number Formatting

```javascript
Math.round(value)              // integers
value.toFixed(2)               // decimals
Intl.NumberFormat().format(v)  // locale-aware
```

---

## CDN Dependencies

```html
<!-- D3.js - diagrams, trees, force layouts -->
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>

<!-- Chart.js - charts and graphs -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>

<!-- Three.js - 3D rendering -->
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>

<!-- GSAP - animations -->
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
```

---

## Complexity Budget

| Rule | Limit |
|------|-------|
| Subtitles | ≤5 words |
| Color ramps per diagram | ≤3 |
| Boxes per horizontal tier | ≤4 (at 140px each) |
| viewBox width | Always 680 |

---

## Browser Opening

```bash
# Windows
start "" "<path>"

# macOS
open "<path>"

# Linux
xdg-open "<path>"
```

---

## Artifact Template

```html
<!-- genui: <name> | <description> -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><name></title>
  <style>
    :root {
      --block-lime: #d4ff8a; --block-lilac: #e6c8ff; --block-cream: #fef7e6;
      --block-mint: #c8ffec; --block-pink: #ffd5ec;
      --ink: #201d1d; --canvas: #fdfcfc; --surface-soft: #f8f7f7;
      --surface-card: #f1eeee; --hairline: rgba(15,0,0,0.12);
      --hairline-strong: #646262; --body: #424245; --mute: #646262;
      --accent: #007aff; --rounded-sm: 4px; --rounded-md: 8px;
      --rounded-lg: 24px; --rounded-pill: 50px;
      --space-xs: 4px; --space-sm: 8px; --space-md: 12px;
      --space-lg: 16px; --space-xl: 24px; --space-xxl: 32px;
    }
    * { box-sizing: border-box; }
    body {
      font-family: 'JetBrains Mono', 'IBM Plex Mono', monospace;
      font-size: 14px; line-height: 1.5; color: var(--ink);
      background: var(--canvas); margin: 0;
    }
    /* Add styles here */
  </style>
</head>
<body>
  <!-- Content here -->
</body>
</html>
```

---

## Invocation Steps

1. **Assess** — What does the user want to see?
2. **Select** — Choose module (diagram/chart/interactive/mockup)
3. **Generate** — Write HTML/SVG using design guidelines
4. **Save** — Write to `.genui/artifacts/artifact-<timestamp>.html`
5. **Index** — Update `.genui/index.html`
6. **Open** — Run platform-appropriate open command
7. **Describe** — Tell user what they're looking at

## Tips

- **Be concise** — Show the essential, explain the rest in prose
- **Use color semantically** — Blue for processes, teal for storage, gray for endpoints
- **Keep subtitles short** — Detail goes in click/hover panels
- **Test the math** — Box width formula prevents overflow
- **Center narrow content** — Don't shrink viewBox below 680

## Edge Cases

- **Large diagrams** — SVG with viewBox + pan/zoom controls
- **Windows with spaces** — `start "" "path with spaces.html"`
- **File already open** — Re-open refreshes the tab
- **Charts need CDN** — Include Chart.js script tag