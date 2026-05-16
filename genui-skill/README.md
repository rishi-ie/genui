# genui

> **Generate beautiful, interactive web artifacts from your CLI agent**

**genui** is a skill for CLI coding agents that renders visual artifacts in your browser. Ask to "show me visually", and the agent generates a self-contained HTML file, opens it instantly, and updates on-the-fly.

Every artifact is a **complete, polished product** — not a sketch. Full design system, interactions, tooltips, legends, and live updates.

## What to ask for

| Type | Examples |
|------|----------|
| Architecture | "show me the system architecture", "draw the API flow" |
| Data flow | "visualize the pipeline", "show how data moves through" |
| Interactive | "create a tip calculator", "build a slider with live updates" |
| Charts | "make a dashboard", "show compound interest growth" |
| Trees | "display the file structure", "show the component tree" |
| Diagrams | "draw a neural network", "create a decision tree" |

## Quick Install

### For pi
```bash
git clone https://github.com/rishi-ie/genui ~/.pi/agent/skills/genui
```

### For Claude Code
```bash
git clone https://github.com/rishi-ie/genui ~/.claude/skills/genui
```

### For other CLI agents
Copy `genui-skill/SKILL.md` to your agent's skills directory.

## How it works

1. **Detect** — Agent sees "show me visually", "render", "visualize", etc.
2. **Plan** — What interactions make it engaging? (sliders, tooltips, live updates)
3. **Generate** — Complete artifact with design system, interactions, polish
4. **Save** — Write to `.genui/artifacts/artifact-<timestamp>.html`
5. **Open** — Launch in default browser
6. **Update** — Re-open when you ask for changes

## Design System

Every artifact uses **Figma × OpenCode**:

- **Monospace** — JetBrains Mono typography
- **Pastel blocks** — Lime headers, lilac cards, cream legend
- **Pill buttons** — `border-radius: 50px`
- **Hairline borders** — 1px subtle lines
- **Light theme only** — Clean cream canvas (#fdfcfc)

### Color Semantics (Diagrams)

| Represents | Color |
|-----------|-------|
| Input / Start | gray |
| Process / Service | blue |
| Database / Storage | teal |
| Decision | blue |
| Success | teal |
| Error | coral |
| User / Person | purple |
| External | coral |

## Complete Artifact Standard

Every artifact includes:

- ✅ Header with brand mark and title
- ✅ Full design system (CSS variables, spacing, shapes)
- ✅ Interactions (sliders, tooltips, hover states, live calculations)
- ✅ Legend for diagrams
- ✅ Zoom controls
- ✅ Polish (rounded corners, hairline borders, pastel blocks)

**Content is runtime** — sliders control outputs, tooltips show details, everything updates live.

## Templates Included

| File | Type | Features |
|------|------|----------|
| `flow.html` | Diagram | Hover tooltips, zoom, semantic colors |
| `decision-tree.html` | Diagram | Branching paths, hover info |
| `slider.html` | Interactive | Sliders, live calculations, breakdown |
| `compound-interest.html` | Interactive | Chart.js growth chart, multiple inputs |
| `api-architecture.html` | Diagram | Multi-tier system, semantic colors |
| `transformer-interactive.html` | Diagram | Full transformer architecture |
| `hierarchy-tree.html` | Diagram | D3 tree layout |
| `neural-network.html` | Diagram | Network visualization |
| `data-flow.html` | Animation | Animated particles |
| `architecture-diagram.html` | Diagram | Generic system diagram |
| `3d-viewer.html` | 3D | Three.js orbit controls |
| `interactive-form.html` | Form | Form prototype |

## CDN Dependencies

```html
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
```

## SVG Rules

- **viewBox width = 680** — ensures 1:1 pixel mapping
- **Box width formula** — prevents text overflow
- **Semantic colors** — not random pastels
- **≤3 color ramps** per diagram

See `DESIGN_GUIDELINES.md` for complete rules.

## Artifact Storage

```
.genui/
├── artifacts/
│   ├── artifact-20250601-143022.html
│   ├── artifact-20250601-150045.html
│   └── index.html          ← gallery
├── SKILL.md                 ← agent prompt
└── DESIGN_GUIDELINES.md    ← generation rules
```

## License

MIT

---

**Built for CLI coding agents** | [GitHub](https://github.com/rishi-ie/genui)