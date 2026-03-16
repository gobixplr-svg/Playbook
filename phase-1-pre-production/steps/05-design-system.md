# Step 5: Design System

## Purpose

Extract the visual decisions from Step 4 (UI/UX Design) into a formalized, reusable system of design tokens, component specifications, and SwiftUI code-ready definitions. By the end of this step, every color, font size, spacing value, and component variant is defined exactly once — so that production code implements the design with zero ambiguity.

## Why This Matters

Step 4 is creative exploration — it answers "what should the app look and feel like?" Step 5 is systematic codification — it answers "how do we build it consistently?"

Without a design system:
- Colors are hardcoded as hex strings scattered across 50 files
- The same button is implemented three different ways by the third sprint
- Spacing is eyeballed, producing subtle inconsistencies that make the app feel unpolished
- Dark mode is a nightmare because nothing is abstracted

With a design system:
- **Single source of truth** — change a color once, it updates everywhere
- **Consistency** — every button, card, and label behaves identically
- **Speed** — new screens compose from existing components like building blocks
- **Dark mode for free** — semantic tokens resolve to different values per appearance

## Prerequisites

- Step 3 (Architecture) completed — data models and service protocols inform which components need data-bound states
- Step 4 (UI/UX Design) completed — wireframes and screen designs provide the visual vocabulary to systematize

## Process

### 5.1 Define Design Tokens

Design tokens are the atomic values that everything else is built from. They are named constants — not raw values.

**Color tokens:**

Map every color from the Step 4 palette to a semantic token name:

```
brand/primary     → Trail Teal #0A7B6F
brand/secondary   → Summit Amber #D4952B
brand/accent      → Sunset Coral #E06B4D
neutral/background → Cloud White #F8F6F3
neutral/surface    → Card White #FFFFFF
...
```

For each token, define light AND dark mode values. These are paired — a single token name resolves differently per appearance.

**Typography tokens:**

Define the type scale as named styles, not raw font/size/weight combinations:

```
largeTitle  → SF Rounded, 34pt, Bold, 41pt line height
title1      → SF Rounded, 28pt, Bold, 34pt line height
body        → SF Pro, 17pt, Regular, 22pt line height
...
```

**Spacing tokens:**

Define a spacing scale based on a base unit (typically 4pt or 8pt):

```
spacing/xs   → 4pt
spacing/sm   → 8pt
spacing/md   → 16pt
spacing/lg   → 24pt
spacing/xl   → 32pt
spacing/xxl  → 48pt
```

All margins, paddings, and gaps should use these tokens — never arbitrary values.

**Radius tokens:**

Define corner radius values by semantic use:

```
radius/small    → 8pt   (buttons, chips, small elements)
radius/medium   → 12pt  (cards, inputs)
radius/large    → 16pt  (sheets, large cards, badges)
radius/full     → 9999pt (pills, circular avatars)
```

**Shadow tokens:**

Define elevation levels:

```
shadow/sm    → (0, 1, 3, 0.08)  (cards on surface)
shadow/md    → (0, 4, 12, 0.12) (floating elements)
shadow/lg    → (0, 8, 24, 0.16) (sheets, modals)
```

### 5.2 Map Tokens to SwiftUI

Translate each token into SwiftUI code. This creates the bridge from design to production.

**Color extension:**

```swift
extension Color {
    static let brandPrimary = Color("TrailTeal")
    // resolved from Asset Catalog with light/dark variants
}
```

**Typography modifier or ViewModifier:**

```swift
struct AppTypography {
    static let largeTitle = Font.system(.largeTitle, design: .rounded).bold()
    // or custom font with fallback
}
```

**Spacing constants:**

```swift
enum Spacing {
    static let xs: CGFloat = 4
    static let sm: CGFloat = 8
    static let md: CGFloat = 16
    // ...
}
```

