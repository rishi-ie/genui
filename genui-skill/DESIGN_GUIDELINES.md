# genui v2 — Design & Generation Guidelines

These are the design rules and SVG/HTML generation patterns for genui artifacts. Apply them when generating any visual output.

---

## Core Philosophy

- **Seamless**: Users shouldn't notice where the agent ends and the widget begins.
- **Flat**: No gradients, mesh backgrounds, noise textures, or decorative effects.
- **Compact**: Show the essential inline. Explain the rest in prose.
- **Text goes in your response, visuals go in the artifact** — all explanatory prose stays in the chat.

---

## Complexity Budget (Hard Limits)

| Rule | Limit |
|------|-------|
| Box subtitles | ≤5 words. Detail goes in hover/click panels. |
| Colors | ≤3 ramps per diagram. More = noise. |
| Horizontal tier | ≤4 boxes at ~140px each. 5+ → shrink to ≤110px or wrap. |

---

## SVG Generation Rules

### viewBox is Load-Bearing

```svg
<svg width="100%" viewBox="0 0 680 H">
```

**Never change the 680.** It matches the widget container width so coordinate units = CSS pixels 1:1.

- `viewBox="0 0 480 H"` in a 680px container scales by 680/480 = 1.42×
- Your 14px text renders at ~20px — too large
- Keep viewBox at 680 and center narrow content (e.g., x=180..500)

### Box Width Formula

```
box_width = max(title_chars × 8, subtitle_chars × 7) + 24
```

**Example**: 
- Title "Authentication Service" = 22 chars → 22 × 8 = 176px
- Subtitle "JWT validation" = 15 chars → 15 × 7 = 105px  
- **Minimum box width = 200px**

### Pre-Built SVG Classes

```css
/* Text (already loaded in artifacts) */
.t    { font: 14px sans-serif; font-weight: 500; }
.ts   { font: 12px sans-serif; font-weight: 400; }
.th   { font: 14px sans-serif; font-weight: 500; }

/* Shapes */
.box  { fill: var(--color-background-secondary); stroke: var(--color-border-tertiary); }
.node { cursor: pointer; }
.arr  { marker-end: url(#arrow); }
.leader { stroke-dasharray: 4 4; }

/* Color ramps */
.c-gray  { fill: #F1EFE8; stroke: #5F5E5A; }
.c-blue  { fill: #E6F1FB; stroke: #185FA5; }
.c-teal  { fill: #E1F5EE; stroke: #0F6E56; }
.c-purple { fill: #EEEDFE; stroke: #534AB7; }
.c-coral { fill: #FAECE7; stroke: #993C1D; }
.c-pink  { fill: #FBEAF0; stroke: #993556; }
.c-amber { fill: #FAEEDA; stroke: #854F0B; }
.c-green { fill: #EAF3DE; stroke: #3B6D11; }
.c-red   { fill: #FCEBEB; stroke: #A32D2D; }
```

### Arrow Marker (Include in Every SVG)

```svg
<defs>
  <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" 
          markerWidth="6" markerHeight="6" orient="auto-start-reverse">
    <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" 
          stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
  </marker>
</defs>
```

Then use `marker-end="url(#arrow)"` on lines. The head inherits `context-stroke` — a gray line gets a gray head.

### ViewBox Safety Checklist

1. Find lowest element: `max(y + height)` across all rects + 40px buffer
2. Find rightmost element: `max(x + width)` must be ≤ 680
3. For `text-anchor="end"`: `anchor_x - (label_chars × 8)` must be ≥ 0
4. Never use negative x or y coordinates
5. Check arrow intersections: if a line crosses a box's interior, use L-shaped detour

### Text Rules

| Class | Size | Weight | Use |
|-------|------|--------|-----|
| `.th` | 14px | 500 | Node titles |
| `.t`  | 14px | 400 | Region labels |
| `.ts` | 12px | 400 | Subtitles, descriptions |

- **Sentence case** always. Never Title Case or ALL CAPS.
- No mid-sentence bolding. Use `code style` for names.
- SVG text never auto-wraps. Use `<tspan>` for multi-line.

### Color Assignment Rules

Color should encode **meaning**, not sequence:

