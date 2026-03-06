# UI/UX Spec — [App Name]

> Playbook Reference: [Step 4: UI/UX Design](../../steps/04-uiux-design.md)

## Project

**App Name:** [Name]
**Date:** [YYYY-MM-DD]
**Author:** [Name]

---

## Visual Direction

### Mood Board

_Link or embed 10-20 reference images that capture the app's personality._

| # | Reference | What to Learn From It |
|---|-----------|----------------------|
| 1 | [App/image name] | [What you admire — color, spacing, animation, etc.] |
| 2 | | |
| 3 | | |

### App Personality

**The app should feel:** [3-5 adjectives]

**The app should NOT feel:** [3-5 adjectives]

### Visual References

| # | App / Brand | What to Learn |
|---|-------------|---------------|
| 1 | [Name] | [Specific design quality] |
| 2 | | |
| 3 | | |

---

## Color Palette

### Brand Colors

| Role | Name | Hex | RGB | Usage |
|------|------|-----|-----|-------|
| Primary | | | | [Main CTA, brand identity, key UI elements] |
| Secondary | | | | [Supporting elements, variety, secondary actions] |
| Accent | | | | [Highlights, celebrations, alerts] |

### Neutral Colors

| Role | Name | Hex | Usage |
|------|------|-----|-------|
| Background | | | [Page background] |
| Surface | | | [Cards, sheets, elevated surfaces] |
| Border | | | [Dividers, input borders] |
| Text Primary | | | [Headlines, body text] |
| Text Secondary | | | [Captions, labels, metadata] |
| Text Tertiary | | | [Placeholders, disabled text] |

### Semantic Colors

| Role | Hex | Usage |
|------|-----|-------|
| Success | | [Completions, confirmations] |
| Warning | | [Cautions, approaching limits] |
| Error | | [Errors, destructive actions] |
| Info | | [Informational messages] |
| Premium | | [Premium features, upgrade prompts] |

### Accessibility

| Combination | Contrast Ratio | WCAG Level |
|-------------|---------------|------------|
| Text Primary on Background | :1 | [AA/AAA] |
| Text Secondary on Background | :1 | [AA/AAA] |
| Primary on Background | :1 | [AA/AAA] |
| Text on Primary | :1 | [AA/AAA] |

---

## Typography

### Font Choices

| Role | Font | Weight | Fallback |
|------|------|--------|----------|
| Headlines | | | |
| Body | | | |
| Numbers / Stats | | | |

### Type Scale

| Style | Font | Size | Weight | Line Height | Usage |
|-------|------|------|--------|-------------|-------|
| Large Title | | pt | | | [Screen headers] |
| Title 1 | | pt | | | [Section headers] |
| Title 2 | | pt | | | [Card titles] |
| Headline | | pt | | | [Emphasized body text] |
| Body | | pt | | | [Default text] |
| Subhead | | pt | | | [Secondary text, labels] |
| Caption | | pt | | | [Metadata, timestamps] |

---

## Screen Inventory

| # | Screen | Tab / Flow | Type | Priority |
|---|--------|-----------|------|----------|
| 1 | | | Push / Sheet / Cover | Core / Secondary |
| 2 | | | | |
| 3 | | | | |

---

## User Flows

### Flow 1: [Flow Name]

**Entry:** [Where the user starts]
**Goal:** [What they accomplish]

```
[Screen A] → [Screen B] → [Screen C]
     │              │
     └─ [Alt path]  └─ [Sheet/Modal]
```

**Steps:**
1. User [action] → sees [screen]
2. User [action] → sees [screen]
3. ...

---

### Flow 2: [Flow Name]

_Repeat for each key flow..._

---

## Wireframes

### [Screen Name]

```
┌─────────────────────────────────┐
│                                 │
│   [ASCII wireframe here]        │
│                                 │
└─────────────────────────────────┘
```

**Elements:**
- [Element]: [Description and behavior]
- [Element]: [Description and behavior]

**States:**
- Empty: [Description]
- Loading: [Description]
- Error: [Description]

---

_Repeat for each screen..._

---

## Screen Designs

### Design Notes per Screen

| Screen | Key Design Decisions | Animation Notes |
|--------|---------------------|-----------------|
| | | |

### Interactive States

| Element | Default | Pressed | Disabled | Selected |
|---------|---------|---------|----------|----------|
| Primary Button | | | | |
| Card | | | | |
| Tab | | | | |

---

## Interaction Patterns

### Gestures

| Gesture | Context | Action |
|---------|---------|--------|
| Tap | | |
| Swipe | | |
| Long press | | |
| Pull down | | |
| Pinch | | |

### Transitions

| Navigation | Transition Type | Duration |
|------------|----------------|----------|
| Push to detail | | ms |
| Present sheet | | ms |
| Present fullscreen | | ms |
| Tab switch | | ms |

### Animations

| Trigger | Animation | Duration | Easing |
|---------|-----------|----------|--------|
| | | ms | |

---

## Prototype Links

| Flow | Tool | Link |
|------|------|------|
| First launch | Figma | [Link] |
| Core loop | Figma | [Link] |
| | | |

---

## AI Tooling Log

_Track which AI tools were used and their outputs for future reference._

| Step | Tool | Input (prompt/description) | Output | Used? |
|------|------|---------------------------|--------|-------|
| Mood board | | | | |
| Color palette | | | | |
| Screen design | | | | |

---

## Feedback Notes

| Reviewer | Date | Key Feedback | Action Taken |
|----------|------|-------------|--------------|
| | | | |

---

## Status

- [ ] Visual direction defined (mood board, personality, references)
- [ ] Color palette established with accessibility verification
- [ ] Typography chosen with type scale
- [ ] User flows mapped
- [ ] Wireframes created for all screens
- [ ] High-fidelity designs for core screens
- [ ] Interactive prototype built
- [ ] Accessibility review complete
- [ ] Feedback gathered
