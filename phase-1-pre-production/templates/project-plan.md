# Project Plan — [App Name]

> Playbook Reference: [Step 7: Project Plan](../steps/07-project-plan.md)

## Project

**App Name:** [Name]
**Date:** [YYYY-MM-DD]
**Author:** [Name]
**Total Stories:** [Number]
**Sprint Duration:** [e.g., 2–3 weeks]

---

### Sprint Duration Guidance

| Context | Recommended Duration | Notes |
|---------|---------------------|-------|
| Solo dev, learning the stack | 3 weeks | Extra time for discovery and debugging |
| Solo dev, experienced stack | 2 weeks | Tight but achievable with clear specs |
| Small team (2–3 devs) | 1–2 weeks | Shorter sprints, more frequent integration |

---

## Dependency Graph

### Hard Blockers

| Story | Blocks | Reason |
|-------|--------|--------|
| | | |

### Soft Dependencies

| Story | Enhances | Notes |
|-------|----------|-------|
| | | |

### Independent Stories

- [Story ID] — [Title]

---

## Walking Skeleton

**Core loop:** [Describe the end-to-end user experience this achieves]

| Order | Story ID | Story Title | Why It's in the Skeleton |
|-------|----------|-------------|--------------------------|
| 1 | | | |
| 2 | | | |

**Demo scenario:** [How you'd show this working to someone in 60 seconds]

---

## Sprint Plan

### Sprint 1: [Theme Name]

**Theme:** [What capability does this sprint deliver?]
**Deliverable:** [What can the user do after this sprint?]
**Demo scenario:** [How you'd show this sprint's outcome to someone in 60 seconds]
**Dependencies resolved:** [What does completing this unlock for future sprints?]

**Stories:**

#### [Story ID] — [Story Title]

**Acceptance criteria reference:** [Link to requirements doc section]

_Tasks (TDD order: test → implement → verify):_

- [ ] **Test:** [Write failing test for core behavior]
      Layer: Tests
      Estimate: S
- [ ] **Implement:** [Build the feature to pass the test]
      Layer: [Data / Service / View / Integration]
      Estimate: M
- [ ] **Verify:** [Run full test suite, check integration with existing features]
      Layer: Integration
      Estimate: S

_Additional tasks:_

- [ ] [Task description]
      Layer: [Data / Service / View / Integration / Tests]
      Estimate: [S / M / L]

**Story done when:** [Specific completion criteria — not just "tests pass" but what the user can do]

---

_Repeat for each story in the sprint..._

---

### Sprint Integration Buffer

_Reserve 1–2 days at the end of each sprint for:_
- [ ] Integration testing across all sprint stories
- [ ] Bug fixes discovered during integration
- [ ] Update documentation and progress tracking
- [ ] Demo to stakeholders (or self-demo against milestone criteria)

---

_Repeat Sprint structure for each sprint..._

---

## Task Size Reference

| Size | Duration | Examples |
|------|----------|---------|
| **S** | < half day | Add a model property, wire a button to existing logic, write a simple unit test |
| **M** | Half day to full day | Implement a service method, build a view with state management, write integration tests |
| **L** | 1–2 days | Integrate a third-party SDK, build a complex multi-state view, implement bidirectional sync |

If a task is larger than **L**, break it into smaller tasks. No single task should span more than 2 days — if it does, you'll lose track of progress and debugging becomes harder.

---

## Milestones

| Milestone | Target Sprint | Key Stories | Demo Scenario |
|-----------|---------------|-------------|---------------|
| | | | |

---

## Risk Register

| Risk | Category | Likelihood | Impact | Mitigation |
|------|----------|------------|--------|------------|
| | Technical / Scope / External / Personal | Low / Med / High | Low / Med / High | |

---

## Coverage Verification

- [ ] Every Must Have story is assigned to a sprint
- [ ] Every Should Have story is assigned to a sprint
- [ ] Dependencies are respected (no story before its blocker)
- [ ] Walking skeleton completes within first 1–2 sprints
- [ ] Each sprint has a demo-able deliverable
- [ ] Milestones mark real capability shifts
- [ ] Risks identified with mitigations
- [ ] Test plan accounted for in task breakdowns

---

## Status

- [ ] Dependency graph mapped
- [ ] Walking skeleton identified
- [ ] Stories assigned to sprints
- [ ] Tasks broken down with estimates
- [ ] Milestones defined
- [ ] Risks documented
- [ ] Coverage verified
