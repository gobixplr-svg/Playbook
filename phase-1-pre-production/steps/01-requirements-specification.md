# Step 1: Requirements Specification

## Purpose

Formalize the features from the Concept Document into detailed, testable requirements. By the end of this step, every feature has a user story, acceptance criteria written in Given/When/Then format, and a test plan — so that when you start building, you know exactly what "done" looks like and how to verify it.

## Why This Matters

The Concept Document captures *what* the app does. Requirements capture *how users experience it* — at a level of detail that can drive both development and testing. This step bridges the gap between "we need HealthKit integration" and "when the user grants HealthKit permission, their last 7 days of step data imports within 10 seconds."

Well-written acceptance criteria are also your TDD test specifications. Write them now, and you've already written your test plan before a single line of code exists.

## Prerequisites

- Phase 0 Concept Document completed with a "Go" or "Conditional Go" decision
- MoSCoW feature list defined (Must Have, Should Have, Could Have, Won't Have)
- Target audience and core value proposition established

## Process

### 1.1 Review Phase 0 Outputs

Before writing anything new, re-read the key Phase 0 deliverables:

- **Concept Document** — especially the MoSCoW feature prioritization, core user flows, and risk register
- **Discovery Questionnaire** — for scope decisions and deferred items
- **Technical Feasibility** — for platform constraints and complexity assessments
- **Vision Statement** — for the core values that guide tradeoff decisions

**Flag anything that's changed** since Phase 0. New insights, shifted priorities, or resolved research items should be noted before formalizing requirements.

### 1.2 Define Epics

Group the MoSCoW features into logical epics. An epic is a large body of work that delivers a cohesive piece of user value.

**Guidelines:**
- Each epic maps to a major capability area (not a screen or a technical layer)
- Must Have and Should Have features should be distributed across epics — don't create a "nice-to-haves" epic
- Aim for 5-8 epics for a typical v1

**Example epic structure:**
| Epic | Description | Features Included |
| ------ | ------------- | ------------------- |
| Route System | Browse, select, and track progress on routes | Route library, route detail, progress tracking |
| Activity Tracking | Pull and convert movement data | HealthKit integration, activity conversion engine |

### 1.3 Write User Stories

For each Must Have and Should Have feature, write a user story:

> **As a** [type of user], **I want** [capability] **so that** [benefit/outcome].

**Guidelines:**
- Write from the user's perspective, not the developer's. "As a user, I want to see my position on a trail map" not "The app renders a Mapbox overlay."
- One story per distinct piece of user value. If a story has "and" in it, consider splitting.
- Include the MoSCoW priority (Must Have / Should Have) with each story.
- Reference the Concept Document feature it maps to — traceability matters.

**Story sizing:**
- A story should be completable in 1-5 days of development
- If it's bigger, break it into smaller stories within the same epic
- If it's smaller than half a day, it might be a task within another story, not its own story

### 1.4 Define Acceptance Criteria

For each user story, write acceptance criteria in **Given/When/Then** format:

> **Given** [precondition/context],
> **When** [action/trigger],
> **Then** [expected outcome].

**Guidelines:**
- Each story should have 2-6 acceptance criteria
- Cover the happy path first, then edge cases
- Be specific and measurable — "loads quickly" is not testable; "loads within 3 seconds on cellular" is
- Include negative cases — what should NOT happen
- These criteria become your TDD test specifications in Phase 2

**Example:**
```
Story: As a user, I want to see my current position on a trail map so that I know how far I've come.

AC1: Given the user has an active route with logged distance,
     When they open the route detail view,
     Then their position pin appears at the correct point along the trail path.

AC2: Given the user has logged more distance since last viewing,
     When they open the route detail view,
     Then the position pin reflects the updated distance.

AC3: Given the user has completed 100% of the route distance,
     When they open the route detail view,
     Then the position pin appears at the endpoint and a completion badge is displayed.
```

### 1.5 Write Test Plans

For each user story, categorize its acceptance criteria by test type:

| Test Type | What It Verifies | When It Runs |
| ----------- | ----------------- | -------------- |
| **Unit** | Business logic in isolation (calculations, data transforms, state changes) | Every commit (CI) |
| **Integration** | Components working together (API calls, data persistence, HealthKit queries) | Every commit (CI) |
| **UI** | User-visible behavior (screens render correctly, navigation works, interactions respond) | Every PR / nightly |
| **Snapshot** | Visual consistency (components look the same after changes) | Every PR |

**For each acceptance criterion, assign a test type:**
```
AC1 (position on map): Integration test — requires route data + distance calculation + map rendering
AC2 (updated distance): Unit test — distance calculation logic; Integration test — data refresh flow
AC3 (completion): Unit test — completion detection logic; UI test — completion badge display
```

This mapping tells you exactly what test infrastructure you'll need in Step 8 (Dev Environment) and gives you a ready-made test plan for Phase 2 (Production).

### 1.6 Identify Edge Cases & Constraints

For each epic, document:

**Edge cases:**
- What happens at boundaries? (0 distance, max distance, route completion, empty states)
- What happens with bad/missing data? (HealthKit permission denied, no network, corrupted data)
- What happens with unusual user behavior? (starting multiple routes, deleting account mid-route)

**Platform constraints:**
- HealthKit background refresh limits
- Mapbox API rate limits or offline requirements
- Supabase free tier boundaries
- App Store review requirements (HealthKit usage description, privacy manifest)

**Business constraints:**
- Free vs. premium feature boundaries
- Data retention policies
- Privacy requirements (what data is stored, where, for how long)

### 1.7 Prioritize & Sequence

Order the epics and stories by:

1. **Dependencies** — What must be built before other things can work? (e.g., user accounts before cloud sync, route data before route display)
2. **Risk reduction** — Build the hardest/riskiest things early so you learn fast
3. **User value** — Deliver a usable experience as early as possible (vertical slices over horizontal layers)

Create a dependency map showing which stories block others. This feeds directly into Step 7 (Project Plan).

**Identify the "walking skeleton"** — the minimal set of stories that, when complete, produce a functioning (if bare-bones) app. This is your first sprint target in Phase 2.

## Deliverable

Complete the [Requirements Document Template](../templates/requirements-document.md) and save it to your project's `docs/pre-production/requirements-document.md`.

## Definition of Done

- [ ] Phase 0 outputs reviewed and any changes flagged
- [ ] Features grouped into 5-8 epics
- [ ] User stories written for all Must Have features
- [ ] User stories written for all Should Have features
- [ ] Acceptance criteria defined in Given/When/Then format for every story
- [ ] Test plan assigned (unit / integration / UI / snapshot) for every acceptance criterion
- [ ] Edge cases documented per epic
- [ ] Platform and business constraints documented
- [ ] Stories prioritized with dependency map
- [ ] Walking skeleton identified
- [ ] Every story traces back to a Concept Document feature (no orphan requirements)

## Tips

- **Don't design the solution.** User stories describe what the user experiences, not how it's implemented. "See my position on a map" is a requirement. "Use ST_LineInterpolatePoint to calculate position" is architecture (Step 3).
- **Write acceptance criteria you'd bet money on.** If the criterion is vague enough that two developers would interpret it differently, it's not specific enough.
- **The test plan is not optional.** Writing test plans now feels premature, but it forces you to think about testability before writing code. This is the foundation of TDD.
- **Should Haves are real requirements too.** They get full stories and acceptance criteria, not just a bullet point. The only difference from Must Haves is that v1 can ship without them if time runs short.
- **Don't add features.** This step formalizes what's already in the Concept Document. If you discover a new feature, add it to the Icebox in ROADMAP.md — don't scope-creep the requirements.
- **Keep it scannable.** The requirements document will be referenced constantly during development. Use tables, consistent formatting, and clear headers.
