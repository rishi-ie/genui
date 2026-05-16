---
name: genui
description: Generate beautiful, interactive web artifacts (diagrams, charts, 3D models, forms) that auto-open in the browser with live refresh. Use when the user asks to show something visually, visualize, diagram, render, mock up, preview, or generate an interactive artifact — especially for architecture diagrams, transformer visualizations, data flow, neural network diagrams, component trees, or any spatial/visual concept the user wants to see in the browser. Activates on phrases like "show me visually", "render this", "visualize how", "generate the diagram", "draw the architecture", "let me see", "open in browser", "mock this up", "show me what this looks like", "can you show me". Do NOT trigger for plain text/code responses unless the user explicitly wants a visual artifact.
---

# genui

Generate self-contained HTML artifacts with live browser preview. The agent writes a single `.html` file in the user's current working directory under `.genui/artifacts/`, opens it automatically, and the browser refreshes on every update.

## Core Behavior

1. **Detect the intent** — user wants to see something visually rendered in the browser
2. **Design the visualization** — choose format that best communicates the concept
3. **Generate the artifact** — write a complete, self-contained `.html` file
4. **Open it** — launch it in the default browser via CLI
5. **Watch for changes** — re-open (or refresh) when the artifact is updated

## Artifact Directory

```
<cwd>/.genui/artifacts/
```

- Save each artifact as `artifact-<timestamp>.html` (e.g., `artifact-20250516-143022.html`)
- Add a `name` to the top comment for readability: `<!-- genui: transformer-attention | interactive attention pattern visualization -->`
- Maintain `index.html` in `.genui/` — a gallery listing all artifacts with names and timestamps

## Design System (Figma × OpenCode)

Fused design: Figma's pastel color-blocks meet OpenCode's monospace typographic restraint. Light theme only.

### Colors (Figma Block System)

```css
:root {
  /* Block Colors - Pastel sections */
  --block-lime: #d4ff8a;      /* Header, accent blocks */
  --block-lilac: #e6c8ff;     /* Info card, secondary blocks */
  --block-cream: #fef7e6;     /* Legend, cards */
  --block-mint: #c8ffec;      /* Alternate blocks */
  --block-pink: #ffd5ec;     /* Accent blocks */
  --block-coral: #ffcdc8;     /* Secondary blocks */
  
  /* Monochrome - OpenCode system */
  --ink: #201d1d;             /* Headlines, primary text */
  --canvas: #fdfcfc;          /* Default background */
  --surface-soft: #f8f7f7;   /* Component fill */
  --surface-card: #f1eeee;   /* Formulas, stats */
  --hairline: rgba(15,0,0,0.12);  /* 1px borders */
  --hairline-strong: #646262;
  --body: #424245;            /* Default paragraph */
  --mute: #646262;            /* Labels, metadata */
  --accent: #007aff;          /* Cross-attention, links */
}
```

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

h1 { font-size: 86px; font-weight: 340; letter-spacing: -1.72px; line-height: 1; }
h2 { font-size: 64px; font-weight: 340; letter-spacing: -0.96px; }
h3 { font-size: 26px; font-weight: 540; letter-spacing: -0.26px; }
h4 { font-size: 18px; font-weight: 480; }

.label {
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--mute);
}

.mono {
  font-family: 'JetBrains Mono', monospace;
}
```

### Layout (Box-Constrained)

```css
/* Spacing */
--space-xs: 4px;
--space-sm: 8px;
--space-md: 12px;
--space-lg: 16px;
--space-xl: 24px;
--space-xxl: 32px;

/* All elements inside boxes */
.header { padding: 0 var(--space-xl); }
.card { padding: var(--space-lg); }
.panel { padding: var(--space-xxl); }
.legend { padding: var(--space-lg) var(--space-xl); }
.info-card { padding: var(--space-lg); }
```

### Shapes (Figma)

- **Pill buttons**: `border-radius: 50px`
- **Cards/containers**: `border-radius: 24px` (lg)
- **Components**: `border-radius: 8px` (md)
- **Inputs**: `border-radius: 8px`

### Elevation

| Level | Treatment | Use |
|---|---|---|
| 0 | Flat, no shadow | Color block sections |
| 1 | 1px hairline border | Cards, inputs |
| 2 | Subtle shadow `0 4px 16px rgba(0,0,0,0.06)` | Floating panels |

### Light Theme Only

No dark mode. Monochrome system on cream canvas with pastel color blocks for accent surfaces.

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
      --block-lime: #d4ff8a;
      --block-lilac: #e6c8ff;
      --block-cream: #fef7e6;
      --ink: #201d1d;
      --canvas: #fdfcfc;
      --surface-soft: #f8f7f7;
      --surface-card: #f1eeee;
      --hairline: rgba(15,0,0,0.12);
      --hairline-strong: #646262;
      --body: #424245;
      --mute: #646262;
      --accent: #007aff;
      --rounded-sm: 4px;
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
    body {
      font-family: 'JetBrains Mono', 'IBM Plex Mono', monospace;
      font-size: 14px;
      line-height: 1.5;
      color: var(--ink);
      background: var(--canvas);
      margin: 0;
    }
    /* Add styles here */
  </style>
</head>
<body>
  <!-- Content here -->
</body>
</html>
```

