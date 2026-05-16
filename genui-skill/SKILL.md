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

## Design System (Cohere Aesthetic)

Apply to all artifacts unless user specifies otherwise.

### CSS Variables

```css
:root {
  /* Surfaces */
  --bg: #ffffff;
  --surface: #eeece7;
  --dark-section: #003c33;
  --navy-section: #071829;

  /* Text */
  --ink: #212121;
  --muted: #93939f;
  --slate: #75758a;

  /* Accents */
  --coral: #ff7759;
  --soft-coral: #ffad9b;
  --blue: #1863dc;
  --focus: #4c6ee6;

  /* Utility */
  --border: #d9d9dd;
  --border-light: #e5e7eb;
}
```

### Typography

```css
body {
  font-family: 'Inter', 'Space Grotesk', ui-sans-serif, system-ui, sans-serif;
  font-size: 16px;
  line-height: 1.5;
  color: var(--ink);
  background: var(--bg);
  margin: 0;
}

h1, h2, h3, h4 {
  font-weight: 400;
  letter-spacing: -0.02em;
  line-height: 1.1;
  margin: 0 0 0.5em;
}

h1 { font-size: 48px; }
h2 { font-size: 32px; }
h3 { font-size: 24px; }
h4 { font-size: 18px; }

.label {
  font-family: 'SF Mono', 'Consolas', monospace;
  font-size: 12px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--muted);
}
```

### Shapes

- Cards: `border-radius: 8px`
- Media: `border-radius: 22px`
- Buttons/pills: `border-radius: 32px`

### Elevation

- Mostly flat
- Surface alternation for depth
- Thin borders for containment
- No heavy shadows

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
      --bg: #ffffff; --surface: #eeece7; --dark-section: #003c33;
      --navy-section: #071829; --ink: #212121; --muted: #93939f;
      --slate: #75758a; --coral: #ff7759; --soft-coral: #ffad9b;
      --blue: #1863dc; --focus: #4c6ee6; --border: #d9d9dd;
      --border-light: #e5e7eb;
    }
    body { font-family: 'Inter', 'Space Grotesk', ui-sans-serif, system-ui; margin: 0; }
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

### Architecture Diagram (Transformers, Neural Networks)
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
- Grid floor for reference

### Interactive Form/Mockup
- White surface, labeled inputs
- Immediate feedback on interaction
- Pill CTAs matching design system

## Index Page

```html
<!-- genui-index -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>genui artifacts</title>
  <style>
    body { font-family: 'Inter', ui-sans-serif; max-width: 800px; margin: 40px auto; padding: 0 20px; }
    h1 { font-size: 32px; font-weight: 400; letter-spacing: -0.02em; margin-bottom: 32px; }
    .artifacts { list-style: none; padding: 0; }
    .artifacts li { padding: 20px 0; border-bottom: 1px solid var(--border, #d9d9dd); display: flex; justify-content: space-between; align-items: center; }
    .name { font-size: 18px; color: #212121; text-decoration: none; }
    .name:hover { color: #1863dc; }
    .meta { font-size: 14px; color: #93939f; }
    .label { font-family: monospace; font-size: 12px; text-transform: uppercase; color: #93939f; }
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
- **Use dark sections** — Deep green/navy works great for product-style visualizations
- **Coral sparingly** — Accent color, not fill
- **Responsive** — `max-width: 100%` on media

## Edge Cases

- **Large diagrams** — SVG with viewBox + pan/zoom controls
- **Windows spaces** — `start "" "path with spaces.html"`
- **File already open** — Re-open refreshes the tab
- **No browser** — Print path so user can open manually