The spec should define the SwiftUI approach — these are not runnable code yet (that's Phase 2), but they define the exact API that production code will use.

### 5.3 Define Component Library

Components are the reusable building blocks that compose every screen. Each component combines tokens into a specific, interactive element.

**Identify components from wireframes:**

Review every wireframe from Step 4 and extract the repeating patterns:

- Buttons (primary, secondary, ghost, icon)
- Cards (route card, stats card, badge card)
- Navigation bars (standard, transparent-over-map)
- Tab bars
- Progress bars and indicators
- Stat displays (pills, grids, hero stats)
- Badges (earned, unearned, categories)
- Lists (route list, milestone list, friend list)
- Sheets (milestone, share card, paywall)
- Empty states
- Loading states (skeleton shimmers)
- Error states

**For each component, document:**

1. **Anatomy** — What sub-elements compose this component? (icon, label, background, border)
2. **Variants** — What versions exist? (primary/secondary/ghost button, active/completed route card)
3. **States** — How does it look in each state? (default, pressed, disabled, selected, loading, error)
4. **Tokens used** — Which exact design tokens does it reference? (colors, fonts, spacing, radii)
5. **Behavior** — How does it respond to interaction? (tap animation, haptic, transition)
6. **Sizing** — Fixed height? Full-width? Intrinsic? Min/max constraints?
7. **Accessibility** — Touch target size (44pt minimum), dynamic type support, VoiceOver label

### 5.4 Specify Component Details

For each component in the library, write a complete specification that a developer (or AI) can implement without ambiguity.

**Example specification format:**

```
## PrimaryButton

### Anatomy
[Icon (optional)] + [Label]

### Variants
- Standard: Full-width, tall (50pt), Trail Teal background
- Compact: Intrinsic width, shorter (44pt)
- Destructive: Error Red background

### Tokens
- Background: brand/primary
- Text: neutral/onPrimary (white)
- Corner radius: radius/small (8pt)
- Font: headline (SF Pro, 17pt, Semibold)
- Padding: horizontal spacing/lg (24pt), vertical spacing/md (16pt)

### States
- Default: Full opacity
- Pressed: 0.85 opacity, scale 0.98
- Disabled: 0.4 opacity, no interaction
- Loading: Label replaced with ProgressView

### Behavior
- Tap: light haptic (.light impact)
- Animation: spring response 0.3, damping 0.7

### Accessibility
- Touch target: 50pt height minimum (meets 44pt requirement)
- Dynamic type: label scales, button height adjusts
- VoiceOver: reads label as button trait
```

### 5.5 Define Layout Patterns

Document the recurring layout patterns that compose screens from components:

**Screen layout template:**
- Standard screen: Large Title → content → tab bar
- Map screen: Full-screen map → floating stats card → transparent nav bar
- Sheet: Drag handle → hero content → actions
- Modal: Close button → content → CTA

**Grid system:**
- Base unit: 4pt
- Content margin: 16pt (left/right)
- Card gap: 12pt
- Section gap: 24pt
- Badge grid: 4 columns, 8pt gap

**Responsive behavior:**
- How do layouts adapt to larger phones vs. SE-sized phones?
- What scrolls vs. what is fixed?
- How do components reflow in landscape (if supported)?

### 5.6 Document Animation & Motion

Define the animation language used throughout the app:

**Timing curves:**
```
standard    → spring(response: 0.35, dampingFraction: 0.85)
quick       → spring(response: 0.25, dampingFraction: 0.9)
celebratory → spring(response: 0.5, dampingFraction: 0.7)
```

**Transition standards:**
- Push navigation: system default
- Sheet presentation: system default
- Tab switch: crossfade, 0.2s
- Content loading: fade in, 0.3s
- Number counter: count-up with easeOut, 0.6s

**Haptic patterns:**
- Button tap: `.impact(.light)`
- Milestone reached: `.notification(.success)`
- Badge earned: `.impact(.medium)` + `.notification(.success)`
- Error: `.notification(.error)`

**Animation categories to define:**
1. **Micro-interactions** — button presses, toggle switches, text field focus. These should feel instant (0.1–0.2s) and use `easeOut` curves.
2. **Transitions** — screen-to-screen navigation, modal presentation, tab switching. Use `easeInOut` at 0.25–0.35s. Match iOS system conventions unless your brand intentionally departs.
3. **State changes** — loading → loaded, empty → populated, collapsed → expanded. Use `spring` animations for natural feel. Define skeleton/shimmer states for loading.
4. **Celebrations** — achievement earned, milestone reached, route completed. These are your brand moments — invest in making them distinctive. Longer duration (0.5–1.5s), multi-stage sequences, consider haptics.
5. **Data updates** — progress bar advancement, counter increments, list reordering. Use `linear` or `easeInOut` to feel smooth, not jarring.

**Motion principles to establish:**
- **Consistent direction** — elements entering from the right should exit to the left. Pick a spatial model and stick to it.
- **Meaningful motion** — animation should communicate something (completion, error, hierarchy). If removing an animation doesn't lose information, the animation isn't needed.
- **Respect reduced motion** — define fallback behaviors for every animation when `UIAccessibility.isReduceMotionEnabled` is true (typically instant transitions, no bouncing, no parallax).

**Document each animation as:**
- Trigger (what starts it)
- Duration and curve
- Properties animated (opacity, scale, position, color)
- Reduced motion alternative

### 5.8 Asset Planning & Pipeline

Before entering production, inventory the visual assets the app will need and decide how each will be created. This step is advisory — you can defer asset creation to later sprints — but planning now prevents last-minute scrambles before launch.

**Why plan assets early:**

- App icons and launch screens are needed for the first testable build
- Empty state illustrations and onboarding graphics affect layout decisions
- Knowing whether you'll use SF Symbols vs. custom icons impacts component specs
- Store screenshots require real content and polished UI — planning backward from launch avoids crunch

See the [Asset Pipeline Reference](../references/asset-pipeline.md) for detailed tool recommendations, generation workflows, and format specifications.

**Asset inventory — identify what you need:**

Review your wireframes and user flows from Step 4 and catalog every visual asset:

| Category | Assets | Priority | Creation Approach |
| -------- | ------ | -------- | ----------------- |
| **App Identity** | App icon (1024x1024), launch screen, logo variants | Must Have — needed for first build | AI generation → refine, or commission |
| **Iconography** | Tab bar icons, action icons, status indicators | Must Have — needed for UI | SF Symbols first, custom only when SF Symbols fall short |
| **Empty States** | No data, no results, first-time-use illustrations | Should Have — improves polish | AI illustration generation or minimal vector |
| **Onboarding** | Welcome screens, feature highlights, tutorial graphics | Should Have — improves retention | AI generation matched to visual direction |
| **In-App Graphics** | Achievement badges, progress visuals, decorative elements | Could Have — adds delight | Design tool or AI generation |
| **Store Assets** | Screenshots, preview video, promotional art | Must Have — but deferred to Phase 4 | Planned here, created in Phase 4 |
| **Marketing** | Social cards, press kit, website hero | Could Have — deferred to Phase 4+ | Planned here, created in Phase 4 |

**Decide your icon strategy:**

This is the most impactful asset decision because icons appear on every screen.

1. **SF Symbols first (recommended for iOS):** Apple provides 5,000+ symbols that automatically support Dynamic Type, weight variants, and accessibility. Use SF Symbols for any standard action (share, settings, heart, star, map pin, etc.).
2. **Custom icons only when needed:** If your app has domain-specific concepts that SF Symbols can't express (e.g., a trail blaze marker, a specific badge shape), plan custom icons. Define the style: line weight, corner radius, filled vs. outlined, size grid.
3. **Consistency rule:** Don't mix SF Symbols and custom icons with different visual weights. If you use custom icons, match them to SF Symbol weight conventions.

**App icon approach:**

The app icon is the single most visible brand asset. Decide the creation approach early:

- **AI generation → refinement:** Use Midjourney or similar to explore concepts, then refine the best option in a vector tool. Fast, good for exploration, but may need manual polish.
- **Design tool:** Build directly in Figma or Sketch using your design tokens. Full control, but slower.
- **Commission:** Hire a designer for the icon only ($50-200 for a quality app icon). Worth it if the icon is critical to brand identity.
- **Apple guidelines:** Single, recognizable glyph. No text (illegible at small sizes). No photos. Avoid transparency. Test at 29pt, 40pt, 60pt, and 1024pt — it must read at all sizes.

**Asset catalog structure:**

Plan the Xcode Asset Catalog organization:

```text
Assets.xcassets/
├── AppIcon.appiconset/          # App icon (single 1024x1024, Xcode generates sizes)
├── Colors/                       # Named colors (from design tokens)
│   ├── BrandPrimary.colorset/
│   ├── BrandSecondary.colorset/
│   └── ...
├── Icons/                        # Custom icons (if any)
│   ├── icon-trail-blaze.imageset/
│   └── ...
├── Illustrations/                # Empty states, onboarding, decorative
│   ├── empty-state-no-routes.imageset/
│   └── ...
└── LaunchScreen/                 # Launch screen assets
    └── ...
```

**Naming convention:** `category-descriptor-variant` (e.g., `icon-badge-earned`, `illustration-empty-search`, `onboard-welcome-01`). Lowercase, hyphen-separated.

**Add your asset inventory to the [Design System Spec](../templates/design-system-spec.md)** in the Asset Inventory section, noting priority and creation approach for each item.

### 5.9 Verify Design System Coverage

Check the design system against the wireframes and requirements:

**Coverage checklist:**
- [ ] Every color in the wireframes maps to a token
- [ ] Every text style maps to a typography token
- [ ] Every spacing value is from the spacing scale
- [ ] Every interactive element is a specified component
- [ ] Every component has all states defined
- [ ] Every animation has a defined timing curve
- [ ] Dark mode values exist for all color tokens
- [ ] All components meet 44pt touch target minimum
- [ ] Dynamic Type is accounted for in all text components

## Deliverable

Complete the [Design System Spec Template](../templates/design-system-spec.md) and save it to your project's `docs/pre-production/design-system-spec.md`.

## Definition of Done

- [ ] Color tokens defined with light and dark mode values
- [ ] Typography tokens defined with full type scale
- [ ] Spacing scale defined with named tokens
- [ ] Corner radius tokens defined
- [ ] Shadow/elevation tokens defined
- [ ] All tokens mapped to SwiftUI code patterns
- [ ] Component library identified from wireframe analysis
- [ ] Each component fully specified (anatomy, variants, states, tokens, behavior, accessibility)
- [ ] Layout patterns documented (grids, margins, screen templates)
- [ ] Animation and motion language defined (timing, haptics, transitions)
- [ ] Design system verified against all wireframes for complete coverage
- [ ] Dark mode adaptation documented for all visual elements

## Tips

- **Tokens before components.** Define all your atomic values (colors, fonts, spacing) before specifying components. Components reference tokens — if the tokens change, components update automatically.
- **Name tokens semantically, not visually.** `brand/primary` not `teal`. `neutral/background` not `offWhite`. Semantic names survive redesigns; visual names don't.
- **Don't over-specify v1.** You don't need 40 components. Identify the 10-15 components that compose 90% of your screens. Add more during production as needed.
- **The spec is the contract.** When a developer (or AI) implements a component, they should be able to match the spec without asking questions. If the spec is ambiguous, fix the spec.
- **Use the wireframes as your test.** After defining the system, mentally "rebuild" each wireframe using only your tokens and components. If you can't, something is missing.
- **SwiftUI code patterns, not runnable code.** The spec defines the API surface (how you'll use `Color.brandPrimary`, `Spacing.md`, `PrimaryButton()`). Actual implementation happens in Phase 2.

### Game Development Notes

If building a game rather than a standard app:

- **Glow tokens replace shadow tokens.** Dark-background games with neon aesthetics use glow intensity levels (none/subtle/standard/intense) instead of shadow elevation. Define glow colors, blur radii, and intensity values as tokens.
- **Gameplay object colors are a first-class token category.** Blob colors, tile colors, team colors — whatever your core gameplay objects use — should be defined as tokens alongside UI colors. Include unlock order, accessibility contrast ratios, and colorblind-distinguishable values.
- **Dark-only is valid.** Not every app needs light/dark mode. Games with a specific visual aesthetic (neon, space, underwater) can commit to a single appearance and eliminate the token duplication.
- **Use engine-native code patterns.** For Unity: `ScriptableObject` color palettes and `static class` design tokens instead of SwiftUI `Color` extensions. For Godot/Unreal: equivalent data-driven approaches.
