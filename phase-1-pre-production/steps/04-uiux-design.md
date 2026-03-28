# Step 4: UI/UX Design

## Purpose

Establish the visual direction, user flows, wireframes, and screen designs for the app. By the end of this step, every key screen has a wireframe, the visual language is defined, and interactive prototypes demonstrate the core user flows — so that production code starts from a clear design target, not guesswork.

## Why This Matters

"I'll figure out the UI as I code" is how apps end up with inconsistent layouts, clashing colors, and confusing navigation. Investing in design before production has three benefits:

1. **Speed** — Developers build faster when they know exactly what the screen looks like. No pausing to make design decisions mid-sprint.
2. **Consistency** — A defined visual direction ensures every screen feels like part of the same app. Without it, screens designed months apart drift in style.
3. **Quality** — Design is the biggest differentiator in crowded app categories. Competitors with dated UIs leave an opening for a polished product.

This step is the creative exploration phase — mood, color, typography, layout. Step 5 (Design System) extracts the reusable system from these designs.

### AI-Assisted Workflow (Recommended)

For solo developers, AI tools can collapse the gap between "design concept" and "working code." Two approaches, used together or separately:

**AI Code Generation (v0, Google AI Studio):**

Tools like [v0](https://v0.dev) and Google AI Studio can generate actual UI component code from detailed prompts — not just mockups, but working React/SwiftUI/HTML with real framework components. This is the fastest path from concept to implementation:

1. **Prepare context-rich prompts** for each key page/screen — include the project context, content structure, design direction, and specific components from your stack (e.g., shadcn/ui, SwiftUI native components)
2. **Generate in v0 or AI Studio** — get working code for each page concept. Iterate, pick what resonates, request variations.
3. **The output becomes both the UI/UX spec AND the starting code** — instead of writing a design doc that describes what to build, you have actual components that demonstrate it. The "spec" is the code.
4. **This can merge Steps 4 and 5** — when the generated code uses your component library (shadcn/ui, etc.), it establishes the design system implicitly. Document the design decisions, extract tokens, and you've covered both steps.

The key is prompt quality. Feed these tools your full project context — concept doc, requirements, architecture, content models — not just "make me a blog." The more specific the input, the more useful the output.

**AI Image Generation (visual exploration):**

See the [AI Design Pipeline Reference](../references/ai-design-pipeline.md) for image-generation tools (Midjourney, Stitch, Figma, Codia) that help with mood boards, visual direction, and screen mockups.

**Both approaches work.** Code generation is faster when your stack has strong AI tool support (React + Tailwind + shadcn/ui is ideal for v0). Image generation is better for native platforms or when exploring visual direction before committing to code. The process below works with or without AI tools.

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

**Mood board:**
- Collect 5-10 images that capture the app's atmosphere — use AI generation tools, screenshots from reference apps, or photos that match the intended feeling
- Focus on atmosphere, not specific UI patterns — color temperature, lighting, density, typography weight
- Use design-specific language when searching or prompting: "interface," "layout," "component" — not vague words like "beautiful" or "cool"
- Save the best images to `docs/pre-production/mood-board/` for reference throughout the project

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

**What to include in each user flow:**
- Entry point (how does the user arrive at this flow?)
- Decision points (where does the user choose between paths?)
- Happy path (the ideal sequence from start to goal)
- Error/edge paths (what happens when something goes wrong?)
- Exit points (where does the user end up after completing the flow?)

**Core flows every app needs:**
- First launch (onboarding → core feature)
- Core loop (the action users repeat most often)
- Recovery (error → resolution → back to core loop)
- Settings/account management

Map flows as simple diagrams: boxes for screens, arrows for navigation, diamonds for decisions. The user flow map becomes the test plan for UI testing — each path through the flow is a test case.

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

**HUD/Overlay/Persistent Element Checklist:**

Some apps have elements that persist across multiple screens — game HUDs, floating action buttons, status bars, always-visible toolbars, or notification banners. These don't fit the "one wireframe per screen" model and need explicit attention:

- [ ] Are there always-visible overlay or HUD elements? (score displays, health bars, mini-maps, floating buttons)
- [ ] Which screens do they appear on? (all screens, only during gameplay, only when authenticated)
- [ ] How do overlays interact with navigation? (do they persist during transitions, hide on certain screens?)
- [ ] What's the z-order? (which overlays appear above others)
- [ ] How do overlays respond to safe areas? (notch, home indicator, status bar)
- [ ] Are there multiple overlay states? (expanded/collapsed HUD, notification appearing/dismissing)

If your app has persistent overlays, create a dedicated wireframe showing the overlay layer separately from screen content. This prevents the overlay from being "assumed" in screen wireframes but never explicitly designed.

### 4.6 Design Key Screens

Transform wireframes into high-fidelity designs with real colors, typography, and imagery.

**How to approach high-fidelity design:**
- Use your wireframes as the blueprint — the layout is decided, now apply the visual treatment
- Be specific about layout: "Three stat pills in a row below the title" not "show some stats"
- Use exact color values from your palette, not vague descriptions
- Reference known patterns: "Airbnb-style card layout" or "iOS large title navigation"
- Describe the mood: "celebratory but classy" or "inviting, not aggressive"
- Generate multiple variants per screen and pick the best elements from each
- Whether using AI tools (see [AI Design Pipeline](../references/ai-design-pipeline.md)), Figma manually, or another design tool — the principles are the same

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
3. Import or paste screen designs — each screen as a separate frame
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
- See the [AI Design Pipeline Reference](../references/ai-design-pipeline.md) for tools that translate Figma designs into SwiftUI code
- Generated code is a starting point — extract design values (colors, spacing, layout structure) rather than using generated views directly

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
- [ ] High-fidelity designs created for core flow screens
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
- **AI tools are a starting point.** If you use AI generation tools (see [AI Design Pipeline](../references/ai-design-pipeline.md)), remember that output needs human curation and refinement. AI accelerates; it doesn't replace taste.
- **Code generation can collapse Steps 4 + 5.** If you use v0 or similar tools to generate actual component code with your design system (e.g., shadcn/ui + Tailwind), the output establishes both the UI design and the design system in one pass. Document the design decisions extracted from the generated code rather than writing a separate wireframe spec. Don't let the process create busywork — if the code IS the spec, write a brief design decisions doc and move on.

### Game Development Notes

- **Audio direction belongs in pre-production.** For games where sound is a core polish layer (rhythm games, atmospheric games, games with satisfying feedback loops), define audio direction during this step — not mid-sprint. Identify: soundtrack style, SFX categories (e.g., splat, merge, explosion, UI tap), audio-reactive elements, and whether you'll source, commission, or generate audio assets.
- **Game feel is a design deliverable.** Haptic feedback patterns, screen shake intensity, VFX timing, and animation curves are part of the user experience. Specify them here alongside visual design — they inform production implementation just as much as wireframes do.
- **Portrait vs landscape is a core UX decision for mobile games.** Lock the orientation early and design all screens for it. One-handed reachability in portrait mode constrains button placement and touch target zones differently than landscape.
