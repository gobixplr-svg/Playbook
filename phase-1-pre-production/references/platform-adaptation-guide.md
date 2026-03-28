# Platform Adaptation Guide

The Playbook's step guides are written with iOS (SwiftUI + Supabase) as the default platform. This reference maps each step to platform-specific adaptations for web projects and game development.

Use this guide when starting a project that isn't a standard iOS app. It tells you what to translate, what to skip, and what to add at each step.

---

## Web Projects (React, Vue, Next.js, etc.)

### Step-by-Step Adaptations

| Step | Adaptation |
| ---- | ---- |
| **Step 1: Requirements** | Same process. Replace iOS-specific acceptance criteria (HealthKit, Core Location) with web equivalents (browser APIs, responsive breakpoints). |
| **Step 2: Spikes** | Same process. Spike web-specific unknowns: SSR vs SSG, hosting configuration, API rate limits, SEO crawlability. |
| **Step 3: Architecture** | Replace SwiftUI patterns with your framework's architecture (React components + hooks, Vue composables, etc.). Replace "service protocols" with API client modules or server actions. |
| **Step 4: UI/UX** | **AI code generation is highest-leverage here.** Tools like v0 (React + Tailwind + shadcn/ui) produce working components directly. This can merge Steps 4+5. Design for responsive breakpoints (mobile, tablet, desktop) instead of a single device size. |
| **Step 5: Design System** | If Step 4 used code generation with a component library (shadcn/ui, Radix, etc.), the design system is already established in code. Extract and document tokens (CSS custom properties, Tailwind config). Don't re-spec what the code already defines. |
| **Step 6: API & Data** | If building a static site with no backend, skip this step — note the rationale and move on. If using a headless CMS or API, document the content models and API contracts. |
| **Step 7: Project Plan** | Same process. Web projects often have fewer sprints (simpler architecture). Content-heavy sites should use the [Content Authoring Brief](../templates/content-authoring-brief.md) to plan content alongside features. |
| **Step 8: Dev Environment** | Replace Xcode setup with: package manager (npm/pnpm), dev server, build tooling, hosting config (Vercel/Cloudflare/Netlify), CI/CD for preview deploys. |

### Web-Specific Discovery Questions

Add these to Phase 0 discovery when building a web project:

- What's the hosting/deployment target? (Vercel, Cloudflare Pages, Netlify, self-hosted)
- SSR, SSG, or SPA? (Affects architecture, SEO, and hosting requirements)
- What's the SEO strategy? (Static pre-rendering, dynamic meta tags, sitemap, structured data)
- Is there a content pipeline? (MDX, headless CMS, database, static files)
- What analytics are needed? (Privacy-friendly options: Cloudflare Analytics, Plausible, Fathom)
- What's the responsive strategy? (Mobile-first, desktop-first, specific breakpoints)

### Framework Porting Note

When AI code generation tools (v0, Bolt, etc.) output code in a different framework than your target (e.g., Next.js output but you're using React Router), there's an implicit porting step. Account for this in sprint planning — it's not just copy-paste, it's adapting routing, data loading, and build configuration.

---

## Game Development (Unity, Godot, Unreal)

### Step-by-Step Adaptations

| Step | Adaptation |
| ---- | ---- |
| **Step 1: Requirements** | Add a **Game Mechanics Spec** section between requirements and spikes. Define: scoring formulas, input types (tap, swipe, tilt), difficulty curves, progression systems, game economy (if any). Standard acceptance criteria don't capture gameplay feel. |
| **Step 2: Spikes** | **Critical for games.** Spike core mechanics early — physics performance, rendering pipeline, input feel. A game that doesn't feel good at the spike level won't feel good at launch. Use the [Sprint 0 Spike Tracker](../../phase-2-production/templates/sprint-0-spike-tracker.md) if spikes need full sprint treatment. |
| **Step 3: Architecture** | Replace SwiftUI patterns with engine-native patterns. Unity: MonoBehaviour + ScriptableObject event channels + pure C# services. Godot: Nodes + Signals + Resources. Key: separate game logic from engine code for testability. |
| **Step 4: UI/UX** | Add **audio direction** as a deliverable — SFX categories, music style, audio-reactive elements. Add **game feel spec** — haptics, screen shake, VFX timing, animation curves. Add **HUD/overlay wireframes** separate from screen wireframes. |
| **Step 5: Design System** | Replace shadow tokens with glow tokens for dark-background games. Add gameplay object colors as a token category (blob colors, tile colors, team colors). Engine-native code patterns: ScriptableObject palettes (Unity), Resource themes (Godot). Dark-only is valid for atmospheric games. |
| **Step 6: API & Data** | Most games have no backend. Define: save file schema (JSON/SQLite/PlayerPrefs), external SDK contracts (Game Center, ad networks), data flows between gameplay systems. See "No-Backend / On-Device Projects" section in Step 6. |
| **Step 7: Project Plan** | Sprint 0 (spikes) is almost always needed. The sprint after the walking skeleton is always convergence — budget 40% for discovered work. Device testing should happen by Sprint 2-3, not deferred to polish. |
| **Step 8: Dev Environment** | Replace Xcode setup with engine setup: Unity Hub + URP/HDRP pipeline, assembly definitions, test frameworks (NUnit Edit Mode + Play Mode), build targets (iOS/Android/PC). Include Git LFS for binary assets. |

### Game-Specific Templates

| Template | When to Use |
| ---- | ---- |
| [Sprint 0 Spike Tracker](../../phase-2-production/templates/sprint-0-spike-tracker.md) | First sprint of game production — validate core mechanics |
| [Content Authoring Brief](../templates/content-authoring-brief.md) | Games with authored content (levels, questions, dialogue) |

### Game-Specific Anti-Patterns

- **Designing for 3D when 2D is sufficient.** 3D adds exponential complexity (camera, lighting, mesh optimization). If the game concept works in 2D, build in 2D.
- **Deferring device testing.** The simulator/editor lies about performance, input feel, and safe areas. Export to a real device by Sprint 2.
- **Building UI before gameplay.** Get the core loop feeling good before investing in menus, settings, and polish screens. If the gameplay isn't fun, nothing else matters.
- **No integration buffer on convergence sprints.** Game systems are especially prone to invisible coupling. The first full playthrough always generates 1-2 sessions of discovered work.
- **Art estimation without spec.** Art/VFX work without a written spec takes 2-3x longer due to iteration. Spec it first, even if the spec is just "5 bullet points and a reference image."

### Safe Area Testing

For mobile games, safe area testing should be planned in pre-production, not discovered mid-sprint:

- **Portrait games:** consistent padding-top (notch) and padding-bottom (home indicator) across all screens
- **Landscape games:** safe areas on left/right (notch) plus top/bottom (status bar, home indicator)
- **Test in iOS Simulator in the correct orientation** — Device Simulator in the wrong orientation mode has misled UI testing in multiple projects
- **Define safe area constants in the design system** — don't hard-code per-screen

---

## Both Platforms: Content Strategy

For content-heavy projects (blogs, content apps, trivia games, educational tools), add a **content strategy step** to Phase 0 or early Phase 1:

1. **Content inventory** — what content does the app need to function?
2. **Content timeline** — when must content exist relative to feature sprints?
3. **Content authoring approach** — manual, AI-assisted, sourced, or user-generated?
4. **Minimum viable content** — smallest set that makes the core feature testable

Use the [Content Authoring Brief](../templates/content-authoring-brief.md) template to formalize this.
