# Phase 1 Retrospective — [App Name]

> Playbook Reference: [Step 9: Phase Retrospective](../steps/09-phase-retrospective.md)

## Project

**App Name:** [Name]
**Date:** [YYYY-MM-DD]
**Author:** [Name]
**Phase Duration:** [Start date] — [End date]

---

## 1. Deliverables Review

### Step Deliverables

| Step | Deliverable | Output File | Complete |
|------|-------------|-------------|----------|
| 1 | Requirements Document | `docs/pre-production/requirements-document.md` | [ ] |
| 2 | Spike Reports | `docs/pre-production/spike-reports/` | [ ] |
| 3 | Architecture Document | `docs/pre-production/architecture-document.md` | [ ] |
| 4 | UI/UX Spec | `docs/pre-production/uiux-spec.md` | [ ] |
| 5 | Design System Spec | `docs/pre-production/design-system-spec.md` | [ ] |
| 6 | API & Data Spec | `docs/pre-production/api-spec.md` | [ ] |
| 7 | Project Plan | `docs/pre-production/project-plan.md` | [ ] |
| 8 | Dev Environment Setup | `docs/pre-production/dev-environment-setup.md` | [ ] |

### Supporting Artifacts

| Artifact | Location | Complete |
|----------|----------|----------|
| Project instructions (CLAUDE.md) | `.claude/CLAUDE.md` | [ ] |
| Coding rules | `.claude/rules/` | [ ] |
| AI memory seed | `.claude/memory/MEMORY.md` | [ ] |
| Session hooks | `.claude/hooks/` | [ ] |
| Task tracking | `docs/ROADMAP.md` | [ ] |

---

## 2. Process Evaluation

### What went well?

- _[Which steps produced the most useful output?]_
- _[Where did the workflow flow naturally?]_
- _[What decisions were you most confident in?]_

### What was difficult?

- _[Which steps took the longest or needed the most iteration?]_
- _[Where did scope drift occur?]_
- _[What was underestimated?]_

### What was missing?

- _[Gaps the process didn't cover?]_
- _[Deliverables you wished existed earlier?]_
- _[Context lost between sessions?]_

### What would you change?

- _[Steps to reorder, combine, or add?]_
- _[Templates to expand or simplify?]_
- _[Workflow improvements?]_

### Spec Consistency Check

- [ ] Same identifiers used across all specs (story IDs, model names, service names)
- [ ] Architecture models match API schema tables
- [ ] Design system tokens referenced in UI/UX spec
- [ ] Project plan stories match requirements document stories

---

## 3. Phase 2 Readiness

### Phase 1 Exit Criteria

- [ ] Requirements formalized with acceptance criteria and TDD test plans
- [ ] Technical spikes completed for all unknowns
- [ ] Architecture documented with testability patterns
- [ ] UI/UX designs exist for key screens and user flows
- [ ] Design system established with reusable components and SwiftUI specs
- [ ] API contracts and database schema designed
- [ ] Project plan with milestones and sprints exists
- [ ] Dev environment configured with testing infrastructure
- [ ] Conditional Go items from Phase 0 resolved
- [ ] AI workspace configured and verified

### Sprint 1 Readiness

- [ ] Walking skeleton stories identified (list IDs: ___________)
- [ ] Sprint 1 stories have task breakdowns with estimates
- [ ] Sprint 1 dependencies are all resolved (no blockers from outside Sprint 1)
- [ ] Dev environment passes validation checklist (Step 8, Section 8.8)

---

## 4. Process Improvements

### Add to Playbook

- _[Specific addition to a step guide or template]_

### Change in Process

- _[Reorder, combine, or modify existing steps]_

### New Template/Reference

- _[Something that should exist but doesn't]_

---

## 5. Phase 2 Continuity Package

| Artifact | Status | Notes |
|----------|--------|-------|
| MEMORY.md updated for Phase 2 | [ ] | _Current Phase → Phase 2, Sprint 1 details added_ |
| ROADMAP.md transitioned | [ ] | _Phase 1 → Done, Phase 2 → Active_ |
| Sprint 1 kickoff brief (below) | [ ] | _Theme, stories, first task, dependencies_ |

### Sprint 1 Kickoff Brief

**Sprint theme:** _[Theme name from project plan]_
**Sprint goal:** _[What can the user do after this sprint?]_

**Stories:**
- _[Story ID] — [Title]_

**First task to pick up:** _[The specific task to start with]_

**Key dependencies:** _[What must be set up or available before Sprint 1 work begins]_

**Done when:** _[Demo scenario — how you'd show Sprint 1 is complete]_

---

## 6. Close Phase 1

- [ ] All deliverables committed to version control
- [ ] ROADMAP.md updated (Phase 1 → Done, Phase 2 → Active)
- [ ] No unresolved Conditional Go items
- [ ] Phase 2 continuity package complete
- [ ] Dev environment validation passes

---

## Status

- [ ] Deliverables reviewed (9.1)
- [ ] Process evaluated (9.2)
- [ ] Phase 2 readiness validated (9.3)
- [ ] Process improvements documented (9.4)
- [ ] Continuity package created (9.5)
- [ ] Phase 1 closed (9.6)