| If node represents... | Use ramp |
|-----------------------|----------|
| Input / Start | c-gray |
| Process / Service | c-blue |
| Database / Storage | c-teal |
| Error / Danger | c-red |
| Success | c-green |
| Warning | c-amber |
| User / Person | c-purple |
| External / API | c-coral |
| Generic step | c-gray |

**Max 3 colors per diagram.** Example: c-gray + c-blue + c-teal for an API flow.

### Text on Colored Backgrounds

Always use the darkest stop from the same ramp:

| Fill | Title | Subtitle |
|------|-------|----------|
| c-blue (light) | Blue 800 | Blue 600 |
| c-teal (light) | Teal 800 | Teal 600 |

**Never use black or gray on colored backgrounds.**

---

## HTML Generation Rules

### Fragment Format (No DOCTYPE/html/head/body)

```html
<!-- Good: just content -->
<div style="display: flex; gap: 12px;">
  <span style="font-size: 14px;">Label</span>
  <input type="range" min="0" max="100" value="50">
</div>

<!-- Bad: wrapping boilerplate -->
<!DOCTYPE html>
<html><head><title>Widget</title></head><body>...</body></html>
```

### CSS Variables Available

```css
/* Backgrounds */
--color-background-primary: #ffffff
--color-background-secondary: #f5f5f5

/* Text */
--color-text-primary: #1a1a1a
--color-text-secondary: #666666

/* Borders */
--color-border-tertiary: rgba(0,0,0,0.15)
--color-border-secondary: rgba(0,0,0,0.3)

/* Semantic */
--color-text-info: #0066cc
--color-text-danger: #cc0000
--color-text-success: #006633

/* Layout */
--border-radius-md: 8px
--border-radius-lg: 12px
```

### Component Patterns

**Metric Card:**
```html
<div style="background: var(--color-background-secondary); 
            border-radius: var(--border-radius-md); 
            padding: 1rem;">
  <div style="font-size: 13px; color: var(--color-text-secondary);">Revenue</div>
  <div style="font-size: 24px; font-weight: 500;">$1.2M</div>
</div>
```

**Slider with Label:**
```html
<div style="display: flex; align-items: center; gap: 12px;">
  <label style="font-size: 14px;">Years</label>
  <input type="range" min="1" max="40" value="20" style="flex: 1;">
  <span id="years-out" style="font-size: 14px; font-weight: 500;">20</span>
</div>
<script>
  document.getElementById('years').addEventListener('input', 
    e => document.getElementById('years-out').textContent = e.target.value);
</script>
```

**Raised Card:**
```html
<div style="background: var(--color-background-primary); 
            border: 0.5px solid var(--color-border-tertiary);
            border-radius: var(--border-radius-lg); 
            padding: 1rem 1.25rem;">
  <!-- content -->
</div>
```

### Number Formatting

Always round displayed numbers:
```javascript
Math.round(value)           // integers
value.toFixed(2)           // 2 decimals
Intl.NumberFormat().format(value)  // locale-aware
```

**Wrong**: `0.1 + 0.2 = 0.30000000000000004`  
**Right**: `Math.round(0.1 + 0.2 * 10) / 10 = 0.3`

---

## CDN Allowlist

Artifacts may only load from:
- `cdnjs.cloudflare.com`
- `esm.sh`
- `cdn.jsdelivr.net`
- `unpkg.com`

**Chart.js CDN:**
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
```

---

## Module Selection Guide

| Request Type | Module(s) | Key Rules |
|--------------|-----------|-----------|
| Architecture diagram | diagram | viewBox=680, box formula, color ramps |
| Interactive explainer | interactive | Sliders, live calc, send data |
| Dashboard / metrics | chart + mockup | Chart.js, metric cards |
| UI mockup | mockup | Cards, forms, pre-styled inputs |
| SVG illustration | art | Fill canvas, bold colors, path curves |
| Data flow | diagram | Arrows, tiers, labels |

---

## Artifact File Structure

```
<cwd>/.genui/artifacts/
├── artifact-<timestamp>.html    # Generated artifact
├── index.html                  # Gallery with timestamps
└── .genui.json                 # Config (optional)
```

---

## Browser Auto-Open

```bash
# Windows
start "" ""

# macOS
open ""

# Linux
xdg-open ""
```

For live refresh, use a file watcher that re-opens on change.