## Browser Opening

**Windows:**
```bash
start "" "<absolute-file-path>"
```

**macOS:**
```bash
open "<absolute-file-path>"
```

**Linux:**
```bash
xdg-open "<absolute-file-path>"
```

**Live refresh:** Re-run the open command when file changes. Browser auto-refreshes.

## CDN Dependencies

```html
<!-- D3.js - diagrams, charts, force layouts -->
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>

<!-- Three.js - 3D rendering -->
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>

<!-- Mermaid - flowcharts, diagrams -->
<script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>

<!-- GSAP - animations -->
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
```

Include only what's needed.

## Artifact Patterns

### Architecture Diagram
- SVG via D3 with interactive nodes
- Color-coded by layer/type
- Hover tooltips with details
- Zoom/pan for large diagrams
- Directional arrows showing flow

### Data Flow Visualization
- Animated particles along paths
- Hover to highlight specific flow
- Left-to-right or top-to-bottom layout

### 3D Viewer
- Three.js with orbit controls
- Ambient + directional lighting

### Interactive Form/Mockup
- Pastel block sections on cream canvas
- Monospace typography throughout
- Pill buttons with proper spacing

## Index Page

```html
<!-- genui-index -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>genui artifacts</title>
  <style>
    :root {
      --block-cream: #fef7e6;
      --ink: #201d1d;
      --canvas: #fdfcfc;
      --hairline: rgba(15,0,0,0.12);
      --body: #424245;
      --mute: #646262;
      --rounded-lg: 24px;
      --space-lg: 16px;
      --space-xl: 24px;
    }
    body { font-family: 'JetBrains Mono', monospace; font-size: 14px; background: var(--canvas); margin: 0; padding: var(--space-xl); }
    h1 { font-size: 24px; font-weight: 480; color: var(--ink); margin: 0 0 var(--space-xl); }
    ul { list-style: none; padding: 0; margin: 0; }
    li { padding: var(--space-lg) 0; border-bottom: 1px solid var(--hairline); }
    a { color: var(--ink); text-decoration: none; }
    a:hover { color: var(--accent); }
    .meta { font-size: 12px; color: var(--mute); margin-top: 4px; }
  </style>
</head>
<body>
  <h1>genui artifacts</h1>
  <ul class="artifacts">
    <!-- Populated by agent -->
  </ul>
</body>
</html>
```

## Invocation Steps

1. **Assess** — What does the user want to see? What format best communicates it?
2. **Design** — Choose diagram/chart/3D/form based on the concept
3. **Generate** — Write complete HTML, apply design system, use CDN libs
4. **Save** — Write to `.genui/artifacts/artifact-<timestamp>.html`
5. **Index** — Update `.genui/index.html` with the new artifact
6. **Open** — Run platform-appropriate open command
7. **Describe** — Tell the user what they're looking at and how to interact

## Tips

- **One idea per artifact** — Multiple concepts = multiple artifacts
- **Interactive by default** — Hover, click, tooltips make it engaging
- **Clean and minimal** — Whitespace, don't overcrowd
- **Label everything** — Nodes, layers, axes, sections
- **Use pastel blocks** — Lime/lilac/cream/mint for accent surfaces
- **Monochrome text** — Keep body text in ink/body colors
- **Pill buttons** — Use border-radius: 50px for CTAs
- **Box-constrained** — Every element inside a box with consistent padding

## Edge Cases

- **Large diagrams** — SVG with viewBox + pan/zoom controls
- **Windows spaces** — `start "" "path with spaces.html"`
- **File already open** — Re-open refreshes the tab
- **No browser** — Print path so user can open manually