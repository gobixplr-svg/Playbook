# Step 4: UI/UX Design

## Purpose

Establish the visual direction, user flows, wireframes, and screen designs for the app. By the end of this step, every key screen has a wireframe, the visual language is defined, and interactive prototypes demonstrate the core user flows — so that production code starts from a clear design target, not guesswork.

## Why This Matters

"I'll figure out the UI as I code" is how apps end up with inconsistent layouts, clashing colors, and confusing navigation. Investing in design before production has three benefits:

1. **Speed** — Developers build faster when they know exactly what the screen looks like. No pausing to make design decisions mid-sprint.
2. **Consistency** — A defined visual direction ensures every screen feels like part of the same app. Without it, screens designed months apart drift in style.
3. **Quality** — Design is the biggest differentiator in crowded app categories. Competitors with dated UIs leave an opening for a polished product.

This step is the creative exploration phase — mood, color, typography, layout. Step 5 (Design System) extracts the reusable system from these designs.

## AI-Powered Design Pipeline

This step uses AI tools to accelerate design without requiring professional design skills:

| Phase | Tool | Cost | Purpose |
|-------|------|------|---------|
| Visual direction | Midjourney | $10 one-time | Mood boards, color exploration, atmosphere |
| Color system | Khroma + Coolors | Free | AI color palette generation, accessibility checking |
| Screen generation | Google Stitch | Free | Text-to-UI design, multi-screen flows, Figma export |
| Design refinement | Figma (Starter) | Free | Polish designs, build components, prototype flows |
| Design-to-code | Codia AI + Figma MCP | Free | Figma → SwiftUI code bridge, Claude Code integration |

**Pipeline:** Midjourney (mood) → Khroma/Coolors (colors) → Stitch (screens) → Figma (refine) → Codia/MCP (SwiftUI)

You don't need every tool. The minimum viable pipeline is: hand-drawn wireframes → Figma → code. AI tools accelerate the process but aren't required.

## Process

### 4.1 Define Visual Direction

Before designing any screens, establish the emotional direction of the app through structured visual research.

**App personality (do this first):**
- Write 3-5 adjectives that describe how the app should feel (e.g., "adventurous, warm, premium, encouraging, clean")
- Write 3-5 adjectives the app should NOT feel (e.g., "not clinical, not competitive, not cluttered, not childish")
- Write a one-sentence "design north star" that captures the ideal experience
- These words guide every design decision going forward

**Visual reference research (the core of this step):**

Don't just list apps you like — do structured research. For each reference:

1. **Select references by design problem, not by preference.** Identify the 4-6 specific design challenges your app faces (e.g., "browsing destinations," "showing progress on a map," "celebrating achievements"), then find the app that solves each one best.

2. **Study each reference in depth.** For every reference app, document:
   - **Layout patterns** — How do they structure their main screens? Card sizes? Grid vs list? Spacing?
   - **Color usage** — What's their primary palette? How do they use color to create hierarchy?
   - **Typography** — What feels distinctive? How do they handle stats/numbers?
   - **Animation/celebration** — How do they celebrate achievements or progress?
   - **What makes it feel premium** — The specific details that elevate it above average

3. **Extract specific patterns, not vague impressions.** "AllTrails uses full-width trail cards with 16:9 hero photos where the photo takes 60% of the card area" is useful. "AllTrails has a nice design" is not.

4. **Map each pattern to YOUR app.** Create a table: Reference Pattern → Your Decision → Where Applied. This creates traceability from reference → design decision → wireframe. Every wireframe element should trace back to a deliberate decision.

5. **Document anti-patterns too.** What did you find that you want to AVOID? (e.g., "Headspace hides premium labels, causing surprise paywalls — we'll show lock icons upfront")

