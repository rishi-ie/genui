# genui

> **Generate beautiful, interactive web artifacts from your CLI agent**

**genui** is a skill for CLI coding agents that renders visual artifacts in your browser. Every artifact is a **complete, polished product** — with sliders, charts, tooltips, and live calculations.

Based on design principles from [Impeccable](https://github.com/pbakaus/impeccable) — spatial rhythm, motion without bounce, and anti-pattern avoidance.

## What to ask for

| Type | Examples |
|------|----------|
| Architecture | "show me the system architecture", "draw the API flow" |
| Data flow | "visualize the pipeline", "show how data moves through" |
| Interactive | "create a tip calculator", "build a slider with live updates" |
| Charts | "make a dashboard", "show compound interest growth" |
| Trees | "display the file structure", "show the component tree" |
| Diagrams | "draw a neural network", "create a decision tree" |

## Install

### For pi

```bash
git clone https://github.com/rishi-ie/genui ~/.pi/agent/skills/genui
```

Then restart pi or run `/skills` to activate.

### For Claude Code

```bash
git clone https://github.com/rishi-ie/genui ~/.claude/skills/genui
```

Skills auto-load on restart.

### For Cursor

```bash
# Enable Skills in Settings → Beta → Agent Skills
git clone https://github.com/rishi-ie/genui ~/.cursor/skills/genui
```

### For OpenCode

```bash
git clone https://github.com/rishi-ie/genui ~/.opencode/skills/genui
```

### For other agents

Copy `genui-skill/SKILL.md` to your agent's skills directory. Each agent loads skills differently — check their docs.

## How it works

1. **Ask** — "show me visually", "render this", "create a chart"
2. **Agent generates** — Complete HTML artifact with design system + interactions
3. **Browser opens** — Artifact appears in your default browser
4. **Interact** — Sliders, tooltips, live calculations all work
5. **Update** — Agent re-generates when you ask for changes

## Design Laws

### Color

- **Never pure black/white** — everything tinted slightly
- **One accent ≤10% surface** — use color deliberately

### Typography

- **JetBrains Mono** everywhere — intentional design choice
- **No em dashes (—)** — use commas or periods

### Motion

- **Ease out** — `ease` or `ease-out`, no bounce/elastic
- **Don't animate layout properties**

### Anti-Patterns (Never Do)

| Ban | Instead |
|-----|---------|
| Side-stripe borders | Full borders, icons |
| Gradient text | Solid color |
| Identical card grids | Vary sizes/content |
| Modal as first thought | Inline, progressive |

## Semantic Colors (Diagrams)

| Represents | Color |
|-----------|-------|
| Input / Start | gray |
| Process / Service | blue |
| Database / Storage | teal |
| Decision | blue |
| Success | green |
| Error | red |

**Max 3 colors per diagram.**

## Templates

3 working templates demonstrating interactions:

| File | What it does |
|------|--------------|
| `flow.html` | Diagram with hover tooltips + zoom |
| `slider.html` | Tip calculator with live math |
| `compound-interest.html` | Investment calculator + Chart.js graph |

## CDN Dependencies

Available in every artifact:

```html
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
```

## Artifact Storage

```
<cwd>/.genui/artifacts/
├── artifact-20250601-143022.html
└── index.html
```

## Credits

- Design principles from [Impeccable](https://github.com/pbakaus/impeccable) by Paul Bakaus
- Inspired by [pi-generative-ui](https://github.com/Michaelliv/pi-generative-ui)

## License

MIT

---

**Built for CLI coding agents** | [GitHub](https://github.com/rishi-ie/genui)