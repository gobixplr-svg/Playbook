# Design System Spec — [App Name]

> Playbook Reference: [Step 5: Design System](../../steps/05-design-system.md)

## Project

**App Name:** [Name]
**Date:** [YYYY-MM-DD]
**Author:** [Name]

---

## Design Tokens

### Color Tokens

#### Brand Colors

| Token | Name | Light Mode | Dark Mode | Usage |
|-------|------|-----------|-----------|-------|
| `brand/primary` | | | | [Main CTA, active states] |
| `brand/secondary` | | | | [Supporting accents, achievements] |
| `brand/accent` | | | | [Celebrations, highlights] |

#### Neutral Colors

| Token | Name | Light Mode | Dark Mode | Usage |
|-------|------|-----------|-----------|-------|
| `neutral/background` | | | | [Page background] |
| `neutral/surface` | | | | [Cards, sheets] |
| `neutral/border` | | | | [Dividers, input borders] |
| `neutral/textPrimary` | | | | [Headlines, body text] |
| `neutral/textSecondary` | | | | [Captions, metadata] |
| `neutral/textTertiary` | | | | [Placeholders, disabled] |

#### Semantic Colors

| Token | Light Mode | Dark Mode | Usage |
|-------|-----------|-----------|-------|
| `semantic/success` | | | [Completions, confirmations] |
| `semantic/warning` | | | [Cautions, limits] |
| `semantic/error` | | | [Errors, destructive actions] |
| `semantic/info` | | | [Informational messages] |
| `semantic/premium` | | | [Premium features, upgrade prompts] |

#### On-Colors (Text/Icons on Colored Backgrounds)

| Token | Value | Usage |
|-------|-------|-------|
| `on/primary` | | [Text on brand/primary background] |
| `on/secondary` | | [Text on brand/secondary background] |
| `on/accent` | | [Text on brand/accent background] |

### Typography Tokens

| Token | Font | Size | Weight | Line Height | Usage |
|-------|------|------|--------|-------------|-------|
| `type/largeTitle` | | pt | | pt | [Screen headers] |
| `type/title1` | | pt | | pt | [Section headers] |
| `type/title2` | | pt | | pt | [Card titles] |
| `type/title3` | | pt | | pt | [Sub-section headers] |
| `type/headline` | | pt | | pt | [Emphasized body text] |
| `type/body` | | pt | | pt | [Default text] |
| `type/callout` | | pt | | pt | [Secondary descriptions] |
| `type/subhead` | | pt | | pt | [Supporting text] |
| `type/footnote` | | pt | | pt | [Timestamps] |
| `type/caption1` | | pt | | pt | [Small labels] |
| `type/caption2` | | pt | | pt | [Minimum size text] |

#### Specialty Typography

| Token | Font | Size | Weight | Usage |
|-------|------|------|--------|-------|
| `type/heroStat` | | pt | | [Large feature numbers] |
| `type/supportingStat` | | pt | | [Secondary stat displays] |
| `type/inlineStat` | | pt | | [Inline numerical values] |

### Spacing Tokens

| Token | Value | Usage |
|-------|-------|-------|
| `spacing/xs` | pt | [Tight internal spacing] |
| `spacing/sm` | pt | [Component internal padding] |
| `spacing/md` | pt | [Standard spacing] |
| `spacing/lg` | pt | [Section spacing] |
| `spacing/xl` | pt | [Large section gaps] |
| `spacing/xxl` | pt | [Screen-level spacing] |

**Base unit:** [4pt / 8pt]

### Corner Radius Tokens

| Token | Value | Usage |
|-------|-------|-------|
| `radius/small` | pt | [Buttons, chips] |
| `radius/medium` | pt | [Cards, inputs] |
| `radius/large` | pt | [Sheets, large cards] |
| `radius/full` | pt | [Pills, circles] |

### Shadow Tokens

