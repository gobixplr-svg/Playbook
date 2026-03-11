# Asset Creation Pipeline

## Overview

This reference documents how to create, optimize, and manage visual assets for app development. It covers every asset category from app icons to store screenshots, with tool recommendations and format specifications for each.

This guide is advisory — review it during [Step 5.8 (Asset Planning)](../steps/05-design-system.md) to decide what your app needs, then return to specific sections as you create assets during production and launch.

## Tool Recommendations

No single tool covers every asset type. Choose based on what you're creating:

| Asset Type | Recommended Tools | Cost | Notes |
| ---------- | ----------------- | ---- | ----- |
| App icon exploration | Midjourney, DALL-E, Ideogram | $10-20/mo | Great for rapid concept exploration |
| App icon refinement | Figma, Affinity Designer, Sketch | Free-$70 | Vector tools for final polish |
| SF Symbol customization | SF Symbols app (Apple) | Free | Export custom symbols matching Apple's grid |
| Custom icons | Figma, Affinity Designer | Free-$70 | Match SF Symbol weight and grid |
| Illustrations | Midjourney, DALL-E, Illustrations.co | $0-20/mo | AI for custom; stock for speed |
| Screenshots | [AppShot](https://outofofficeoutdoors.com/products/appshot) (free), ScreenshotBot, Rotato | Free-$50 | Device frames, text overlays, App Store sizing |
| Batch resizing | [BatchSnap](https://outofofficeoutdoors.com/products/batchsnap) (free) | Free | Browser-based batch resize, app icon generation, format conversion |
| Preview video | Rotato, Screen Studio, QuickTime | $30-50 | Device-framed screen recordings |
| Marketing graphics | Figma, Canva | Free | Templates for social cards, press kits |

**Principle:** Use AI generation for exploration and concept development. Use design tools for final production assets. AI output is a starting point, not a deliverable.

## App Icon

The app icon is the most visible brand asset. It appears on the home screen, App Store, notifications, settings, and Spotlight search. Invest accordingly.

### Apple Requirements (iOS)

- **Single asset:** Provide one 1024x1024px PNG. Xcode automatically generates all required sizes.
- **No transparency:** The icon must be opaque. iOS applies the rounded-rectangle mask automatically.
- **No text:** Text is illegible at small sizes (29pt on Settings). Use a glyph or abstract mark.
- **sRGB color space:** Required for consistent rendering across devices.
- **No embedded layers:** Export as a flat PNG, not PSD or layered format.

### Size Rendering Contexts

Test your icon at these sizes — it must be recognizable at each:

| Size | Context |
| ---- | ------- |
| 1024pt | App Store listing (full detail visible) |
| 120pt | Home screen (iPhone) |
| 80pt | Spotlight search |
| 60pt | Home screen (compact) |
| 40pt | Notification |
| 29pt | Settings |

### Creation Workflow

1. **Explore concepts (AI):** Generate 20-30 icon concepts with Midjourney or similar. Prompt structure: `[app concept] app icon, [style], iOS app icon, simple glyph, gradient background, --ar 1:1 --stylize 100`
2. **Narrow to 3-5 candidates:** Select icons that read well when squinted at (the small-size test)
3. **Refine in vector tool:** Recreate the best concept in Figma or Affinity Designer for clean edges and exact color control
4. **Test at all sizes:** Export at 1024px, then view at Settings size (29pt) — if the concept doesn't read, simplify
5. **Test on device:** AirDrop to your phone, set as the icon, and live with it for a day before committing
6. **Dark mode / tinted:** iOS 18+ supports tinted and dark mode icon variants. Provide these for polish.

### Common Mistakes

- Putting the company name in the icon (illegible at small sizes)
- Too much detail (looks like noise at 29pt)
- Following trends over timelessness (gradients and gloss age fast)
- Not testing on a real home screen next to other apps

## SF Symbols

SF Symbols is Apple's icon library — 5,000+ symbols designed to integrate with San Francisco (the system font). They are the default choice for iOS iconography.

### When to Use SF Symbols

- **Always use for standard actions:** share, settings, heart, star, trash, plus, search, filter, sort, close, back, forward
- **Always use for system concepts:** notifications, location, camera, microphone, wifi, battery
- **Consider for domain concepts:** Apple adds symbols regularly — check the SF Symbols app before creating custom

### When to Create Custom Icons

- Your app has domain-specific concepts with no SF Symbol equivalent
- You need a specific brand expression that SF Symbols can't provide
- You need multi-color icons with specific brand palette application

### Custom Symbol Creation

If you need custom symbols that feel native alongside SF Symbols:

1. **Download the SF Symbols app** from developer.apple.com
2. **Export a template:** Right-click any symbol > Export Template. This gives you the 9-weight, 3-scale SVG grid.
3. **Design on the grid:** Create your custom symbol matching the template's metrics (cap height, x-height, baseline)
4. **Match the weight:** Your custom symbol at Regular weight should have the same visual density as SF Symbols at Regular
5. **Import as Custom Symbol:** File > Import in SF Symbols app validates your symbol against Apple's guidelines
6. **Export for Xcode:** Add the validated symbol to your Asset Catalog as a Symbol Image Set

### Rendering Modes

SF Symbols support four rendering modes — choose based on your design system:

| Mode | Behavior | Best For |
| ---- | -------- | -------- |
| Monochrome | Single color, inherits foreground | Most UI icons, tab bars |
| Hierarchical | Single color with opacity layers for depth | Icons needing visual hierarchy |
| Palette | Two or more custom colors applied to layers | Brand-colored icons |
| Multicolor | Fixed colors defined by Apple | Symbols with inherent meaning (weather, flags) |

## Illustrations & Empty States

Illustrations add personality and soften empty or error states. They're a "should have" — your app works without them, but feels more polished with them.

### Recommended Approach by App Type

| App Personality | Illustration Style | Tool Recommendation |
| --------------- | ------------------ | ------------------- |
| Professional / enterprise | Minimal line art or geometric | Figma (build from shapes) |
| Friendly / consumer | Soft, rounded, warm illustrations | AI generation (Midjourney with style ref) |
| Playful / gaming | Bold, colorful, character-driven | AI generation + manual refinement |
| Minimal / utility | No illustrations — use SF Symbols + text | SF Symbols app |

### Empty State Strategy

Every screen that can be empty needs an empty state. Plan these:

| Screen | Empty Condition | Recommended Treatment |
| ------ | --------------- | --------------------- |
| Main list/feed | No content yet | Illustration + headline + CTA button |
| Search results | No matches | Illustration + "Try different keywords" |
| Favorites/bookmarks | Nothing saved | Illustration + explanation of how to save |
| Error / offline | Network failure | Illustration + "Retry" button |
| First-time use | New user, no data | Welcome illustration + getting started prompt |

### AI Illustration Workflow

1. **Establish a style reference:** Generate or find one illustration that matches your app's visual direction. Save this as your style anchor.
2. **Use style reference for consistency:** In Midjourney, use `--sref [URL]` with your anchor image. In DALL-E, describe the style explicitly in every prompt.
3. **Generate per-context:** Create illustrations for each empty state with prompts specific to the context: `[scene description], [style adjectives], flat illustration, minimal, white background, --sref [anchor] --ar 1:1`
4. **Remove backgrounds:** Use remove.bg or Figma's background removal to get transparent PNGs
5. **Standardize sizing:** All empty state illustrations should be the same dimensions (e.g., 240x240pt @2x = 480x480px)
6. **Color-match to design tokens:** Adjust illustration colors to use your brand palette, not arbitrary AI-chosen colors

### Format Specifications

| Asset Type | Format | Sizes | Color Space |
| ---------- | ------ | ----- | ----------- |
| Illustrations | PNG with transparency | @2x (default) + @3x | sRGB or Display P3 |
| Custom icons | SVG (source) → PDF or PNG (Xcode) | Template-based for symbols, @2x/@3x for raster | sRGB |
| Photos/backgrounds | JPEG or HEIC | @2x + @3x | sRGB or Display P3 |

## Onboarding Graphics

Onboarding screens introduce the app's value proposition. They need to communicate quickly — users swipe through fast.

### Approaches by Complexity

**Minimal (recommended for v1):**
- SF Symbol or single illustration per screen
- Bold headline + one-sentence description
- 3 screens maximum
- Build in SwiftUI with your design tokens — no external assets needed

**Illustrated:**
- Custom illustration per screen showing the app's core actions
- More memorable but requires 3-5 illustrations
- Use the AI illustration workflow above with consistent style

**Screenshot-based:**
- Actual app screenshots with callout annotations
- Most informative but requires a finished UI (chicken-and-egg problem for v1)
- Better suited for post-launch onboarding updates

### Recommendations

- Start with minimal onboarding for v1 — you can always add illustrations later
- Focus on the value proposition, not feature lists
- Test: if a user skips onboarding entirely, can they still use the app? If not, your app design needs work, not your onboarding.

## Launch Screen

The launch screen appears briefly while the app loads. It should feel like a seamless transition into the app, not a splash ad.

### Apple Guidelines

- **Use a storyboard or SwiftUI:** Dynamic layout, not a static image (required since iOS 14)
- **Match your first screen:** The launch screen should visually transition into whatever the user sees first
- **Keep it minimal:** Background color + centered logo/icon at most. No text, no loading bars, no marketing.
- **Respect dark mode:** Provide a launch screen that adapts to the user's appearance setting

### Recommended Approach

1. Set the launch screen background to your `neutral/background` design token color
2. Optionally center a small version of your app icon or wordmark
3. That's it — don't over-design the launch screen

## Asset Optimization

### Image Compression

Before shipping, optimize all raster assets:

| Tool | Purpose | Cost |
| ---- | ------- | ---- |
| TinyPNG / TinyJPG | Lossy compression (significant size reduction) | Free for <500/mo |
| ImageOptim (macOS) | Lossless compression (preserves quality) | Free |
| Squoosh (Google) | Fine-grained compression control | Free |

**Rule of thumb:** Run all PNGs through ImageOptim (lossless) at minimum. For assets over 100KB, consider TinyPNG (lossy) — the quality loss is imperceptible for illustrations.

### Asset Catalog Best Practices

- **Use Asset Catalogs** for all images (not bundled files) — Xcode optimizes delivery per device
- **Provide @2x and @3x** for raster images. @1x is no longer needed (no non-Retina iOS devices in active use)
- **Use PDF for vector assets** — scales to any size without quality loss, single file instead of @2x/@3x
- **Use Symbol Image Sets** for custom SF Symbols — proper rendering mode support
- **Name consistently:** `category-descriptor-variant` (lowercase, hyphen-separated)
- **Organize by category:** Colors/, Icons/, Illustrations/, Photos/ subdirectories in the Asset Catalog

## Store & Marketing Assets

These are planned during pre-production but created during [Phase 4: Launch](../../phase-4-launch/README.md). During Phase 1, identify what you'll need so you can capture raw materials (screenshots, screen recordings) during production sprints.

### What to Plan Now

- **Screenshot list:** Which screens will you showcase? (Usually 4-6 screens highlighting core features)
- **Preview video concept:** What 15-30 second flow best demonstrates the app's value?
- **Text overlays:** What headlines will accompany each screenshot?
- **Device frames:** Which devices will you show? (iPhone 16 Pro Max is the primary showcase)

### What to Capture During Production

As you build features in Phase 2, save materials for store assets:

- Screenshots of polished UI states (populated with realistic data)
- Screen recordings of key flows (smooth, no hesitation)
- Before/after states for transformation features
- Celebration/achievement moments

This avoids re-staging everything during the launch crunch. See Phase 4 Step 2 for the full store asset creation guide.

### Recommended Free Tools

These browser-based tools handle the most common asset creation and resizing tasks at no cost:

- **[AppShot](https://outofofficeoutdoors.com/products/appshot)** — Free App Store screenshot generator. Add device frames, text overlays, and backgrounds to your raw screenshots, then export at all required Apple sizes. No account or install required.
- **[BatchSnap](https://outofofficeoutdoors.com/products/batchsnap)** — Free batch image resizer and converter. Generate iOS app icon sets (all required sizes from one 1024px source), favicons, and social media images. Runs entirely in-browser with no uploads — your images stay private.
