# Step 1: Sprint Execution

## Purpose

Run a structured sprint from kickoff to review. This is a repeatable cycle — use it for every sprint in Phase 2.

## When to Use

At the start of each sprint, after Pre-Production is complete and the project plan defines sprint scope.

## Inputs

- Project plan with sprint stories, tasks, and dependencies
- Architecture document with data models, service protocols, and module structure
- API spec with database schema, RLS policies, and API contracts

## Process

### 0. Design System Bootstrap (First Sprint Only)

Before writing any feature code, validate that your design system actually works in-app:

1. Implement `DesignTokens.swift` (or equivalent) with **actual color values** — hex codes via `UIColor`, not named asset catalog references unless the assets are also created and verified
2. Use a **deterministic mock user ID** (`static let mockUserId`) shared across all mock services. Random `UUID()` in auth/health/route services causes cross-service mismatches that silently break user flows
3. Build a throwaway `#if DEBUG` token gallery view that renders every color, font, and spacing token on screen
4. Run in simulator, screenshot, compare to design spec — fix discrepancies before any feature uses the tokens
5. Delete or gate the gallery view behind `#if DEBUG`

**Why:** Design tokens defined in a spec but never validated in-app cause cascading invisible bugs. In RoamAbout, `Color("TrailTeal")` referenced asset catalog entries that never existed — every branded element was transparent. Three sprints of features built on top were technically correct but visually broken. 30 minutes of bootstrap prevents this entire class of failure.

### 1. Sprint Setup (Day 1)

1. Copy the [Sprint Tracker](../templates/sprint-tracker.md) template into your project
2. Fill in the sprint metadata (number, theme, goal, stories)
3. Verify dependencies from prior sprints are met
4. Identify any blockers that need resolution before coding starts

### 2. Story Execution (Per Story)

Work each story in **vertical slices** through the architecture layers:

```
Data → Service → View → Integration → Tests
```

For each layer:

| Layer | What You Build | Done When |
| ------- | --------------- | ----------- |
| **Data** | Models (`Codable` structs), database migrations | Model compiles, JSON round-trips |
| **Service** | Protocol definition, mock implementation, real implementation | Protocol defined, mock passes tests |
| **View** | SwiftUI views bound to ViewModel | View renders with mock data, **visually verified in simulator** |
| **Integration** | Wire real services, navigation, state | Feature works end-to-end via user flow walkthrough |
| **Tests** | Unit tests (models, services), UI tests (flows) | Tests written and reviewed |

**Key rules:**
- Build the mock implementation first — test against it before touching real services
- Each layer should compile before moving to the next
- Commit after each layer completes
- **Every View must be visually verified in the simulator** before marking the layer done (see Tier 1.5 below)

**Mock service requirements:**
- Use a **shared deterministic user ID** across all mock services (auth, routes, health, achievements). Random `UUID()` per service causes silent cross-service mismatches.
- Provide a `.withDemoData()` or `.withSeedData()` factory that includes **realistic starting state** — active routes with progress, earned badges, activity history. This enables end-to-end flow validation without real backends.
- Auto-restore sessions so developers don't have to sign in after every Command+R. Mock auth should simulate a persisted session.

### 3. Testing Cadence

Use a **3-tier testing approach** to balance confidence with development speed. Full test suite runs are expensive (simulator clone setup, 2+ minutes per cycle) and should only happen at natural checkpoints — not after every file.

#### Tier 1: Build Check (during development)

- **When:** After every file or small feature change
- **What:** `xcodebuild build` — compilation only, no simulator, no tests
- **Time:** ~15-30 seconds
- **Purpose:** Catch type errors, missing imports, API misuse immediately

#### Tier 1.5: Visual Smoke Test (after every View layer)

- **When:** After completing any View layer task
- **What:** Run the app in the simulator, navigate to the new/changed view, verify visually
- **Time:** ~30-60 seconds
- **Purpose:** Catch invisible elements, broken layouts, missing colors, wrong fonts — bugs that `xcodebuild build` cannot detect
- **Checklist:**
  - [ ] Can you see every text element? (check against both light and dark backgrounds)
  - [ ] Do colors match the design spec?
  - [ ] Are buttons tappable and visually distinct?
  - [ ] Does the layout look intentional, not clipped or overflowing?
  - [ ] Does navigation work (push, back, dismiss)?

**Why this exists:** A view can compile perfectly with transparent colors, white-on-white text, or navigation that goes nowhere. Build checks verify syntax. Visual smoke tests verify the user can actually see and use the feature.

#### Tier 2: Test Plan (end of each story)

- **When:** After completing a story's code + tests
- **What:** Present a written test summary listing new tests, what they cover, and expected behavior. Developer decides whether to run them in Xcode or defer to Tier 3.
- **Time:** No build cost; optional manual spot-check by developer
- **Purpose:** Track test coverage without blocking development flow
- **Format:**
  ```
  ## Test Plan — [Story ID]
  New tests: 8 | Modified: 0 | Total: N

  - ModelTests (3): defaultInit, jsonRoundTrip, snakeCaseJSON
  - ViewModelTests (5): fetchSuccess, handleError, stateTransition...

  Manual check (optional):
  - [ ] Tap [feature] in simulator — verify [expected behavior]
  ```

