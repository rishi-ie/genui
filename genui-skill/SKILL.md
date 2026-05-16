---
name: genui
description: Generate beautiful, interactive web artifacts (diagrams, charts, visualizations, mockups) from your CLI agent. Use when the user asks to show something visually, visualize, render, mock up, diagram, draw, or create any visual representation — especially for architecture diagrams, data flows, neural networks, component trees, interactive calculators, or dashboard widgets. Activates on phrases like "show me visually", "render this", "visualize how", "draw the architecture", "let me see", "mock this up", "create a dashboard", "show me what this looks like".
---

# genui

Generate self-contained HTML artifacts with live browser preview. Every artifact is a **complete, polished product** — not a sketch.

## Core Principle: Complete Artifacts

Every artifact includes:
- **Header** with brand mark and title
- **Full design system** — CSS variables, spacing, shapes, typography
- **Interactions** — sliders, tooltips, hover states, live calculations
- **Polish** — legends, zoom controls, proper spacing, rounded corners
- **Content is runtime** — sliders/inputs control everything

```
Good:  Sliders control chart → chart updates live
Bad:   Static image with no controls
```

## Design Laws

### Color

**Never use pure black (#000) or pure white (#fff).** Tint everything toward the brand hue with tiny chroma (0.005-0.015).

**OKLCH-aware** (converted to hex for compatibility):
- Lift lightness toward white: reduce chroma to avoid garish pastels
- Push darkness toward black: reduce chroma to avoid muddy darks

**One accent ≤10% surface.** More color is Committed/Full palette — use deliberately, not by default.

### Typography

- **Font**: JetBrains Mono (monospace everywhere — this is a design choice, not a limitation)
- **Line length**: 65-75ch max for prose sections
- **Hierarchy**: Weight contrast ≥100 units between heading and body
- **No em dashes (—).** Use commas, colons, or periods.

### Layout

- **Vary spacing for rhythm.** Same padding everywhere is monotony.
- **Cards are the lazy answer.** Use them only when truly necessary. Nested cards are always wrong.
- **Don't wrap everything in a container.** Most things don't need one.

### Motion

- **Ease out with exponential curves.** `ease-out-quart` or `ease-out-expo`.
- **Never bounce or elastic.** Looks dated.
- **Don't animate layout properties** (width, height, margin, padding).

### Absolute Bans

Match-and-refuse these patterns:

| Ban | Why | Fix |
|-----|-----|-----|
| Side-stripe borders (`border-left > 1px`) as accent | AI slop tell | Full borders, background tints, or leading icons |
| Gradient text (`background-clip: text`) | Decorative, never meaningful | Solid color. Emphasis via weight/size |
| Glassmorphism as default | Overused | Rare and purposeful, or nothing |
| Identical card grids | Lazy | Vary card sizes, weights, content |
| Modal as first thought | Laziness | Inline / progressive disclosure first |

## Complete Artifact Structure

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
      /* Brand colors — OKLCH-derived, tinted */
      --brand: oklch(45% 0.15 250);      /* Primary hue */
      --accent: oklch(55% 0.2 250);       /* Saturated accent */
      
      /* Surfaces — NEVER pure white, always tinted */
      --canvas: oklch(98% 0.005 250);    /* Background */
      --surface-soft: oklch(95% 0.01 250); /* Cards */
      --surface-card: oklch(92% 0.015 250); /* Raised */
      
      /* Text — NEVER pure black */
      --ink: oklch(20% 0.015 250);       /* Headlines */
      --body: oklch(40% 0.012 250);      /* Body */
      --mute: oklch(55% 0.008 250);      /* Labels */
      
      /* Semantic */
      --success: oklch(55% 0.15 160);
      --error: oklch(55% 0.2 25);
      
      /* Elevation (dark mode: higher lightness = higher elevation) */
      --hairline: oklch(85% 0.01 250 / 0.15);
      
      /* Spacing */
      --space-xs: 4px; --space-sm: 8px; --space-md: 12px;
      --space-lg: 16px; --space-xl: 24px; --space-xxl: 32px;
      
      /* Shapes */
      --rounded-md: 8px; --rounded-lg: 24px; --rounded-pill: 50px;
    }
    * { box-sizing: border-box; }
    body {
      font-family: 'JetBrains Mono', 'IBM Plex Mono', monospace;
      font-size: 14px; line-height: 1.5;
      color: var(--ink); background: var(--canvas); margin: 0;
    }
  </style>
