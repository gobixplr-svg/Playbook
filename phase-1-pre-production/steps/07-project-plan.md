# Step 7: Project Plan

## Purpose

Transform the specs from Steps 1–6 into a sequenced build plan with milestones, sprints, and task breakdowns. This is the bridge between "what we're building" and "when we're building it." By the end of this step, every user story has a sprint assignment, every sprint has a clear deliverable, and every dependency is accounted for.

## Why This Matters

Without a project plan, you'll start coding wherever feels most exciting and discover mid-build that Feature B depends on Feature A, which you haven't started. Or you'll build a beautiful UI for a screen that has no backend yet. A project plan sequences work so each sprint delivers something functional — not just "code was written" but "users can now do X."

This step formalizes three things:
1. **Milestones** — what does "done" look like at major checkpoints?
2. **Sprint sequence** — what gets built in what order, and why?
3. **Task breakdowns** — what specific work happens in each sprint?

## When to Simplify

This step scales with project complexity:

- **Small projects (< 10 stories)** — The dependency graph, walking skeleton, and sprint sequence can be a single section rather than separate subsections. One sprint may cover the entire build. Still define milestones and task breakdowns.
- **Web projects with AI code-gen** — If Steps 4-5 collapsed into working code, the "project plan" may be a short list of remaining integration work rather than a multi-sprint sequence. Still identify the walking skeleton and convergence point.
- **Solo weekend projects** — A bullet list of "build order" with size estimates is sufficient. Don't create process overhead that exceeds the project's complexity.

Never skip this step entirely — even a 10-minute planning pass prevents the "build whatever feels exciting" trap that leads to half-finished features.

## Prerequisites

- Step 1 (Requirements) completed — user stories with acceptance criteria and MoSCoW priorities
- Step 3 (Architecture) completed — system architecture, walking skeleton, service dependencies
- Step 6 (API & Data) completed — database schema, API contracts, data flow dependencies

## Process

### 7.1 Map the Dependency Graph

Before sequencing anything, identify what blocks what. Pull from three sources:

**From Requirements (Step 1):**
- Which stories have explicit prerequisites? (e.g., "Route Progress requires Activity Conversion")
- Which epic must be partially complete before another can start?

**From Architecture (Step 3):**
- Which services depend on other services?
- What's the walking skeleton — the minimal set of stories that produce an end-to-end loop?

**From API & Data (Step 6):**
- Which tables have foreign keys into other tables?
- Which data flows require upstream data to exist first?

