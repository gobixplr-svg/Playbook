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
|-------|---------------|-----------|
| **Data** | Models (`Codable` structs), database migrations | Model compiles, JSON round-trips |
| **Service** | Protocol definition, mock implementation, real implementation | Protocol defined, mock passes tests |
| **View** | SwiftUI views bound to ViewModel | View renders with mock data |
| **Integration** | Wire real services, navigation, state | Feature works end-to-end |
| **Tests** | Unit tests (models, services), UI tests (flows) | Tests written and reviewed |

**Key rules:**
- Build the mock implementation first — test against it before touching real services
- Each layer should compile before moving to the next
- Commit after each layer completes

### 3. Testing Cadence

Use a **3-tier testing approach** to balance confidence with development speed. Full test suite runs are expensive (simulator clone setup, 2+ minutes per cycle) and should only happen at natural checkpoints — not after every file.

#### Tier 1: Build Check (during development)

- **When:** After every file or small feature change
- **What:** `xcodebuild build` — compilation only, no simulator, no tests
- **Time:** ~15-30 seconds
- **Purpose:** Catch type errors, missing imports, API misuse immediately

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
- Verify the demo scenario works on a real device
- Fix integration issues surfaced by combining features
- Update ROADMAP.md with completed stories

### 6. Sprint Review

- Demo the sprint deliverable (run the demo scenario)
- Note what worked and what didn't
- Update sprint tracker with final status
- Identify carryover items for next sprint

## Anti-Patterns

- **Big-bang integration** — Don't build all stories in isolation then wire together at the end
- **Skipping mocks** — Real services hide bugs behind network flakiness. Test with mocks first.
- **Gold-plating** — If it's not in the sprint scope, write it down for later. Don't sneak in extras.
- **Broken builds** — Never leave the project in a state that doesn't compile. Every commit builds.
- **Over-testing cadence** — Don't run full simulator test suites after every story. Use Tier 1 build checks during development, Tier 3 at sprint boundaries.

## Exit Criteria

Sprint is complete when:
- [ ] All sprint stories are done (or explicitly deferred with reason)
- [ ] Demo scenario runs successfully
- [ ] Full test suite passes (Tier 3)
- [ ] Code is committed with clear messages
- [ ] ROADMAP.md is updated
- [ ] No known regressions from prior sprints

## Key Output

Completed [Sprint Tracker](../templates/sprint-tracker.md) with story status, notes, and carryover items.
