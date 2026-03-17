# Step 7: Project Plan

## Purpose

Transform the specs from Steps 1–6 into a sequenced build plan with milestones, sprints, and task breakdowns. This is the bridge between "what we're building" and "when we're building it." By the end of this step, every user story has a sprint assignment, every sprint has a clear deliverable, and every dependency is accounted for.

## Why This Matters

Without a project plan, you'll start coding wherever feels most exciting and discover mid-build that Feature B depends on Feature A, which you haven't started. Or you'll build a beautiful UI for a screen that has no backend yet. A project plan sequences work so each sprint delivers something functional — not just "code was written" but "users can now do X."

This step formalizes three things:
1. **Milestones** — what does "done" look like at major checkpoints?
2. **Sprint sequence** — what gets built in what order, and why?
3. **Task breakdowns** — what specific work happens in each sprint?

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
- **Stories** — which user stories are included, with IDs
- **Deliverable** — what can the user do at the end of this sprint that they couldn't before?
- **Dependencies resolved** — what does completing this sprint unlock for future sprints?

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

### 7.6 Identify Risks and Mitigations

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

### 7.7 Verify Coverage

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
- [ ] Coverage verified against requirements document

## Tips

- **Plan the sprints, not the calendar.** Don't assign dates yet — assign sequences. Dates come when you start Phase 2 and know your actual velocity.
- **The walking skeleton is your first milestone.** Everything else is embellishment. If the skeleton works, the app works — features just make it better.
- **Over-estimate, don't under-estimate.** Solo devs consistently underestimate by 2–3x. If you think a story takes 2 days, plan for 4. The extra time gets absorbed by debugging, testing, and "oh I forgot about that."
- **Sprint themes keep you focused.** When a sprint is "Reward Loop" and you find yourself refactoring auth, you've drifted. Themes create guardrails.
- **Should Haves go last, not sprinkled throughout.** Putting Should Haves in early sprints delays the core experience. Get Must Haves working first, then layer on enhancements.
- **Tasks should be test-first.** For every feature task, the first sub-task is writing the test. This matches the TDD approach from Step 1 and ensures test coverage isn't an afterthought.
- **Leave buffer for integration work.** Individual stories might work in isolation but break when combined. Plan time for "make Sprint N stories actually work together."
