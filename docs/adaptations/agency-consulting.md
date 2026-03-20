# Adaptation: Agency & Consulting Projects

How to adapt the Development Playbook for marketing agency work, client-facing projects, and consulting engagements.

---

## When to Use This Adaptation

Use this when:
- You're building for a client, not for yourself
- Someone else designed the product (you're executing, not concepting)
- There's a client approval gate between phases
- The deliverable includes handoff documentation, not just working software
- The engagement is measured in weeks, not months

## Phase Compression: 6 → 4

The full Playbook (6 phases) is designed for products you own and operate long-term. Agency projects are shorter, more design-driven, and have external approval gates. Compress to four phases:

| Full Playbook | Agency Adaptation | Why |
|---|---|---|
| Phase 0: Concept | Phase 0: Discovery | Client already has the concept. Your job is to understand their goals, not invent the product. |
| Phase 1: Pre-Production | Phase 1: Design & Setup | Design often comes from a separate team. Your pre-production is extracting tokens, scaffolding, and documenting architecture. |
| Phase 2: Production | Phase 2: Build | Same sprint-based approach, but with client review gates between sprints. |
| Phase 3: Testing & QA | _(Folded into Build)_ | QA happens within build sprints as Tier 1.5 visual checks, not as a separate phase. |
| Phase 4: Launch | Phase 3: Launch | Same deployment discipline, plus client handoff documentation. |
| Phase 5: Post-Launch | Phase 4: Optimize (retainer only) | Only applies if the client engagement continues. Many agency projects end at launch. |

## Key Differences from the Standard Playbook

### 1. Client Gates Replace Internal Gates

In the standard Playbook, you decide when to advance phases. In agency work, the client decides.

**Standard gate:** "Am I confident the design is solid enough to build?"
**Agency gate:** "Has the client explicitly approved the design?" (Written confirmation, not verbal.)

This changes the cadence. You may be ready to build on Tuesday, but the client doesn't review until Friday. Build your timeline around their availability, not yours.

### 2. You Don't Control the Design

The standard Playbook assumes you're designing and building. Agency work often splits these roles:
- A designer (who may use different tools than you) creates the visual design
- You extract the design system and build from their specs
- They may or may not be available for questions during the build

**Implication:** Phase 1 needs an explicit "design token extraction" step where you translate the visual design into CSS custom properties, documented in the project CLAUDE.md, before any code is written.

**Implication:** The design-to-code pipeline matters enormously. See `phase-1-pre-production/references/design-to-code-pipeline.md` for the Figma MCP and XD fallback workflows.

### 3. Visual QA Is Non-Negotiable

The standard Playbook's Tier 1.5 (visual smoke tests) is recommended. In agency work, it's mandatory. Here's why:

The client approved a specific design. If your build doesn't match, trust erodes immediately. Every visual mismatch = one more round of revision = one more week of the engagement.

**Process:** After every sprint (or every section build):
1. Open at mobile width (375px) — compare to mobile design
2. Open at desktop width (1400px) — compare to desktop design
3. Fix mismatches before showing the client
4. Document what was checked in the playbook state file

**Lesson learned (agency project, March 2026):** We went through 5+ rounds of annotated screenshots from the client comparing the build to their XD mockups. Issues included: wrong background colors, incorrect content ordering on mobile, CSS specificity bugs, carousel text cutoff, timeline not centered. All of these could have been caught with a disciplined Tier 1.5 pass before showing the client.

### 4. Scope Management Is Active

The standard Playbook assumes you control the feature set. Agency work introduces:

**Change requests:** "Can we also add a FAQ section?" mid-build. You need a clear response: "Yes, that's an addition to scope. Here's what it means for timeline and cost." Or: "Yes, that's within the original scope — I'll add it to the current sprint."

**Content delays:** "We'll get you the testimonials by Wednesday." Wednesday comes. No testimonials. Your build is blocked. Plan for this by using realistic placeholder content and building sections that can accept real content later.

**Design changes after approval:** "Actually, can we make the hero section wider?" After you've already built it. The Phase 1 gate (design approval before code) exists to prevent this. When it happens anyway, document the change and its impact.

### 5. Deliverables Extend Beyond Code

The standard Playbook's final deliverable is working software. Agency work adds:

- **Client handoff documentation** — How to edit content, where to find things, who to call when something breaks
- **Analytics setup** — Not just "the site works" but "the site measures what matters"
- **SEO baseline** — Meta tags, structured data, Search Console verification
- **ACF field guide** (WordPress) — "Click here to change the headline. Click here to add a testimonial."
- **Maintenance notes** — What dependencies need updating, what's hosted where, what's the deployment process

### 6. Retrospectives Include Client Feedback

The standard retrospective asks: "What worked? What didn't? What do we change?"

The agency retrospective adds: "What did the client think? Where did we over-deliver? Where did we under-deliver? Would they hire us again?"

This feedback loop improves your client-facing process, not just your technical process.

---

## Discovery (Phase 0) Adaptation

### What Changes
- **No Guided Concept Session** — The client already has a business. You're not ideating.
- **No Feasibility Assessment** — You've already agreed to build it. (Do feasibility before signing the contract, not after.)
- **No App Naming** — The client has a name.

### What Stays
- **Discovery Questions** — Adapted as a "Client Intake Brief": business goals, target audience, competitive landscape, brand assets, technical requirements
- **Competitive Audit** — Review competitor websites, identify differentiation opportunities
- **Go/No-Go** — Replaced by "scope agreement": client and agency agree on what's being built before any design or code

### New Steps
- **Technical Scope Document** — Site type, hosting, CMS, integrations, content responsibilities
- **Brand Asset Collection** — Logo, colors, fonts, photography. What they have vs. what you need.

---

## Design & Setup (Phase 1) Adaptation

### What Changes
- **No UI/UX Design step** — Design comes from the client's design team. Your job is extraction, not creation.
- **Requirements Spec simplified** — The design IS the spec. Your requirements are technical: "How do we build what they designed?"
- **Architecture Design simplified** — You're choosing a CSS prefix and responsive strategy, not designing a database schema.

### What Stays
- **Design System** — Adapted as "Design Token Extraction": take the visual design and produce CSS custom properties
- **Dev Environment Setup** — Scaffold the project, wire up Bootstrap, create the CLAUDE.md
- **Retrospective** — Reflect on Phase 1 before starting to build

### New Steps
- **Design Approval Gate** — Explicit sign-off from the client before building. This is the most important gate in the agency adaptation.

---

## Build (Phase 2) Adaptation

### What Changes
- **Sprint cadence is faster** — Agency sprints are 1-3 days, not 1-2 weeks. You're building sections, not features.
- **Tier 1.5 visual QA is mandatory** — After every section, compare to the design at mobile + desktop
- **Client review is a gate** — At least one client review before launch, ideally after Sprint 2 (all sections built)

### What Stays
- **Sprint execution model** — Build, check, commit, repeat
- **Vertical slices** — Build each section completely (HTML + CSS + JS + responsive) before moving to the next
- **Always shippable** — The site should look good at every stage, not "it'll come together at the end"

### New Steps
- **Client Review Sprint** — A dedicated sprint for presenting the build, collecting feedback, and implementing revisions. May repeat.

---

## Launch (Phase 3) Adaptation

### What Changes
- **No app store submission** — It's a website, not an app
- **No beta testing** — There's no TestFlight equivalent for a marketing site (staging URL serves this purpose)

### What Stays
- **Preflight checklist** — Comprehensive pre-launch scan
- **Deployment verification** — Post-deploy checks

### New Steps
- **SEO & Analytics Setup** — Meta tags, structured data, Google Analytics, conversion tracking
- **Client Sign-Off** — Written approval to go live
- **Client Handoff Documentation** — How to edit content, manage the site, who to call

---

## When to Use the Full 6-Phase Playbook Instead

Use the full Playbook (not this adaptation) when:
- The client engagement is 3+ months
- You're building a web application, not a marketing site
- There's a backend, database, authentication, or API involved
- The client wants you to design AND build (no separate design team)
- The project will be maintained and iterated on by your team long-term

The agency adaptation is for projects where design is done, scope is fixed, and the deliverable is a polished frontend that meets the design spec.

---

## Case Study: Agency Landing Page (March 2026)

**Engagement:** Rebuild a marketing agency's consultation landing page from Adobe XD design specs.

**Stack:** WordPress (FoundationPress theme), Bootstrap-inspired custom CSS, jQuery.

**What we learned:**
1. **5 rounds of visual QA** before the build matched the XD. Disciplined Tier 1.5 checks after each section would have caught most issues in 1-2 rounds.
2. **CSS specificity battles** with the existing WordPress theme consumed significant debugging time. The CSS prefix strategy (e.g., `lc-` for landing-consultation) was essential.
3. **XD screenshots are a lossy communication format.** Annotated screenshots miss details (exact pixel values, font weights, border radii). Figma MCP would have eliminated this friction.
4. **The AI development package** (CLAUDE.md + skills + adapted playbook) became a more valuable deliverable than the landing page itself. The code is one project; the system serves every future project.
5. **Stakeholder dynamics matter.** The dev team was ready to adopt Figma and new tools. The design team needed a separate conversation. "Recommend but don't force" was the right approach.

**Result:** 4-phase adaptation documented, AI development package delivered, design-to-code pipeline recommendation made. Landing page shipped with all 5 client-requested fixes addressed in a single working session.