#### Tier 3: Full Test Suite (sprint milestones)

- **When:** End of sprint, before major integration points, or when developer requests it
- **What:** `xcodebuild test` — full suite on simulator with all assertions
- **Time:** ~2+ minutes
- **Purpose:** Catch regressions, verify cross-story integration, gate sprint completion

**Anti-pattern:** Running Tier 3 after every story. This adds 10-20 minutes of idle time per sprint for no additional signal beyond what Tier 1 + Tier 2 already provide.

### 4. Daily Rhythm

- Start: Check sprint tracker, pick next task
- Build: One layer at a time, Tier 1 build checks as you go
- End: Commit working code, update tracker, note blockers

### 5. Integration Buffer (Final 1-2 Days)

- Run full test suite (Tier 3) across all sprint stories
- **User Flow Walkthrough** — walk through the app as a real user, not a developer:
  1. Cold start (Command+R) — does the app land in the right state?
  2. Primary flow: new user → discover feature → complete core loop
  3. Returning user: restart → resume where they left off (requires mock seed data)
  4. Every navigation path: forward, back, edge cases (double-tap, swipe back mid-action)
  5. Both light and dark mode
  6. Screenshot each screen, compare to mockups/design spec
- Fix integration issues surfaced by combining features
- Update ROADMAP.md with completed stories

#### First Full Playthrough (Convergence Sprints)

If this is the first sprint where independently-built systems must work together as a product (a "convergence sprint" — see [Project Plan, Section 7.3](../../phase-1-pre-production/steps/07-project-plan.md)), schedule a **First Full Playthrough** early in the sprint — before polish work begins.

**Process:**
1. Play through the complete user flow end-to-end (not debugging, _playing_)
2. Log every issue into three buckets:
   - **Broken:** Something doesn't work at all (dead code paths, missing wiring, crashes)
   - **Wrong:** Something works but the behavior is incorrect (balance issues, wrong defaults, confusing UX)
   - **Missing:** Something the design assumed would exist but doesn't (transitions, feedback, error states)
3. Prioritize: fix Broken first, Wrong second, Missing third
4. Re-plan the sprint with discovered work included

**Why this exists:** Systems tested in isolation pass their own tests but fail together. In Fizzics, the first full playthrough revealed: overflow detection was dead code (never called), the Game Over panel had an inactive-GameObject listener bug, 11 tests were silently broken, and the difficulty curve was fundamentally too shallow (4 merge tiers instead of 6). All of these were invisible during isolated system testing. One playthrough session surfaced them all.

**Budget:** The first full playthrough typically generates 1-2 sessions of discovered work. Plan for this. If the sprint scope doesn't have room, defer the lowest-priority planned story to make space.

### 6. Code Review (End of Sprint)

Before the sprint review, run a structured code review on all code written during the sprint. This catches bugs, security issues, and architectural drift that testing alone misses.

**When using AI coding tools (Claude Code, Cursor, etc.):**

AI-generated code is especially prone to subtle issues — it compiles, passes tests, and looks correct, but may contain security flaws, performance problems, or patterns that diverge from your architecture. Code review is the quality gate.

**Review process:**

1. **Gather the diff:** `git diff <sprint-start-commit>..HEAD` to see everything that changed this sprint
2. **Run a structured code review** on the sprint diff — review it for security, performance, correctness, and maintainability (use an AI coding assistant, a teammate, or self-review against the dimensions below)
3. **Review each dimension:**

| Dimension | What to Check | Common AI-Generated Issues |
| --------- | ------------- | -------------------------- |
| **Security** | Input validation, auth checks, data exposure, secrets in code | Hardcoded API keys, missing server-side validation, overly permissive RLS policies |
| **Performance** | N+1 queries, unbounded loops, missing indexes, memory leaks | Unnecessary re-renders, fetching entire tables instead of filtered queries |
| **Correctness** | Edge cases, error handling, race conditions, off-by-one | Optimistic assumptions about optional values, missing error propagation |
| **Maintainability** | Naming clarity, single responsibility, duplication, test coverage | Generated code that's technically correct but doesn't follow your architecture patterns |
| **Architecture** | Does the code follow the patterns defined in the architecture doc? | Bypassing the service layer, direct database calls from views, inconsistent dependency injection |

1. **Fix critical and high issues** before sprint review. Log medium/low issues as tech debt for future sprints.
2. **Update the sprint tracker** with review findings and actions taken.

**Tip:** If working solo, use an AI coding assistant as your pair reviewer — ask it to review the diff against the dimensions above. If working with others, combine AI review with human review — they catch different things.

### 7. Sprint Review

- Demo the sprint deliverable (run the demo scenario)
- Update sprint tracker with final status
- Identify carryover items for next sprint
- **Run a [Retrospective](02-retrospective.md)** — capture what worked, what didn't, and apply changes to the playbook

