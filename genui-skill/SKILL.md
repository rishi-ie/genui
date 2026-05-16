---
name: genui
description: Generate beautiful, interactive web artifacts (diagrams, charts, 3D models, forms) that auto-open in the browser with live refresh. Use when the user asks to show something visually, visualize, diagram, render, mock up, preview, or generate an interactive artifact — especially for architecture diagrams, transformer visualizations, data flow, neural network diagrams, component trees, or any spatial/visual concept the user wants to see in the browser. Activates on phrases like "show me visually", "render this", "visualize how", "generate the diagram", "draw the architecture", "let me see", "open in browser", "mock this up", "show me what this looks like". Do NOT trigger for plain text/code responses unless the user explicitly wants a visual artifact.
---

# genui

Generate self-contained HTML artifacts with live browser preview. The agent writes a single `.html` file in the user's current working directory under `.genui/artifacts/`, opens it automatically, and the browser refreshes on every update.

## Core Behavior

1. **Detect the intent** — user wants to see something visually rendered in the browser
2. **Generate the artifact** — write a complete, self-contained `.html` file
3. **Open it** — launch it in the default browser via CLI
4. **Watch for changes** — re-open (or refresh) when the artifact is updated

## Artifact Directory

```
<user's cwd>/.genui/artifacts/
```

- Save each artifact as `artifact-<timestamp>.html`
- Also maintain `index.html` in `.genui/` — a simple gallery listing all artifacts with their timestamps and a one-line description
- The agent may optionally give each artifact a short readable name (stored as a `<meta>` comment at the top of the HTML)

## Design System (Default: Cohere Aesthetic)

Apply this design language to all artifacts unless the user specifies otherwise.

### Colors

```css
:root {
  /* Surfaces */
  --bg: #ffffff;
  --surface: #eeece7;
  --dark-section: #003c33;    /* deep green */
  --navy-section: #071829;    /* dark navy */

  /* Text */
  --ink: #212121;
  --muted: #93939f;
  --slate: #75758a;

  /* Brand accents */
  --coral: #ff7759;
  --soft-coral: #ffad9b;
  --blue: #1863dc;

  /* Utility */
  --border: #d9d9dd;
  --border-light: #e5e7eb;
  --focus: #4c6ee6;
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

.caption {
  font-size: 14px;
  color: var(--muted);
}
```

### Shapes

- Cards: `border-radius: 8px`
- Media: `border-radius: 22px`
- Buttons/pills: `border-radius: 32px`
- Use rounded corners on all cards and media containers

### Elevation

- Keep it mostly flat
- Use surface alternation (white vs stone) for depth
- Thin borders (`1px solid var(--border)`) for card containment
- No heavy drop shadows

## Artifact Template (Bare Minimum)

Every artifact should have this structure:

```html
<!-- genui: <artifact name> | <short description> -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><artifact name></title>
  <style>
    /* Design tokens */
    :root {
      --bg: #ffffff; --ink: #212121; --muted: #93939f;
      --border: #d9d9dd; --coral: #ff7759; --blue: #1863dc;
      --surface: #eeece7; --dark-section: #003c33;
    }
    body { font-family: 'Inter', 'Space Grotesk', ui-sans-serif, system-ui; margin: 0; }
    /* ... styles ... */
  </style>
</head>
<body>
  <!-- content -->
  <script>
    // Enable live refresh when this file is rewritten
    // The agent can inject a timestamp or version to trigger reload
    if (window.__genui) window.__genui.reload();
  </script>
</body>
</html>
```

## Auto-Open & Live Refresh

### Opening in the Browser

Use this cross-platform approach:

**macOS:**
```bash
open "<file path>"
```

**Windows:**
```bash
start "" "<file path>"
```

**Linux:**
```bash
xdg-open "<file path>"
```

### Live Refresh

When the agent rewrites the artifact file, it should:
1. Update the file in place
2. Re-run the open command (browsers handle the "same file already open" case gracefully — it refreshes)
3. OR use a lightweight file watcher approach if available

For simplicity: **always re-open the file** — the browser will switch to it and refresh if already open.

## CDN Dependencies

