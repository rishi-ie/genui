---
name: genui
description: Generate beautiful, interactive web artifacts (diagrams, charts, visualizations, mockups) from your CLI agent. Use when the user asks to show something visually, visualize, render, mock up, diagram, draw, or create any visual representation — especially for architecture diagrams, data flows, neural networks, component trees, interactive calculators, or dashboard widgets. Activates on phrases like "show me visually", "render this", "visualize how", "draw the architecture", "let me see", "mock this up", "create a dashboard", "show me what this looks like".
---

# genui

Generate self-contained HTML artifacts with live browser preview. Every artifact is a **complete, polished product** — not a sketch or wireframe.

## Core Principle: Complete Artifacts

Every artifact should feel like a finished product:
- **Full design system** — headers, consistent styling, proper typography
- **Interactions** — sliders, tooltips, click-to-expand, live calculations
- **Polish** — legends, zoom controls, proper spacing, rounded corners
- **Content is runtime** — the data changes, the artifact shell is complete

```
Good:  Sliders control chart → chart updates live
Bad:   Static image of a chart with no controls
```

## Artifact Structure

Every artifact has:

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
      /* Design tokens - ALWAYS include these */
      --block-lime: #d4ff8a;
      --block-lilac: #e6c8ff;
      --block-cream: #fef7e6;
      --block-mint: #c8ffec;
      --ink: #201d1d;
      --canvas: #fdfcfc;
      --surface-soft: #f8f7f7;
      --surface-card: #f1eeee;
      --hairline: rgba(15,0,0,0.12);
      --hairline-strong: #646262;
      --body: #424245;
      --mute: #646262;
      --accent: #007aff;
      --rounded-md: 8px;
      --rounded-lg: 24px;
      --rounded-pill: 50px;
      --space-xs: 4px;
      --space-sm: 8px;
      --space-md: 12px;
      --space-lg: 16px;
      --space-xl: 24px;
      --space-xxl: 32px;
    }
    * { box-sizing: border-box; }
    body {
      font-family: 'JetBrains Mono', 'IBM Plex Mono', ui-monospace, monospace;
      font-size: 14px;
      line-height: 1.5;
      color: var(--ink);
      background: var(--canvas);
      margin: 0;
      min-height: 100vh;
    }
  </style>
</head>
<body>
  <!-- Header with brand mark -->
  <div class="header">
    <span class="brand">TAG</span>
    <span class="title">Descriptive Title</span>
  </div>

  <!-- Main content area -->
  <div class="container">
    <!-- Cards, controls, charts, diagrams -->
  </div>

  <!-- Legend (for diagrams) -->
  <div class="legend">...</div>

  <!-- Zoom controls -->
  <div class="zoom-controls">...</div>

  <!-- Tooltip -->
  <div class="tooltip" id="tooltip"></div>

  <script>
    // Interactions, calculations, D3/Chart.js
  </script>
</body>
</html>
```

## Design System

### Colors

```css
/* Block backgrounds (pastel, for headers/cards) */
--block-lime: #d4ff8a;     /* Headers */
--block-lilac: #e6c8ff;    /* Secondary headers */
--block-cream: #fef7e6;    /* Cards, legend */
--block-mint: #c8ffec;     /* Accent sections */
--block-pink: #ffd5ec;     /* Special */

/* Monochrome (text, borders) */
--ink: #201d1d;            /* Headlines */
--canvas: #fdfcfc;         /* Background */
--surface-soft: #f8f7f7;    /* Component fill */
--surface-card: #f1eeee;   /* Cards */
--hairline: rgba(15,0,0,0.12);   /* Borders */
--hairline-strong: #646262;
--body: #424245;           /* Body text */
--mute: #646262;           /* Labels, hints */

/* Semantic */
--accent: #007aff;         /* Links, highlights */
--success: #008866;       /* Positive values */