### 8. Backend Integration (When Transitioning Mock → Real)

When replacing mock services with real backend implementations, follow this checklist in addition to normal story execution. The mock-first pattern makes the swap easy architecturally, but these process steps prevent common integration bugs.

#### Pre-flight
- [ ] **SDK API verification** — Read the SDK docs or source for the specific methods you'll call. Don't assume method signatures match your expectations. (e.g., supabase-swift `signUp` returns non-optional `User`, not `User?`)
- [ ] **Service coupling map** — Document which real services depend on each other. Enable dependent services together. (e.g., RLS-protected routes require real auth — you can't test real routes with mock auth)
- [ ] **Infrastructure pre-flight** — Verify hosting costs, account limits, and self-hosting fallback (see Phase 1, Step 8, Section 8.4)

#### Migration validation
- [ ] **Schema test** — Run `supabase db reset` (or equivalent) and verify all migrations execute without errors. Watch for forward references (functions/types used before they're defined)
- [ ] **Seed data test** — Verify seed data matches mock service data (same UUIDs, same field values) for transparent transition
- [ ] **RLS smoke test** — With a real authenticated session, verify CRUD operations against every table's policies

#### Incremental flag testing
- [ ] Enable **one service at a time** (e.g., auth first, then routes, then achievements)
- [ ] Build and run after each flag change
- [ ] Verify the enabled service works with both mock and real versions of its dependencies
- [ ] Document any coupling requirements discovered (add to service coupling map)

#### Session scoping
- [ ] If the integration plan has **5+ phases**, split across multiple sessions with explicit handoff documents
- [ ] Each session should end with working code and updated progress tracking
- [ ] Write a continuation note at the end of each session describing: what's done, what's next, and any gotchas discovered

## Anti-Patterns

- **Big-bang integration** — Don't build all stories in isolation then wire together at the end
- **Skipping mocks** — Real services hide bugs behind network flakiness. Test with mocks first.
- **Gold-plating** — If it's not in the sprint scope, write it down for later. Don't sneak in extras.
- **Convergence blindness** — Planning a convergence sprint (systems integrating for the first time) as if it's a construction sprint (building new systems in isolation). Convergence always generates discovered work — dead code paths, design gaps, balance issues, iteration cycles. Budget 40% of the sprint for emergent tasks, or defer planned stories to make room. The sprint after the walking skeleton is functional is almost always convergence.
- **Skipping the first playthrough** — Testing systems in isolation but never running the full user flow end-to-end. Individual systems pass their own tests but fail together. One full playthrough session early in the sprint surfaces more integration bugs than a week of unit testing.
- **Broken builds** — Never leave the project in a state that doesn't compile. Every commit builds.
- **Over-testing cadence** — Don't run full simulator test suites after every story. Use Tier 1 build checks during development, Tier 3 at sprint boundaries.
- **Test-only validation** — `xcodebuild build` and `xcodebuild test` cannot verify that a user can see a button, that colors render correctly, or that navigation flows work end-to-end. Every view must be visually verified in the simulator at least once before marking a story complete. Tests verify logic in isolation; visual smoke tests verify the user experience.
- **Phantom design tokens** — Referencing named colors (e.g., `Color("BrandTeal")`) or assets that don't exist in the asset catalog. The code compiles, views render, but elements are transparent/invisible. Always validate design tokens visually before building features on top.
- **Random mock identifiers** — Using `UUID()` in mock services creates a different identity on every launch. Cross-service operations (auth → routes → health) silently fail because IDs don't match. Use deterministic, shared mock IDs.
- **SQL forward references** — Defining functions, types, or views after the RLS policies or queries that reference them. SQL migrations execute top-to-bottom — a forward reference causes `function does not exist` errors. Always define dependencies before dependents.
- **SDK API assumptions** — Writing code against assumed SDK method signatures without checking docs or source. Method return types, optionality, and parameter names vary between SDK versions. Read the actual API before writing service code.
- **Unchecked service coupling** — Enabling one real service without verifying its dependencies on other services. RLS policies require authenticated sessions — real routes with mock auth silently return empty results. Map service dependencies before toggling flags.

## Exit Criteria

Sprint is complete when:
- [ ] All sprint stories are done (or explicitly deferred with reason)
- [ ] Demo scenario runs successfully
- [ ] Full test suite passes (Tier 3)
- [ ] **User flow walkthrough completed** — cold start, primary flow, returning user, light + dark mode
- [ ] **Visual smoke test passed** — every new/changed view verified in simulator against design spec
- [ ] **Code review completed** — security, performance, correctness, maintainability, architecture reviewed; critical issues fixed
- [ ] Code is committed with clear messages
- [ ] ROADMAP.md is updated
- [ ] **[Retrospective](02-retrospective.md) completed** — lessons captured, changes applied to playbook
- [ ] No known regressions from prior sprints

## Key Output

Completed [Sprint Tracker](../templates/sprint-tracker.md) with story status, notes, and carryover items.
