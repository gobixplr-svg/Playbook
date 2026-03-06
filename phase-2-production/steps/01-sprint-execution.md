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
| **Tests** | Unit tests (models, services), UI tests (flows) | All tests pass |

**Key rules:**
- Build the mock implementation first — test against it before touching real services
- Each layer should compile and pass tests before moving to the next
- Commit after each layer completes

### 3. Daily Rhythm

- Start: Check sprint tracker, pick next task
- Build: One layer at a time, test as you go
- End: Commit working code, update tracker, note blockers

### 4. Integration Buffer (Final 1-2 Days)

- Run end-to-end tests across all sprint stories
- Verify the demo scenario works on a real device
- Fix integration issues surfaced by combining features
- Update ROADMAP.md with completed stories

### 5. Sprint Review

- Demo the sprint deliverable (run the demo scenario)
- Note what worked and what didn't
- Update sprint tracker with final status
- Identify carryover items for next sprint

## Anti-Patterns

- **Big-bang integration** — Don't build all stories in isolation then wire together at the end
- **Skipping mocks** — Real services hide bugs behind network flakiness. Test with mocks first.
- **Gold-plating** — If it's not in the sprint scope, write it down for later. Don't sneak in extras.
- **Broken builds** — Never leave the project in a state that doesn't compile. Every commit builds.

## Exit Criteria

Sprint is complete when:
- [ ] All sprint stories are done (or explicitly deferred with reason)
- [ ] Demo scenario runs successfully
- [ ] All tests pass
- [ ] Code is committed with clear messages
- [ ] ROADMAP.md is updated
- [ ] No known regressions from prior sprints

## Key Output

Completed [Sprint Tracker](../templates/sprint-tracker.md) with story status, notes, and carryover items.