/* Diagram colors (use semantically) */
.c-gray  { fill: #f5f5f5; stroke: #666; }
.c-blue  { fill: #e6f1fb; stroke: #0066cc; }
.c-teal  { fill: #e1f5ee; stroke: #008866; }
.c-coral { fill: #faece7; stroke: #993322; }
```

### Semantic Color Assignment (Diagrams)

| Represents | Color |
|-----------|-------|
| Input / Start | gray |
| Process / Service | blue |
| Database / Storage | teal |
| Decision | blue |
| Success / Yes | teal |
| Error / No | coral |
| User / Person | purple |
| External | coral |

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

```css
--rounded-md: 8px;     /* Components */
--rounded-lg: 24px;     /* Cards, legend */
--rounded-pill: 50px;  /* Buttons, brand mark */
```

## Interaction Patterns

### Sliders (always include value display)

```html
<div class="control-group">
  <div class="control-header">
    <span class="control-label">Label</span>
    <span class="control-value" id="display">0</span>
  </div>
  <input type="range" id="input" min="0" max="100" value="50">
</div>

<script>
const input = document.getElementById('input');
input.addEventListener('input', () => {
  document.getElementById('display').textContent = input.value;
});
</script>
```

### Tooltips (for diagram nodes)

```html
<div class="tooltip" id="tooltip"></div>

<script>
node.addEventListener('mouseenter', e => {
  const t = document.getElementById('tooltip');
  t.innerHTML = `<h4>${name}</h4><p>${description}</p>`;
  t.classList.add('visible');
  t.style.left = (e.pageX + 12) + 'px';
  t.style.top = (e.pageY - 40) + 'px';
});
node.addEventListener('mouseleave', () => {
  document.getElementById('tooltip').classList.remove('visible');
});
</script>
```

### Result Cards (for calculators/dashboards)

```html
<div class="result-card">
  <div class="result-label">Label</div>
  <div class="result-value" id="result">0</div>
  <div class="result-detail">Context</div>
</div>
```

## SVG Rules

### viewBox = 680 (fixed width)

```svg
<svg width="100%" viewBox="0 0 680 H">
```

This ensures coordinate units = CSS pixels 1:1. Never change 680.

### Box Width Formula

```
box_width = max(title_chars × 8, subtitle_chars × 7) + 24
```

### Pre-built SVG Classes

```css
.t   { font: 500 14px sans-serif; }
.ts  { font: 400 12px sans-serif; }
.box { fill: var(--surface-soft); stroke: var(--hairline-strong); stroke-width: 1; rx: 6; }
.arr { stroke: var(--hairline-strong); stroke-width: 1.5; fill: none; marker-end: url(#arrow); }
```

### Arrow Marker

```svg
<defs>
  <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" 
          markerWidth="6" markerHeight="6" orient="auto">
    <path d="M2 1L8 5L2 9" fill="none" stroke="var(--hairline-strong)" stroke-width="1.5"/>
  </marker>
</defs>
```

## Common Layouts

### Card with Controls

```html
<div class="card">
  <h2 class="card-title">Section Title</h2>
  <div class="control-group">...</div>
  <div class="control-group">...</div>
</div>
```

### Grid of Result Cards

```html
<div class="results">
  <div class="result-card">
    <div class="result-label">Primary</div>
    <div class="result-value" id="primary">0</div>
  </div>
  <div class="result-card highlight">
    <div class="result-label">Total</div>
    <div class="result-value accent" id="total">0</div>
  </div>
</div>
```

### Number Formatting

```javascript
Math.round(value)              // integers
value.toFixed(2)               // 2 decimals
'$' + Math.round(v).toLocaleString()  // currency
```

## CDN Dependencies

```html
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
```

## Artifact Directory

```
<cwd>/.genui/artifacts/
```

- Save as `artifact-<timestamp>.html`
- Maintain `index.html` gallery

## Invocation Steps

1. **Assess** — What does the user want to see?
2. **Plan** — What interactions will make it engaging? (sliders, tooltips, live updates)
3. **Generate** — Write complete artifact with design system, interactions, polish
4. **Save** — Write to `.genui/artifacts/`
5. **Open** — Launch in browser
6. **Describe** — Tell user what they're looking at and how to interact

## Complexity Budget

| Rule | Limit |
|------|-------|
| Subtitles | ≤5 words |
| Color ramps | ≤3 per diagram |
| Boxes per tier | ≤4 at 140px each |

## Tips

- **Every artifact is complete** — no "click to see more", no placeholder text
- **Content is runtime** — sliders/inputs control everything
- **Hover everywhere** — tooltips on nodes, highlights on connections
- **Sentence case** — never Title Case or ALL CAPS