| Token | X | Y | Blur | Opacity | Usage |
|-------|---|---|------|---------|-------|
| `shadow/sm` | | | | | [Cards on surface] |
| `shadow/md` | | | | | [Floating elements] |
| `shadow/lg` | | | | | [Sheets, modals] |

---

## SwiftUI Token Mapping

### Colors

```swift
// Asset Catalog approach (recommended — supports light/dark automatically)
extension Color {
    // Brand
    static let brandPrimary = Color("BrandPrimary")
    static let brandSecondary = Color("BrandSecondary")
    static let brandAccent = Color("BrandAccent")

    // Neutrals
    static let appBackground = Color("AppBackground")
    static let appSurface = Color("AppSurface")
    // ...
}
```

### Typography

```swift
// Define approach — ViewModifier, Font extension, or enum
```

### Spacing

```swift
enum Spacing {
    static let xs: CGFloat = _
    static let sm: CGFloat = _
    // ...
}
```

### Radii

```swift
enum CornerRadius {
    static let small: CGFloat = _
    static let medium: CGFloat = _
    // ...
}
```

---

## Component Library

### Component Inventory

| # | Component | Variants | Used In |
|---|-----------|----------|---------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

---

### [Component Name]

**Anatomy:**
- [Sub-element]: [Description]

**Variants:**
| Variant | Visual Difference | When Used |
|---------|------------------|-----------|
| | | |

**Tokens Used:**
| Property | Token |
|----------|-------|
| Background | |
| Text color | |
| Font | |
| Corner radius | |
| Padding | |

**States:**
| State | Visual Change |
|-------|--------------|
| Default | |
| Pressed | |
| Disabled | |
| Loading | |

**Behavior:**
- Tap: [animation, haptic]
- Transition: [timing curve]

**Sizing:**
- Height: [fixed/intrinsic] — [value]
- Width: [fixed/full-width/intrinsic]
- Touch target: [value] (minimum 44pt)

**Accessibility:**
- VoiceOver: [trait, label pattern]
- Dynamic Type: [scaling behavior]

---

_Repeat for each component..._

---

## Layout Patterns

### Screen Templates

| Template | Structure | Used By |
|----------|-----------|---------|
| Standard | | [Screens] |
| Map | | [Screens] |
| Sheet | | [Screens] |

### Grid System

- **Base unit:** [value]
- **Content margin:** [value] (left/right)
- **Card gap:** [value]
- **Section gap:** [value]

### Responsive Behavior

| Element | iPhone SE | iPhone 16 | iPhone 16 Pro Max |
|---------|-----------|-----------|-------------------|
| | | | |

---

## Animation & Motion

### Timing Curves

| Name | SwiftUI Definition | Usage |
|------|-------------------|-------|
| standard | | [General UI] |
| quick | | [Micro-interactions] |
| celebratory | | [Achievements, milestones] |

### Transitions

| Navigation | Type | Duration |
|------------|------|----------|
| Push | | ms |
| Sheet | | ms |
| Tab switch | | ms |
| Content load | | ms |

### Haptics

| Trigger | Pattern | Intensity |
|---------|---------|-----------|
| Button tap | | |
| Achievement | | |
| Error | | |

---

## Coverage Verification

- [ ] Every color in wireframes maps to a color token
- [ ] Every text style maps to a typography token
- [ ] Every spacing value is from the spacing scale
- [ ] Every interactive element is a specified component
- [ ] Every component has all states defined
- [ ] Every animation has a defined timing curve
- [ ] Dark mode values exist for all color tokens
- [ ] All components meet 44pt touch target minimum
- [ ] Dynamic Type accounted for in all text components
- [ ] Component specs are unambiguous enough for implementation without questions

---

## Status

- [ ] Color tokens defined (light + dark)
- [ ] Typography tokens defined
- [ ] Spacing tokens defined
- [ ] Radius tokens defined
- [ ] Shadow tokens defined
- [ ] SwiftUI mapping documented
- [ ] Component library identified
- [ ] Components fully specified
- [ ] Layout patterns documented
- [ ] Animation & motion defined
- [ ] Coverage verified against wireframes
