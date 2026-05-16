# genui

> Generate beautiful, interactive web artifacts from your CLI agent

**genui** is a skill for CLI coding agents that renders visual artifacts in your browser. Ask to "show me visually", and the agent generates a self-contained HTML file, opens it instantly, and updates on-the-fly.

## What it does

```
You: "show me the transformer architecture"
Agent: *generates artifact* *opens browser*
→ Beautiful interactive diagram appears
```

**Perfect for:**
- Architecture diagrams (systems, microservices, APIs)
- Neural network visualizations
- Data flow / pipeline diagrams
- File/component hierarchy trees
- Transformer/LLM architecture diagrams
- Interactive forms and mockups
- 3D model previews

## Design System

Artifacts use a **Figma × OpenCode** fused aesthetic:

- **Monospace everywhere** — JetBrains Mono typography
- **Pastel color blocks** — Lime headers, lilac cards, cream legends
- **Pill buttons** — `border-radius: 50px`
- **Hairline borders** — 1px subtle lines
- **Light theme only** — Clean cream canvas (#fdfcfc)

## Quick Install

### For pi (recommended)

```bash
# Clone into your skills directory
git clone https://github.com/rishi-ie/genui ~/.pi/agent/skills/genui
```

Or copy manually:
```bash
cp -r genui-skill ~/.pi/agent/skills/genui
```

### For Claude Code

```bash
# Clone into your skills directory
git clone https://github.com/rishi-ie/genui ~/.claude/skills/genui
```

### For other CLI agents

1. Copy `genui-skill/SKILL.md` to your agent's skills directory
2. Or add its contents directly to your agent's system prompt
3. Each agent has different skill loading — check its docs

## Usage

Just ask your agent naturally:

```
"show me the transformer architecture"
"visualize the attention mechanism"
"draw the data pipeline flow"
"render the neural network"
"let me see the component tree"
"mock up this form"
"show me what this looks like"
```

The agent will:
1. Design the right visualization for your request
2. Generate a complete `.html` artifact
3. Save it to `<cwd>/.genui/artifacts/`
4. Open it in your default browser
5. Update it when you ask for changes

## Artifact Gallery

All artifacts are saved to `.genui/artifacts/` with timestamps. An `index.html` gallery lets you revisit previous artifacts.

```
genui/
├── artifacts/
│   ├── artifact-20250601-143022.html
│   ├── artifact-20250601-150045.html
│   └── index.html          ← gallery
└── SKILL.md                 ← agent prompt
```

## Templates Included

| File | What it renders |
|------|-----------------|
| `transformer-interactive.html` | Full transformer architecture (Vaswani et al.) |
| `architecture-diagram.html` | Generic node-link system diagrams |
| `data-flow.html` | Animated pipeline with particles |
| `hierarchy-tree.html` | D3 tree for file/component structures |
| `neural-network.html` | Interactive neural net visualization |
| `3d-viewer.html` | Three.js 3D model viewer |
| `interactive-form.html` | Clean form prototype |

## CDN Dependencies

These are available via CDN — the agent includes them as needed:

```html
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
```

## Design Tokens

```css
/* Colors */
--block-lime: #d4ff8a;      /* Headers */
--block-lilac: #e6c8ff;     /* Cards */
--block-cream: #fef7e6;     /* Legend */
--block-mint: #c8ffec;       /* Accents */
--ink: #201d1d;             /* Text */
--canvas: #fdfcfc;          /* Background */
--accent: #007aff;          /* Links, highlights */

/* Spacing */
--space-sm: 8px;
--space-lg: 16px;
--space-xl: 24px;
--space-xxl: 32px;

/* Shapes */
--rounded-pill: 50px;       /* Buttons */
--rounded-lg: 24px;         /* Cards */
--rounded-md: 8px;          /* Components */
```

## Browser Opening

The skill auto-detects your OS:

```bash
# Windows
start "" "path/to/file.html"

# macOS
open "path/to/file.html"

# Linux
xdg-open "path/to/file.html"
```

## Contributing

The agent uses these templates as reference patterns. To add a new template:
1. Create `templates/your-template.html`
2. Include the design system CSS variables
3. Add a comment header: `<!-- genui: your-template | description -->`
4. Test by opening in browser

## License

MIT — do whatever you want with it.

---

**Built for CLI coding agents** | [GitHub](https://github.com/rishi-ie/genui)
