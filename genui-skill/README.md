# genui

Generate beautiful, interactive web artifacts (diagrams, charts, 3D models, forms) from your CLI agent. Artifacts auto-open in the browser with live refresh.

## What is this?

A skill for CLI coding agents (Claude Code, pi, openclaw, etc.) to render visual artifacts in the browser. When you ask to "show me visually", "render this", "visualize the architecture", the agent generates a self-contained HTML file, opens it in your browser, and updates it on-the-fly.

**Use cases:**
- Transformer architecture diagrams
- Neural network visualizations  
- Data flow diagrams
- Component trees
- 3D model previews
- Interactive mockups

## Design

Artifacts use a clean, enterprise aesthetic inspired by Cohere — white surfaces, tight typography, coral accents, and rounded cards.

## Installation

### For pi
```bash
# Copy the skill to your skills directory
cp -r genui-skill ~/.pi/agent/skills/genui/
```

### For Claude Code
```bash
# Copy the skill to your .claude/skills directory  
cp -r genui-skill ~/.claude/skills/genui/
```

### For other agents
Add the `SKILL.md` content to your agent's prompt or skills directory according to its documentation.

## Usage

Just ask your agent to show you something visually:

```
"show me the transformer architecture"
"visualize how attention works"
"render the data flow"
"let me see the component tree"
"show me what this mockup looks like"
```

The agent will:
1. Generate a complete `.html` artifact
2. Save it to `.genui/artifacts/` in your current directory
3. Open it in your browser
4. Update it on-the-fly when you ask for changes

## Artifact Gallery

All artifacts are stored in `.genui/artifacts/` and listed in `.genui/index.html` so you can revisit them later.

## CDN Libraries Available

Artifacts can use these via CDN:
- **D3.js** — diagrams, charts, data viz
- **Three.js** — 3D rendering
- **Mermaid** — flowcharts
- **GSAP** — animations