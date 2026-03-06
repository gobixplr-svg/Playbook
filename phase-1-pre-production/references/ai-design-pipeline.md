# AI-Powered Design Pipeline

## Overview

This reference documents an AI-assisted design workflow that accelerates Steps 4 (UI/UX Design) and 5 (Design System). All tools listed are free or near-free. The pipeline is optional — the step guides work without it — but it dramatically speeds up visual exploration for solo developers.

## Tool Stack

| Phase | Tool | Cost | Purpose |
|-------|------|------|---------|
| Visual direction | Midjourney | $10 one-time | Mood boards, color exploration, atmosphere |
| Color system | Khroma + Coolors | Free | AI color palette generation, accessibility checking |
| Screen generation | Google Stitch | Free | Text-to-UI design, multi-screen flows, Figma export |
| Design refinement | Figma (Starter) | Free | Polish designs, build components, prototype flows |
| Design-to-code | Codia AI + Figma MCP | Free | Figma → SwiftUI code bridge, Claude Code integration |

**Pipeline:** Midjourney (mood) → Khroma/Coolors (color) → Stitch (screens) → Figma (refine) → Codia/MCP (SwiftUI)

## Midjourney for Visual Direction

Use Midjourney to explore visual atmosphere before committing to a design direction.

**Prompt structure:**
```
[app concept] app, [visual style], [mood adjectives], mobile UI, --ar 9:16 --stylize [value]
```

**Key parameters:**
- `--ar 9:16` — mobile aspect ratio
- `--stylize 50-200` — lower = more literal, higher = more artistic
- `--sref [URL]` — style reference from an existing image

**Process:**
1. Generate 10-20 images exploring different moods and visual directions
2. Select 3-5 that resonate with your app's personality
3. Extract patterns: What colors repeat? What textures? What spatial relationships?
4. Use these as reference inputs for color palette and screen design tools

## Khroma + Coolors for Color Systems

**Khroma** (khroma.co): Train an AI on colors you like, then generate harmonious palettes.
**Coolors** (coolors.co): Generate and refine palettes with contrast checking.

**Process:**
1. Start from Midjourney palette extraction or brand colors
2. Generate variations in Khroma
3. Test accessibility (WCAG AA contrast) in Coolors
4. Export as design tokens (hex values for light/dark mode)

## Google Stitch for Screen Design

Stitch generates mobile UI designs from text descriptions with Figma export.

**Prompt structure:**
```
Design a [screen type] for [app name], a [app description].
Show [specific elements].
Style: [visual direction from mood boards].
```

**Tips:**
- Generate 3-4 variants per screen and pick the best elements from each
- Stitch handles layout well but may miss brand consistency — refine in Figma
- Export to Figma for component extraction and prototyping

## Figma Refinement

Use Figma (free Starter plan) to:
1. Import Stitch exports
2. Apply your design tokens (colors, typography, spacing)
3. Extract reusable components
4. Build interactive prototypes for user flow validation

## Codia AI + Figma MCP Bridge

Use Codia AI or the Figma MCP server to translate Figma designs into SwiftUI code:
- Codia generates SwiftUI views from Figma frames
- Figma MCP lets Claude Code read your Figma files directly

**Important:** Generated code is a starting point, not production code. Extract design values (colors, spacing, layout structure) rather than using generated views directly.
