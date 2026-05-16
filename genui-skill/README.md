# genui

> **Generate beautiful, interactive web artifacts from your CLI agent**

**genui** is a skill for CLI coding agents that renders visual artifacts in your browser. Ask to "show me visually", and the agent generates a self-contained HTML file, opens it instantly, and updates on-the-fly.

Based on principles from Claude.ai's generative UI system, with simplified tooling for any agent.

## What it does

```
You: "show me the transformer architecture"
Agent: *generates artifact* *opens browser*
→ Beautiful interactive diagram appears
```

## What to ask for

| Type | Examples |
|------|----------|
| Architecture | "show me the system architecture", "draw the API flow" |
| Data flow | "visualize the pipeline", "show how data moves through" |
| Interactive | "create a compound interest calculator", "build a slider" |
| Charts | "make a dashboard", "show this as a bar chart" |
| Trees | "display the file structure", "show the component tree" |
| Diagrams | "draw a neural network", "explain how X works visually" |

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
2. **Select module** — Choose right pattern:
   - `diagram` — SVG with nodes and arrows
   - `interactive` — HTML with sliders, live calculations
   - `chart` — HTML + Chart.js
   - `mockup` — UI components, forms, cards
3. **Generate** — Write complete HTML using design guidelines
4. **Save** — Write to `.genui/artifacts/artifact-<timestamp>.html`
5. **Open** — Launch in default browser
6. **Update** — Re-open when you ask for changes

## Design System

Artifacts use **Figma × OpenCode** — monospace typography with pastel blocks.

### Color Semantics (Diagrams)

| What it represents | Color |
|---------------------|-------|
| Input / Start | gray |
| Process / Service | blue |
| Database / Storage | teal |
| Decision | amber |
| Success | green |
| Error | red |
| User / Person | purple |
| External / API | coral |

### Key Rules

- **viewBox width = 680** — ensures 1:1 pixel mapping
- **Subtitles ≤5 words** — detail goes in hover/click panels
- **≤3 color ramps per diagram** — more = visual noise
- **Sentence case** — never Title Case or ALL CAPS

See `DESIGN_GUIDELINES.md` for complete rules.

## Templates Included

| File | What it renders |
|------|-----------------|
| `transformer-interactive.html` | Transformer architecture (Vaswani et al.) |
| `architecture-diagram.html` | Generic node-link system diagrams |
| `data-flow.html` | Animated pipeline with particles |
| `hierarchy-tree.html` | D3 tree for file/component structures |
| `neural-network.html` | Interactive neural net visualization |
| `3d-viewer.html` | Three.js 3D model viewer |
| `interactive-form.html` | Clean form prototype |

## CDN Dependencies

```html
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
```

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

## Contributing

The templates show the expected patterns. To add a new template:
1. Create `templates/your-template.html`
2. Follow the design guidelines
3. Add header: `<!-- genui: your-template | description -->`
4. Test by opening in browser

## Credits

Based on principles from [pi-generative-ui](https://github.com/Michaelliv/pi-generative-ui) by Michaelliv — the reverse-engineered Claude.ai generative UI system for pi.

Design rules adapted from Claude.ai's visualize guidelines.

## License

MIT

---

**Built for CLI coding agents** | [GitHub](https://github.com/rishi-ie/genui)