</head>
<body>
  <!-- Header: brand mark + title -->
  <header class="header">
    <span class="brand">TAG</span>
    <span class="title">Descriptive Title</span>
  </header>

  <!-- Main content -->
  <main class="container">
    <!-- Control cards, charts, diagrams -->
  </main>

  <!-- Legend (diagrams only) -->
  <aside class="legend">...</aside>

  <!-- Zoom controls -->
  <div class="zoom-controls">...</div>

  <!-- Tooltip (shared, positioned via JS) -->
  <div class="tooltip" id="tooltip"></div>

  <script>
    // Interactions, calculations, Chart.js/D3
  </script>
</body>
</html>
```

## Semantic Color System (Diagrams)

Assign colors by **meaning**, not aesthetics:

| Represents | Color | Rationale |
|-----------|-------|-----------|
| Input / Start / End | `--neutral` gray | Neutral, structural |
| Process / Service | `--c-blue` | Action, work |
| Database / Storage | `--c-teal` | Stable, persistent |
| Decision / Branch | `--c-amber` | Caution, choice |
| Success / Yes | `--c-green` | Positive outcome |
| Error / No | `--c-red` | Danger, reject |
| User / Person | `--c-purple` | Human element |
| External / API | `--c-coral` | Outside system |

**Rule**: Max 3 colors per diagram. Gray + 2 accents is cleaner than full rainbow.

## Interaction Patterns

### Sliders

```html
<div class="control-group">
  <div class="control-header">
    <span class="control-label">Label</span>
    <span class="control-value" id="display">0%</span>
  </div>
  <input type="range" id="input" min="0" max="100" value="50">
</div>
```

### Tooltips

```html
<div class="tooltip" id="tooltip"></div>
<script>
node.addEventListener('mouseenter', e => {
  const t = document.getElementById('tooltip');
  t.innerHTML = `<h4>${name}</h4><p>${desc}</p>`;
  t.classList.add('visible');
  t.style.left = (e.pageX + 12) + 'px';
  t.style.top = (e.pageY - 40) + 'px';
});
</script>
```

### Result Cards

```html
<div class="result-card">
  <div class="result-label">Primary Metric</div>
  <div class="result-value" id="result">$0</div>
  <div class="result-detail">Context here</div>
</div>
```

## Number Formatting

Always round displayed numbers:

```javascript
Math.round(value)                    // integers
value.toFixed(2)                     // 2 decimals
'$' + Math.round(v).toLocaleString()  // currency
```

**Why**: `0.1 + 0.2 = 0.30000000000000004` without rounding.

## SVG Rules

### viewBox Width = 680

```svg
<svg width="100%" viewBox="0 0 680 H">
```

This ensures coordinate units = CSS pixels 1:1. Never change 680.

### Box Width Formula

```
box_width = max(title_chars × 8, subtitle_chars × 7) + 24
```

### Arrow Marker

```svg
<defs>
  <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" 
          markerWidth="6" markerHeight="6" orient="auto">
    <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5"/>
  </marker>
</defs>
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
2. **Plan** — What interactions make it engaging? (sliders, tooltips, live updates)
3. **Generate** — Complete artifact with design system, interactions, polish
4. **Save** — Write to `.genui/artifacts/`
5. **Open** — Launch in browser
6. **Describe** — Tell user how to interact

## Complexity Budget

| Rule | Limit |
|------|-------|
| Subtitles | ≤5 words |
| Color ramps | ≤3 per diagram |
| Boxes per tier | ≤4 at 140px each |

## Tips

- **Every artifact is complete** — no "click to see more", no placeholders
- **Content is runtime** — sliders/inputs control everything
- **Hover everywhere** — tooltips on nodes, highlights on connections
- **Sentence case** — never Title Case or ALL CAPS
- **Vary spacing** — don't use same padding everywhere
- **Cards sparingly** — only when truly the best affordance
- **Reduce chroma at extremes** — pastels near white, muted near black