# genui

> **Generate beautiful, interactive web artifacts from your CLI agent**

**genui** is a skill for CLI coding agents that renders visual artifacts in your browser. Every artifact is a **complete, polished product** — not a sketch or wireframe.

Based on design principles from [Impeccable](https://github.com/pbakaus/impeccable) — OKLCH color theory, spatial rhythm, motion without bounce, and anti-pattern avoidance.

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

## Design Laws

### Color

**Never use pure black (#000) or pure white (#fff).** Everything is tinted toward the brand hue with tiny chroma (0.008-0.018).

**OKLCH color space** — perceptually uniform, so lightness steps look equal.

**One accent ≤10% surface.** Use color deliberately, not by reflex.

### Typography

- **Font**: JetBrains Mono (monospace is the aesthetic — it's intentional)
- **No em dashes (—)**. Use commas, colons, or periods.
- **Vary weight for hierarchy** — at least 100 units between heading and body.

### Layout

- **Vary spacing for rhythm.** Same padding everywhere is monotony.
- **Cards are lazy.** Use them only when truly necessary. Nested cards are always wrong.
- **Don't wrap everything.** Most things don't need a container.

### Motion

- **Ease out with exponential curves** (`ease-out-quart`, `ease-out-expo`).
- **Never bounce or elastic.** Looks dated.
- **Don't animate layout properties.**

### Anti-Patterns (Never Do These)

| Ban | Instead |
|-----|---------|
| Side-stripe borders (`border-left > 1px`) | Full borders, background tints, icons |
| Gradient text | Solid color. Emphasis via weight/size. |
| Identical card grids | Vary sizes, weights, content |
| Modal as first thought | Inline, progressive disclosure |

## Semantic Colors (Diagrams)

Assign by meaning, not aesthetics:

| Represents | Color |
|-----------|-------|
| Input / Start | neutral gray |
| Process / Service | blue |
| Database / Storage | teal |
| Decision | amber |
| Success | green |
| Error | red |
| User / Person | purple |
| External | coral |

**Max 3 colors per diagram.** Gray + 2 accents is cleaner than rainbow.

## Templates Included

| File | Type | Features |
|------|------|----------|
| `flow.html` | Diagram | Hover tooltips, zoom, semantic colors |
| `decision-tree.html` | Diagram | Branching paths, hover info |
| `slider.html` | Interactive | Sliders, live calculations, breakdown |
| `compound-interest.html` | Interactive | Chart.js growth chart |
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

## Artifact Storage

```
.genui/
├── artifacts/
│   ├── artifact-20250601-143022.html
│   └── index.html          ← gallery
└── SKILL.md                 ← agent prompt
```

## Credits

Design principles inspired by [Impeccable](https://github.com/pbakaus/impeccable) by Paul Bakaus — 23 commands for frontend design quality.

## License

MIT

---

**Built for CLI coding agents** | [GitHub](https://github.com/rishi-ie/genui)