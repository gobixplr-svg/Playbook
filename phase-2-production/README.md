# Phase 2: Production

## Purpose

Build the app. Execute the plan from Pre-Production in structured sprints, maintaining working software at every step. This phase answers: **Is it built and does it work?**

## When to Enter

- Phase 1 Pre-Production is complete
- Architecture, design system, and project plan are finalized
- Dev environment is set up and building

## Steps

| Step | Name | Purpose | Key Output |
|------|------|---------|------------|
| 1 | [Sprint Execution](steps/01-sprint-execution.md) | Run a structured sprint from kickoff to review (repeatable) | [Sprint Tracker](templates/sprint-tracker.md) |

**Note:** Phase 2 uses a single repeatable step rather than sequential steps. Each sprint follows the same Sprint Execution process. The project plan (from Phase 1, Step 7) defines sprint scope and sequencing.

## Exit Criteria

Phase 2 is complete when:
- [ ] All "Must Have" features are implemented and functional
- [ ] "Should Have" features are implemented or explicitly deferred
- [ ] App builds and runs on all target platforms
- [ ] End-to-end user flows work with real data
- [ ] No known crashes or data loss bugs
- [ ] Code is committed, branched, and ready for QA

## Key Principles

- **Always shippable.** Every sprint ends with working software. If you can't ship it, the sprint isn't done.
- **Vertical slices.** Build features end-to-end (UI + logic + data) rather than building all UI, then all logic, then all data.
- **Commit often.** Small, frequent commits with clear messages. Every commit should build.
- **Track progress visibly.** Update the roadmap and progress docs after each sprint.

## Estimated Time

- **Solo dev, simple app:** 2-4 weeks
- **Solo dev, complex app:** 1-3 months
- **Team, complex app:** 2-6 months