**Mood board (AI-assisted):**
- Use Midjourney to generate 4-8 images that capture the app's atmosphere
- Use design-specific language: "interface," "layout," "component," "HIG" — not art words like "beautiful" or "fantasy"
- Set `--ar 9:16` for mobile proportions, `--stylize 500-700` for UI-appropriate results
- Use `--sref [URL]` to match a style you like across multiple prompts
- Create a Midjourney Moodboard (5-10 images) for consistent style with `--p [moodboardID]`
- Save the best images to `docs/pre-production/mood-board/` for reference throughout the project
- Focus on atmosphere, not specific UI patterns — color temperature, lighting, density, typography weight

**Browse real app designs:**
- [Mobbin](https://mobbin.com) — hundreds of real app designs organized by screen type (search for your screen types)
- [Dribbble](https://dribbble.com) — design portfolios (search for your app category + "iOS")
- Download and screenshot reference apps yourself — the best research is hands-on

### 4.2 Establish Color Palette

Define the app's color system. Colors are the most impactful design decision — they set the emotional tone instantly.

**Process:**
1. Start with the mood board — what colors dominate?
2. Choose a primary color (brand identity, CTAs, key UI elements)
3. Choose a secondary color (supporting elements, accents, variety)
4. Choose an accent color (alerts, celebrations, highlights)
5. Define neutral colors (backgrounds, text, borders, cards)
6. Define semantic colors (success, warning, error, info)

**AI-assisted approach:**
- **Khroma** (khroma.co): Train it on colors you like, then browse AI-generated palettes
- **Coolors** (coolors.co): Generate palettes, check contrast ratios, export as design tokens

**Accessibility requirements:**
- Text on backgrounds must meet WCAG AA contrast ratio (4.5:1 for body text, 3:1 for large text)
- Use Coolors' contrast checker or Figma's accessibility plugins
- Test colors against light mode AND dark mode backgrounds
- Ensure color is not the only way to convey information (icons + labels)

**Output:** Document hex values, RGB values, and usage guidelines for each color.

### 4.3 Choose Typography

Select fonts for the app. Typography defines readability and personality.

**Guidelines:**
- **System fonts first:** SF Pro (iOS) is optimized for Apple devices — excellent readability, dynamic type support, no licensing cost. Start here unless you have a strong reason not to.
- **One accent font max:** If you want personality beyond SF Pro, add ONE display/headline font. Two custom fonts is the maximum; three is chaos.
- **Define a type scale:** 5-7 sizes that cover all needs — caption, body, subhead, title, large title. Use iOS conventions (Body: 17pt, Title: 28pt, Large Title: 34pt).
- **Weight hierarchy:** Use weight (regular, medium, semibold, bold) rather than size to create hierarchy within a text size.

**Questions to answer:**
- What font for headings? (Personality-driven — rounded, geometric, serif, etc.)
- What font for body text? (Readability-driven — almost always SF Pro)
- What font for numbers/stats? (Tabular/monospaced for alignment — SF Mono or tabular figures)
- What's the minimum size for readable text? (Never below 11pt on iOS)

### 4.4 Map User Flows

Document every key user journey as a sequence of screens with transitions between them.

**For each flow, document:**
1. **Entry point** — Where does the user start? (Tab, notification, deep link)
2. **Screen sequence** — Each screen they pass through
3. **Transitions** — How they move between screens (push, sheet, fullScreenCover)
4. **Decision points** — Where the flow branches (authenticated vs. not, free vs. premium)
5. **Exit point** — Where does the flow resolve?

**Key flows to document:**
- First launch (sign in → onboarding → first route)
- Daily check-in (open app → view progress → close)
- Browse and start a route (library → detail → start journey)
- Active walk / live mode (start session → walk → milestone → end)
- Milestone celebration (milestone reached → photo → badge → share)
- Share achievement (badge/milestone → share card → share sheet)
- Social (view friends → react → add friend)
- Upgrade to premium (encounter paywall → subscribe → unlock)
- Settings and account management

**Use the navigation architecture from Step 3** (Architecture Document) as the foundation.

### 4.5 Create Wireframes

Create low-fidelity layouts for every screen in the app. Wireframes define what goes where — layout, hierarchy, and content — without visual polish.

**Wireframe rules:**
- **No colors** — use grayscale (white, light gray, dark gray, black)
- **No real images** — use placeholder boxes with X marks
- **No pixel-perfect spacing** — approximate proportions are fine
- **Label everything** — every element should be identifiable
- **Include all states** — empty, loading, populated, error

**For each screen, define:**
- Header / navigation bar content
- Content hierarchy (what's most prominent?)
- Interactive elements (buttons, taps, swipes)
- Data displayed (labels, values, images)
- Scroll behavior (fixed header? Collapsing? Infinite scroll?)

**Tools:**
- Pen and paper (fastest for initial exploration)
- Figma with wireframe components (easiest to iterate)
- ASCII art in documentation (version-controllable, always accessible)

**Create wireframes for every screen in the screen inventory**, with priority on the core flow screens.

### 4.6 Design Key Screens

Transform wireframes into high-fidelity designs with real colors, typography, and imagery.

**AI-assisted approach (Google Stitch):**
1. Go to [stitch.withgoogle.com](https://stitch.withgoogle.com) and sign in with Google — completely free, web-based
2. Choose **Standard Mode** (350 generations/month) and **Mobile** platform
3. Write a detailed prompt for each screen. Include: screen purpose, layout structure, specific UI components, color hex values, typography style, and the emotional tone you want
4. Stitch generates multiple design options — select the best one
5. Submit follow-up prompts to refine ("make the stats larger," "add more whitespace")
6. Click **"Copy to Figma"** to export (Standard Mode only)
7. Repeat for each screen

**Writing effective Stitch prompts:**
- Be specific about layout: "Three stat pills in a row below the title" not "show some stats"
- Include exact colors: "teal #0A7B6F" not "a teal color"
- Reference known patterns: "Airbnb-style card layout" or "iOS large title navigation"
- Describe the mood: "celebratory but classy" or "inviting, not aggressive"
- The wireframes from Step 4.5 are your prompt blueprint — describe what's in the wireframe plus the visual treatment

**Design priority order:**
1. The screen most users see first (e.g., Route Library)
2. The "selling" screen (e.g., Route Detail — where users decide to engage)
3. The core experience screen (e.g., Journey Map)
4. The emotional peak (e.g., Milestone Celebration — design this to be screenshot-worthy)
5. The growth engine (e.g., Share Card — this IS your marketing)
6. The achievement showcase (e.g., Trophy Case)
7. Sign In / Onboarding (first impression)
8. The conversion screen (e.g., Paywall)
9. Remaining screens

**For each design, document:**
- Final layout with real content
- Color usage (which palette colors where)
- Typography (which font/size/weight for each element)
- Spacing (padding, margins, gaps)
- Interactive states (default, pressed, disabled, selected)
- Animation notes (what moves, how, and when)

### 4.7 Prototype User Flows

Connect the designed screens into interactive prototypes that can be tapped through.

**Figma setup (if not already done):**
1. Sign up at [figma.com](https://figma.com) — Starter plan is free (3 files, unlimited pages)
2. Create a new file for your app designs
3. Paste Stitch exports (Cmd+V) — each screen as a separate frame
4. Standardize colors and typography to match your palette (create Color Styles and Text Styles)
5. Align all spacing to a 4pt/8pt grid

**Building the prototype:**
1. Select a frame → Switch to Prototype tab (right panel)
2. Drag connections from interactive elements to target frames
3. Define transitions:
   - Push navigation: "Push Right" / 350ms
   - Sheet presentations: "Slide Up" / 350ms
   - Tab switches: "Dissolve" / 200ms
   - Sheet dismiss: "Slide Down" / 350ms
4. Test on a physical device using **Figma Mirror** (free iOS app)
5. Share prototype link for feedback: Click "Share" → "Anyone with the link can view"

**Flows to prototype (priority order):**
1. First launch through first route selection
2. Daily check-in (open → view progress → close)
3. Milestone celebration and sharing
4. Paywall flow

**What you're testing:**
- Does the navigation feel intuitive?
- Are the screens in the right order?
- Is any step confusing or unnecessary?
- Does the celebration moment feel rewarding?
- Is the paywall persuasive but not aggressive?

**Design-to-code bridge (for Step 5 / Phase 2):**
- **Codia AI** (Figma plugin): Select a frame → run plugin → choose SwiftUI → "Get Code" → download Xcode project
- **Figma MCP** (Claude Code integration): `claude mcp add --transport http figma https://mcp.figma.com/mcp` → authenticate → paste Figma URLs into Claude Code prompts to generate SwiftUI

### 4.8 Review and Iterate

Review the designs against the original goals and iterate.

**Self-review checklist:**
- [ ] Does every screen match the visual direction (mood board adjectives)?
- [ ] Is the color palette consistent across all screens?
- [ ] Is the typography hierarchy clear and readable?
- [ ] Do the user flows complete without dead ends?
- [ ] Are empty states, loading states, and error states designed?
- [ ] Do the designs meet accessibility requirements (contrast, touch targets)?
- [ ] Would you screenshot the milestone celebration to share with a friend?
- [ ] Does the paywall clearly communicate value without being pushy?

**Get feedback:**
- Show the prototype to 2-3 people (friends, family, potential users)
- Watch them tap through without guidance — note where they hesitate
- Ask: "What does this app do?" after 10 seconds of browsing. If they can't answer, the design isn't communicating.

## Deliverable

Complete the [UI/UX Spec Template](../templates/uiux-spec.md) and save it to your project's `docs/pre-production/uiux-spec.md`.

## Definition of Done

- [ ] Visual direction defined (personality adjectives, design north star)
- [ ] Reference apps researched in depth (not just listed — patterns extracted and mapped to your app)
- [ ] Reference → Design Decision mapping documented (each wireframe choice traces to a reference)
- [ ] Mood board created (AI-generated or collected, saved to project)
- [ ] Color palette established with hex values and WCAG accessibility verification
- [ ] Typography chosen with type scale defined
- [ ] All user flows mapped with screen sequences and transitions
- [ ] Wireframes created for every screen in the screen inventory
- [ ] High-fidelity designs created for core flow screens (via Stitch/Figma or manual)
- [ ] Interactive prototype for at least the first-launch and daily check-in flows
- [ ] Designs reviewed against accessibility requirements
- [ ] Screen states designed (empty, loading, populated, error)
- [ ] Feedback gathered from at least 2 people

## Tips

- **Start ugly on purpose.** Wireframes should look rough — if they're too polished, you'll resist changing them. The goal is layout and flow, not beauty.
- **Design the celebration first.** The milestone celebration screen is the emotional core of the app and the viral growth engine. If this screen doesn't make users want to screenshot and share, redesign it.
- **Don't skip dark mode.** iOS users expect it. Design for light mode first, then verify your colors work in dark mode. Step 5 (Design System) will formalize the dark mode palette.
- **Test on a real device.** Designs on a desktop monitor feel different on a 6.1" phone screen. Use Figma Mirror or print screenshots at actual phone size.
- **Steal from the best.** Browse Mobbin (mobbin.com) for hundreds of real app designs organized by screen type. See how successful apps solve the same design problems you're facing.
- **Don't over-design v1.** You need enough design to build confidently. You don't need pixel-perfect mockups of every edge case. Wireframes + key screen designs + prototype is sufficient.
- **The AI pipeline is a starting point.** Stitch and Midjourney generate ideas fast, but the output needs human curation and refinement in Figma. AI accelerates; it doesn't replace taste.