Include these via CDN for the artifact:

```html
<!-- D3.js for diagrams and data viz -->
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>

<!-- Three.js for 3D -->
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>

<!-- Mermaid for flowcharts/diagrams -->
<script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>

<!-- GSAP for animations -->
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
```

Use whichever library fits the visualization type. Don't bundle all of them — include only what's needed.

## Common Artifact Patterns

### Architecture Diagram (Transformers, Neural Nets)
- Use SVG via D3 for interactive node-link diagrams
- Nodes should be labeled, positioned, and hoverable
- Edges should show direction/flow with arrows
- Include zoom/pan if the diagram is complex
- Color nodes by layer/type

### Data Flow Visualization
- Animated flow lines showing how data moves through a system
- Hover to highlight a specific path
- Use a directional layout (left-to-right or top-to-bottom)

### Interactive Form / Prototype
- Clean white surface with labeled inputs
- Immediate visual feedback on interaction
- Use Cohere's pill buttons for CTAs

### 3D Model Viewer
- Three.js canvas with orbit controls
- Simple geometric shapes for now
- Ambient + directional lighting

## Index Page

Keep `.genui/index.html` as a simple gallery:

```html
<!-- genui-index -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>genui artifacts</title>
  <style>
    body { font-family: 'Inter', ui-sans-serif; max-width: 800px; margin: 40px auto; padding: 0 20px; }
    h1 { font-size: 32px; font-weight: 400; letter-spacing: -0.02em; }
    .artifacts { list-style: none; padding: 0; }
    .artifacts li { padding: 16px 0; border-bottom: 1px solid #d9d9dd; }
    .artifacts a { font-size: 18px; text-decoration: none; color: #212121; }
    .artifacts a:hover { color: #1863dc; }
    .meta { font-size: 14px; color: #93939f; margin-top: 4px; }
  </style>
</head>
<body>
  <h1>genui artifacts</h1>
  <ul class="artifacts">
    <!-- Populated by agent on each artifact creation -->
    <li><a href="artifacts/artifact-20250101-120000.html">transformer-attention</a> <div class="meta">2025-01-01 12:00 — attention pattern visualization</div></li>
  </ul>
</body>
</html>
```

## Invocation Guide

When the user triggers this skill:

1. **Assess the request** — What do they want to see? What kind of visualization? What's the key thing they should understand from it?

2. **Design the artifact** — Choose the right format (diagram, chart, 3D, form) based on what communicates the concept best.

3. **Generate the HTML** — Write a complete, self-contained `.html` file. Apply the Cohere design system. Use CDN libraries as needed.

4. **Save to `.genui/artifacts/`** — Generate a timestamped filename. Update `.genui/index.html` to include the new artifact.

5. **Open the browser** — Run the platform-appropriate open command.

6. **On update** — Re-open the file (triggers refresh). If the user asks to modify, edit the file and re-open.

7. **Describe what was generated** — Tell the user what they're looking at, what the key elements are, and how to interact with it.

## Tips for Good Artifacts

- **One idea per artifact** — Don't cram everything into one visualization. If the request is complex, create multiple focused artifacts.
- **Interactive by default** — Add hover states, click handlers, tooltips. Static is boring.
- **Clean and minimal** — Use whitespace. Don't over-crowd. Let the visualization breathe.
- **Label everything** — Nodes, axes, sections, layers should all be clearly labeled.
- **Dark sections work well** — Use the deep green (`#003c33`) or navy (`#071829`) backgrounds for product-like mockups.
- **Use coral sparingly** — It's an accent color, not a fill. Use it for highlights and active states.
- **Responsive** — Make artifacts work on different screen sizes. Use `max-width: 100%` on all media.

## Edge Cases

- **Very large diagrams** — Use SVG with viewBox and add pan/zoom controls.
- **User is in a different directory** — Always resolve paths relative to `process.cwd()` or the user-specified path.
- **Browser already open** — Re-opening the same file refreshes it in most browsers.
- **Windows path with spaces** — Use `start "" "path with spaces.html"` to avoid misinterpretation.
- **No browser detected** — Print the file path so the user can open manually.