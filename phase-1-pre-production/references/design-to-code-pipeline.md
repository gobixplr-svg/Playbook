# Reference: Design-to-Code Pipeline

How to go from a visual design file to production code using AI tools. Covers both the recommended Figma MCP workflow and the XD fallback workflow.

---

## The Core Problem

A designer hands you a design file. You need to produce code that matches it exactly. The accuracy of your code depends entirely on how much design information the AI can access.

**More structured data = fewer revision rounds.**

| Input Quality | What AI Gets | Typical Revision Rounds |
|---|---|---|
| "Build me a hero section" | Nothing | 5-10 |
| Screenshot of the design | Visual reference only | 3-5 |
| Screenshot + manual measurements | Visual + some specs | 2-3 |
| Figma MCP structured data | Layout, tokens, hierarchy, components | 1 |

---

## Option A: Figma + MCP Server (Recommended)

Figma's Model Context Protocol server gives AI tools direct access to design file data. Instead of describing the design in words, the AI reads it.

### What the AI Gets

Three tools available via MCP:

1. **`get_design_context`** — Returns structured representation of a selected frame: element hierarchy, layout rules (Auto Layout → flex/grid), text content, component properties, spacing relationships.

2. **`get_variable_defs`** — Returns all design tokens (Figma Variables) used in the selection: colors, spacing values, typography definitions, with semantic names and alias chains.

3. **`get_screenshot`** — Returns a visual reference image of the frame for layout validation.

### Workflow

```
1. Designer: builds in Figma, uses Auto Layout, defines Variables
2. Developer: shares Figma frame URL with Claude Code
3. Claude: reads design context + variables + screenshot via MCP
4. Claude: generates code using actual design tokens
5. Developer: visual QA at mobile + desktop (Tier 1.5)
6. Developer: provides specific feedback if needed (usually 0-1 rounds)
```

### Structuring Figma Files for AI

The better the Figma file is organized, the better the AI output:

- **Name layers semantically:** `hero-section`, `cta-button`, `testimonial-card` — not `Frame 47`, `Group 12`
- **Use Auto Layout everywhere:** Claude reads this as flex/grid. Without it, Claude only gets absolute positioning.
- **Define Variables for design tokens:** Colors, spacing, typography as Figma Variables with semantic names (`color/primary`, `spacing/md`)
- **Use Components for reusable elements:** Claude recognizes these as reusable code components
- **Keep hierarchy shallow:** 3-4 nesting levels max

### Setup

For a detailed setup walkthrough, see the Figma MCP setup guide in your project's AI package or reference the Figma MCP documentation at `developers.figma.com/docs/figma-mcp-server/`.

---

## Option B: XD + Manual Spec Extraction (Fallback)

When the design team uses Adobe XD (or any tool without an MCP integration), you extract specs manually and provide them to the AI as text.

### Workflow

```
1. Designer: builds in XD
2. Developer: opens XD file, inspects each section
3. Developer: extracts colors, fonts, spacing, layout from the inspect panel
4. Developer: provides specs to Claude Code as structured text
5. Claude: generates code from the text specs
6. Developer: visual QA at mobile + desktop (Tier 1.5)
7. Developer: multiple rounds of "this doesn't match" feedback
```

### How to Extract Specs from XD

For each section, capture:

```
Section: Hero
- Background: #000000
- Layout: 2-column grid (645px left / flexible right) on desktop, single column mobile
- Padding: 56px top/bottom
- H1: 50px/60px Neue Haas Grotesk Bold, white, left-aligned
- Body: 20px/24px Libre Franklin Regular, white
- Stats: 36px bold numbers, 12px muted labels, 4-column row
- Form inputs: #1A1A1A background, 1px rgba(255,255,255,0.15) border, 3px radius
- CTA button: #FCD002 background, black text, bold 16px, full width, 3px radius
```

### Common XD-to-Code Mistakes

**Specificity conflicts:** The XD gives you exact values, but your CSS framework (Bootstrap, WordPress theme) may have competing rules. Always check specificity when styles don't apply.

**Mobile-desktop ordering:** XD shows mobile and desktop as separate artboards. The HTML structure needs to accommodate both with CSS media queries and potentially flexbox `order` or grid areas. Verify the mobile content order explicitly — it often differs from desktop.

**Approximate values:** XD rounds measurements. A developer clicking on elements might see "296px" when the actual position should be "50% of container." Use logical values (percentages, calc, auto margins) over magic numbers.

**Missing states:** XD typically shows the default state. Ask the designer about: hover, focus, active, loading, empty, error states. These are commonly missed in handoff.

---

## Design Token Extraction

Regardless of tool (Figma or XD), the first build step is always: extract the design system into CSS custom properties.

### What to Extract

| Category | What to Capture | CSS Variable Pattern |
|---|---|---|
| Colors | Brand primary, secondary, accent, neutrals, backgrounds | `--color-primary`, `--color-bg-dark` |
| Typography | Display font, body font, weights used, loading method | `--font-display`, `--font-body` |
| Spacing | Padding, margin, gap patterns (build a consistent scale) | `--space-xs` through `--space-2xl` |
| Layout | Container max-width, horizontal padding, breakpoints | `--container-max`, `--container-pad` |
| Borders | Corner radii, border colors/widths | `--radius`, `--radius-lg` |

### Output

```css
:root {
    --color-primary: #000000;
    --color-accent: #FCD002;
    --color-bg-dark: #141414;
    --font-display: "Neue Haas Grotesk Display", sans-serif;
    --font-body: "Libre Franklin", sans-serif;
    --space-md: 16px;
    --space-lg: 24px;
    --container-max: 1400px;
    --radius: 3px;
}
```

This goes into the project CLAUDE.md and custom.css. Every component references tokens, never raw values. When the client changes their brand color, you update one line.

---

## Tier 1.5 Visual QA for Design Accuracy

After building each section, before showing the client:

1. **Mobile (375px):** Open in browser DevTools responsive mode. Compare to mobile design frame side-by-side.
2. **Desktop (1400px):** Full browser width. Compare to desktop design frame.
3. **Check:** Colors match hex values. Font sizes/weights correct. Spacing matches. Content order correct on mobile. No overflow or clipping.
4. **Fix mismatches** before moving to the next section.

This is the single most effective quality gate for design accuracy. It catches the issues that generate client revision rounds.

---

## Choosing a Pipeline

| Factor | Figma + MCP | XD + Manual |
|---|---|---|
| Setup effort | Medium (MCP config, Figma Variables) | None |
| Per-section effort | Low (share link, AI reads) | High (inspect, extract, describe) |
| Accuracy | High (structured data) | Medium (human transcription errors) |
| Revision rounds | 0-1 | 2-5 |
| Maintenance | Figma subscription ($15/editor/month) | Free |
| Future-proof | Yes (active ecosystem) | No (XD discontinued 2023) |

**Recommendation:** Migrate to Figma when feasible. Use XD fallback when the design team isn't ready to switch. The AI package supports both workflows today.