**Dependency types:**
- **Hard blocker** — cannot start until dependency is complete (e.g., route progress needs conversion engine)
- **Soft dependency** — can start earlier but benefits from dependency (e.g., streaks enhance trophy case but aren't required)
- **Independent** — no dependency, can be built in any order (e.g., onboarding tutorial)

**Output:** A dependency table listing each story, what it blocks, and what blocks it.

**Dependency table format:**

| Story | Depends On | Type | Reason |
| ------- | ----------- | ------ | -------- |
| E3-S3 (Progress) | E2-S2 (Conversion) | Hard | Progress needs converted distance data |
| E5-S1 (Badges) | E3-S3 (Progress) | Hard | Badge criteria evaluate progress milestones |
| E5-S3 (Streaks) | E5-S2 (Trophy Case) | Soft | Streaks display in trophy case but aren't required for it |
| E1-S4 (Onboarding) | — | Independent | Can be built anytime after core loop exists |

Build this table by walking through each story and asking: "What must exist before I can build this?" If the answer is "nothing," it's independent. If the answer is "this other story's output," it's a hard or soft dependency.

### 7.2 Define the Walking Skeleton

The walking skeleton is the smallest set of stories that produces a functional end-to-end loop. It proves the architecture works, validates the core user experience, and gives you something real to test against.

**To identify the walking skeleton:**
1. Start from the core user action (the "magic moment" — when the user first experiences value)
2. Trace backward: what must exist for that moment to happen?
3. Trace forward: what must exist for that moment to feel complete?

The walking skeleton typically spans 1–2 sprints and covers auth → data input → core logic → primary visualization.

**Output:** A numbered list of stories that form the walking skeleton, in build order.

### 7.3 Sequence Stories into Sprints

Group user stories into sprints following these principles:

**Sprint sequencing rules:**
1. **Dependencies first** — stories that block others go in earlier sprints
2. **Must Haves before Should Haves** — MoSCoW priorities drive ordering within a sprint
3. **Walking skeleton first** — the end-to-end loop ships before embellishments
4. **Each sprint delivers a demo-able increment** — not just "backend work" but something a user could see
5. **Balance sprint size** — aim for similar effort per sprint (3–5 stories, depending on complexity)

**Sprint structure:**
- **Sprint name** — a descriptive label (not just "Sprint 1")
- **Theme** — what capability does this sprint deliver?
- **Sprint type** — Construction, Convergence, or Polish (see below)
- **Stories** — which user stories are included, with IDs
- **Deliverable** — what can the user do at the end of this sprint that they couldn't before?
- **Dependencies resolved** — what does completing this sprint unlock for future sprints?

#### Sprint Type Classification

Not all sprints are the same. Failing to recognize the type leads to under-planning convergence work and over-planning polish work.

| Type | Characteristics | Planning Approach |
| ---- | --------------- | ----------------- |
| **Construction** | Building new systems in relative isolation. Each story maps to a distinct system or feature. | Plan by system. Stories are independent. Estimate normally. |
| **Convergence** | Making independently-built systems work together as a product. First time the full user flow runs end-to-end. | Budget 40% for discovered work. Include integration testing, balance tuning, and visual/UX iteration as explicit tasks. Plan a "First Full Playthrough" session early. |
| **Polish** | Refining a working product. Tuning feel, fixing edge cases, visual improvements. | Plan by feel-target, not feature list. Time-box iteration (e.g., "VFX tuning: 1 session max"). |

**Why this matters:** In Fizzics (a physics puzzle game), Sprint 4 was planned as Construction (6 stories: Game Center, Ads, Art ×4) but was actually Convergence — the first time all systems had to work together as a shippable product. The sprint delivered 105 tasks instead of the planned 30. Integration bugs surfaced (dead overflow detection, broken event system), design gaps became visible (difficulty curve too shallow, requiring a full rebalance), and art/UI work revealed the need for system architecture (not just asset swaps). Two stories were deferred to the next sprint.

The pattern is consistent across projects: the sprint after the walking skeleton is functional is always a convergence sprint. Plan for it.

**Sprint duration guidance:**
- Solo dev: 2–3 weeks per sprint
- Small team: 1–2 weeks per sprint
- Adjust based on story complexity — some sprints will be faster than others

### 7.4 Break Stories into Tasks

For each story in each sprint, break it down into implementation tasks. These are the actual units of work — what you'll pick up, build, test, and commit.

**Task granularity:**
- Each task should take roughly half a day to a full day
- Tasks should be independently committable (the project compiles after each one)
- Follow the TDD cycle: write test → implement → verify

**Task categories (use these to structure each story):**
1. **Data layer** — models, database operations, migrations
2. **Service layer** — business logic, protocol implementations
3. **View layer** — SwiftUI views, view models
4. **Integration** — connecting services to views, wiring dependencies
5. **Tests** — unit tests, integration tests, UI tests (per the test plan from Step 1)

**Task format:**
```
- [ ] [Task description]
      Story: [Story ID]
      Layer: [Data / Service / View / Integration / Tests]
      Estimate: [S / M / L]
```

Sizes: **S** = < half day, **M** = half day to full day, **L** = 1–2 days

These are **effort** sizes, not wall-clock. See 7.4.1 for why that distinction matters when AI agents are in the loop.

### 7.4.1 Estimating in the AI-Agent Era: Wall-Clock vs. Effort

The traditional "size estimate" assumes one human typing on one keyboard. When you delegate work to an AI agent — especially one running in a separate worktree while you do something else — that assumption breaks. A 5-hour effort can be a 30-minute wall-clock. A 1-hour effort can be a 4-hour wall-clock if you're context-switching across several agent sessions.

For any task with effort >30 minutes, estimate **two** numbers:

- **Wall-clock** — how long before you can review the output. Drives "before the meeting?" / "tonight or tomorrow?" decisions and what stakeholders actually wait for.
- **Effort** — the size and complexity of the change itself. Drives review burden and scope-vs-bandwidth tradeoffs.

**Quote both numbers, and name the strategy.** Don't quote a single human-developer number then silently delegate. The recommended format:

> "Wall-clock ~30-45 min via agent delegation. Effort-equivalent of 5-7 hours of focused human work — meaning the diff will be substantive."

When choosing between strategies, name the trade-off:

> "5-7 hours if I do it inline; 30-45 min if I delegate to an agent in a worktree (recommended for this size)."

Inline work is incremental — you see issues mid-build, ask questions, course-correct. Agent-delegated work shows up complete: bigger upfront review, fewer course corrections, harder to redirect mid-stream. These are different commitments, not just different timelines.

**Calibration biases to track.** After a few projects, you'll know where your gut is wrong. Common patterns:

- **Design + brand + prototype work:** AI agents tend to overdeliver here. If you've been quoting based on human time, you're likely overestimating by 3-10× when delegating. Adjust down.
- **Debug + investigation + integration:** Root causes surprise. The agent finds a layer-4 bug when you scoped a layer-1 fix. Underestimate by 2-3×. Adjust up.
- **Build features end-to-end:** Usually accurate within 2× either direction.
- **Doc / spec / runbook writing:** Usually accurate.

These are general patterns. Your project-specific biases are more useful — log misses in `docs/sprints/sprint-N/retro.md` so you can re-derive estimates from real data instead of from gut feel. Phase 5 retrospectives should explicitly review estimating accuracy.

**When someone pushes back on your estimate ("that feels off"):** don't defend the number. Re-derive from your calibration log. Which category is this work in? What's the bias on that category? Is there a project-specific reason it should be different?

### 7.5 Define Milestones

Milestones are meaningful checkpoints — moments where the project reaches a qualitatively different state. They're bigger than "sprint complete" and mark real capability shifts.

**Good milestones:**
- "Core loop functional" — user can sign in, import data, pick a route, and see progress
- "App is monetizable" — subscription and paywall working
- "Feature complete" — all v1 stories implemented

**Bad milestones:**
- "Backend done" — too vague, not user-visible
- "Sprint 3 complete" — just a date, not a capability

**For each milestone:**
- Name (capability-focused)
- Target sprint
- Stories that must be complete
- Demo scenario (how you'd show this to someone)

**Milestone design principles:**
- **Name milestones after capabilities, not sprints** — "Core Loop Functional" communicates progress; "Sprint 2 Complete" doesn't.
- **Limit to 3–5 milestones** — more than 5 and they stop feeling meaningful. Each milestone should be a celebration point.
- **Milestones should be demo-able** — if you can't show it to someone in 60 seconds, it's not a real milestone.
- **The first milestone is the most important** — it proves the architecture works and gives you something to build on. Prioritize getting there fast.

**Milestone table format:**

| Milestone | Target Sprint | Key Stories | Demo Scenario |
| ----------- | --------------- | ------------- | --------------- |
| Core Loop Functional | Sprint 2 | E1-S1, E2-S1, E3-S1–S3, E4-S1 | Sign in → import activity → pick route → see progress on map |

The "Demo Scenario" column forces you to think about milestones from the user's perspective, not the developer's. If you can't write a scenario, the milestone is too abstract.

### 7.6 Expert Review Checkpoint

Before finalizing the project plan, consult platform-specific expertise to surface compliance gaps, architectural blind spots, and submission risks that the Playbook's generic steps don't cover.

**Why this exists:** In KnowYou (a social trivia app), expert review before the project plan surfaced 10+ compliance and design gaps — account deletion (GDPR + App Store), UGC moderation requirements, PrivacyInfo.xcprivacy manifests, ad consent flow ordering (UMP → ATT → AdMob), and paywall legal link requirements. Every one of these would have been a Sprint 3-5 blocker requiring simultaneous updates to 4+ specs. The review saved an estimated full sprint of rework.

**Review areas by platform:**

| Platform | Key Review Areas |
| ---- | ---- |
| **iOS / App Store** | PrivacyInfo.xcprivacy, ATT consent flow, account deletion requirement, age rating, in-app purchase rules, UGC moderation policy |
| **Web** | GDPR/CCPA consent, cookie policy, accessibility (WCAG), hosting/CDN configuration, SEO technical requirements |
| **Game (any platform)** | Age rating with interactive elements, ad SDK compliance, virtual currency regulations, loot box disclosure requirements |

**How to run the review:**

1. **Gather your specs** — requirements, architecture, API design, and UI/UX spec
2. **Review against platform guidelines** — Apple's [App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/), Google's [Play Console requirements](https://play.google.com/console/about/), or your target platform's submission checklist
3. **Use AI assistance** — ask an AI coding assistant to audit your specs against the platform guidelines. Prompt: "Review these specs against [platform] submission requirements. What compliance gaps exist?"
4. **Document findings** — for each gap, note the requirement, which spec needs updating, and whether it affects the sprint plan
5. **Update specs** — cross-cutting concerns discovered here typically require updates to 2-4 specs simultaneously (requirements, architecture, API, UI/UX). Update them all before finalizing the project plan.

**When to skip:** If you've shipped multiple apps to the same platform and your specs already account for platform requirements, this is a quick sanity check (5 minutes) rather than a deep review. If this is your first submission to a platform, invest 30-60 minutes.

**Output:** Compliance items integrated into the sprint plan as explicit tasks, not afterthoughts discovered mid-sprint.

### 7.7 Cross-Spec Consistency Verification

Before declaring the project plan final, verify that identifiers are consistent across all specs produced in Steps 1-6. Naming drift between specs creates ambiguity during implementation — the developer (or AI) picks whichever name it finds first, leading to mismatches between database columns, service methods, and UI labels.

**What to verify:**

| Identifier Type | Check Across |
| ---- | ---- |
| Story IDs (E1-S1, etc.) | Requirements ↔ Project Plan ↔ Sprint tasks |
| Data model names | Requirements ↔ Architecture ↔ API Spec |
| Database table/column names | Architecture ↔ API Spec |
| Service protocol names | Architecture ↔ API Spec ↔ Project Plan |
| Screen/view names | UI/UX Spec ↔ Requirements ↔ Design System |
| Design token names | Design System ↔ UI/UX Spec |

**How to run the check:**
1. Pick one spec as the source of truth for each identifier type (usually Requirements for story IDs, Architecture for model names, API Spec for table names)
2. Search for each identifier in the other specs
3. Flag mismatches (e.g., `users` table in architecture but `user_profiles` in API spec)
4. Resolve by updating the non-authoritative spec to match the source of truth

**Why this exists:** In KnowYou, a `users` vs `user_profiles` table name mismatch between the architecture and API specs was only caught during the Phase 1 retrospective. Catching it earlier — before any code references the wrong name — is free. Catching it during Sprint 3 means renaming across models, services, queries, and tests.

**When to skip:** If a single person wrote all specs in one continuous session, consistency is likely high. If specs were written across multiple sessions or by multiple contributors (including AI), always run this check.

### 7.8 Identify Risks and Mitigations

Every project has risks. Naming them upfront with a mitigation plan prevents surprises.

**Risk categories:**
- **Technical** — third-party API changes, SDK incompatibilities, performance issues
- **Scope** — feature creep, underestimated complexity, unclear requirements
- **External** — App Store review, backend service outages, rate limits
- **Personal** — burnout, context switching, motivation dips (real for solo devs)

**For each risk:**
- Description
- Likelihood (Low / Medium / High)
- Impact (Low / Medium / High)
- Mitigation strategy

### 7.9 Verify Coverage

Cross-reference the plan against requirements:

**Verification checklist:**
- [ ] Every Must Have story is assigned to a sprint
- [ ] Every Should Have story is assigned to a sprint
- [ ] Dependencies are respected (no story appears before its blocker)
- [ ] The walking skeleton is complete by the end of the first 1–2 sprints
- [ ] Each sprint has a clear, demo-able deliverable
- [ ] Milestones mark real capability shifts
- [ ] Risks are identified with mitigation strategies
- [ ] Test plan from Step 1 is accounted for in task breakdowns

## Deliverable

Complete the [Project Plan Template](../templates/project-plan.md) and save it to your project's `docs/pre-production/project-plan.md`.

## Definition of Done

- [ ] Dependency graph mapped (hard blockers, soft dependencies, independent stories)
- [ ] Walking skeleton identified with specific stories
- [ ] All user stories assigned to sprints in dependency-safe order
- [ ] Each sprint has a theme, story list, and demo-able deliverable
- [ ] Stories broken into implementation tasks with size estimates
- [ ] Milestones defined with target sprints and demo scenarios
- [ ] Risks identified with mitigation strategies
- [ ] Expert review completed for target platform (or explicitly skipped with rationale)
- [ ] Cross-spec consistency verified (story IDs, model names, table names, service names match across all specs)
- [ ] Coverage verified against requirements document

## Tips

- **Plan the sprints, not the calendar.** Don't assign dates yet — assign sequences. Dates come when you start Phase 2 and know your actual velocity.
- **The walking skeleton is your first milestone.** Everything else is embellishment. If the skeleton works, the app works — features just make it better.
- **Quote two estimates, not one.** Wall-clock and effort have decoupled now that AI agents handle work in parallel. See 7.4.1 — single-number estimates either misrepresent timeline (when you delegate) or misrepresent review burden (when you don't say you delegated). Both numbers, every time, for any task >30 min.
- **Track your calibration biases per project.** Solo devs without AI tend to underestimate effort by 2-3×. With AI agents, the bias splits by work type — overestimate creative work when delegating, underestimate debug work always. Log misses in sprint retros so you can re-derive next time.
- **Sprint themes keep you focused.** When a sprint is "Reward Loop" and you find yourself refactoring auth, you've drifted. Themes create guardrails.
- **Should Haves go last, not sprinkled throughout.** Putting Should Haves in early sprints delays the core experience. Get Must Haves working first, then layer on enhancements.
- **Tasks should be test-first.** For every feature task, the first sub-task is writing the test. This matches the TDD approach from Step 1 and ensures test coverage isn't an afterthought.
- **Leave buffer for integration work.** Individual stories might work in isolation but break when combined. Plan time for "make Sprint N stories actually work together."
- **Run a design sanity check before each sprint.** Before starting a sprint, ask: "Does the current design hold up? Are the numbers right? Have we tested the feel?" This is especially important before convergence sprints. A 5-minute question like "are 4 tiers enough depth for an endless game?" can prevent a multi-day rebalance mid-sprint.
- **Require design specs for L-sized stories.** Small and medium stories can be implemented directly. Large stories benefit from a written spec before coding starts — even a one-page outline of what "done" looks like, what decisions need to be made, and what iteration is expected. This is validated: UI passes succeed when spec'd upfront; VFX work takes 2–3× longer without one.
