# Phase 1: Pre-Production

## Purpose

Transform the approved Concept Document into a buildable plan. This phase answers: **How are we building this, and in what order?**

## When to Enter

- Phase 0 Concept Document is complete with a "Go" or "Conditional Go" decision
- Core concept, audience, and feature scope are defined

## Steps

| Step | Name | Purpose | Key Output |
|------|------|---------|------------|
| 1 | [Requirements Specification](steps/01-requirements-specification.md) | Formalize features into user stories, acceptance criteria, and TDD test plans | Requirements Document |
| 2 | [Technical Spikes](steps/02-technical-spikes.md) | Time-boxed prototypes to validate unknowns before committing | Spike reports + recommendations |
| 3 | [Architecture Design](steps/03-architecture-design.md) | System architecture, data models, testability patterns | Architecture Document |
| 4 | [UI/UX Design](steps/04-uiux-design.md) | Visual direction, wireframes, screen designs, user flow prototypes | UI/UX Spec + Figma designs |
| 5 | [Design System](steps/05-design-system.md) | Design tokens, component library, SwiftUI component specs | Design System Spec |
| 6 | [API & Data Design](steps/06-api-data-design.md) | Database schema, API contracts, data flows | API Spec + Schema |
| 7 | Project Plan | Milestones, sprints, task lists | Project Plan |
| 8 | Dev Environment Setup | Repos, CI/CD, testing infrastructure, tooling | Working dev environment |
| 9 | Phase Retrospective | Capture process learnings, create Phase 2 continuity | Retrospective + continuity artifacts |

### How to Start

Begin with **Step 1 (Requirements Specification)**. It formalizes the MoSCoW feature list from the Concept Document into detailed user stories with testable acceptance criteria. Every subsequent step builds on this foundation.

Steps are designed to flow linearly — each step's output feeds the next. However, Steps 4-5 (UI/UX Design → Design System) and Step 6 (API & Data Design) can proceed in parallel once Architecture (Step 3) is complete.

## Cross-Cutting Principles

### Test-Driven Development (TDD)

TDD is not a single step — it's embedded throughout:

- **Step 1 (Requirements):** Acceptance criteria are written in Given/When/Then format. These ARE the test specifications. Every user story ships with a test plan before any code is written.
- **Step 3 (Architecture):** Testability is a first-class architectural concern. Protocol-oriented design enables mocking. Dependency injection enables isolation. Clean boundaries enable unit testing.
- **Step 8 (Dev Environment):** Testing infrastructure is set up from day one — XCTest targets, UI test targets, CI pipeline that runs tests on every commit.
- **Phase 2 (Production):** Every feature is built test-first. Write the failing test, then write the code to make it pass.

### Design for the MVP, Not the Vision

Architecture should support v1 features cleanly. Future features should be possible but not pre-built.

### Spike the Unknowns

Any "Complex" or "Unknown" items from the feasibility assessment get a time-boxed spike (Step 2) before committing to architecture.

### Set Up for Speed

The dev environment, CI/CD, and tooling should make Phase 2 (Production) as frictionless as possible.

## AI-Powered UI/UX Tooling

Steps 4 and 5 use an AI-assisted design pipeline. All tools are free or near-free:

| Phase | Tool | Cost | Purpose |
|-------|------|------|---------|
| Visual direction | Midjourney | $10 one-time | Mood boards, color exploration, atmosphere |
| Color system | Khroma + Coolors | Free | AI color palette generation, accessibility checking |
| Screen generation | Google Stitch | Free | Text-to-UI design, multi-screen flows, Figma export |
| Design refinement | Figma (Starter) | Free | Polish designs, build components, prototype flows |
| Design-to-code | Codia AI + Figma MCP | Free | Figma → SwiftUI code bridge, Claude Code integration |

**Pipeline:** Midjourney (mood) → Stitch (screens) → Figma (refine) → Codia/MCP (SwiftUI)

## Exit Criteria

Phase 1 is complete when:
- [ ] Requirements are formalized with acceptance criteria and TDD test plans
- [ ] Technical spikes are completed for all "Complex" or "Unknown" items
- [ ] Architecture is documented with testability patterns
- [ ] UI/UX designs exist for all key screens and user flows
- [ ] Design system is established with reusable components and SwiftUI specs
- [ ] API contracts and database schema are designed
- [ ] Project plan with milestones and sprints exists
- [ ] Dev environment is configured with testing infrastructure and a "hello world" builds
- [ ] Conditional Go items from Phase 0 are resolved
- [ ] Phase retrospective completed with Phase 2 continuity artifacts

## Deliverables

### Process (Playbook)
- Step guides with instructions for each activity
- Blank templates ready to copy into any project

### Project-Specific
Copy templates into your project at `docs/pre-production/`:
```
your-project/
├── docs/
│   ├── concept/                    # Phase 0 deliverables (already exist)
│   ├── pre-production/
│   │   ├── requirements-document.md
│   │   ├── spike-reports/
│   │   ├── architecture-document.md
│   │   ├── uiux-spec.md
│   │   ├── design-system-spec.md
│   │   ├── api-spec.md
│   │   └── project-plan.md
│   └── ROADMAP.md
```

## Estimated Time

- **Solo dev, simple app:** 3-5 days
- **Solo dev, complex app:** 1-2 weeks
- **Team, complex app:** 2-4 weeks

## Common Pitfalls

- **Skipping requirements formalization** — "I know what I'm building" leads to scope drift. The concept document captures what, but user stories capture how users experience it.
- **Spiking too long** — Spikes are time-boxed experiments, not prototypes. A spike that takes more than 2-3 days has become a project.
- **Designing the architecture for the vision** — Design for v1's 12 Must Haves. The architecture should accommodate future features but not implement them.
- **Skipping the design phase** — "I'll figure out the UI as I code" produces inconsistent, unpolished apps. Investing in UI/UX design before production saves significant rework.
- **Setting up dev environment last** — CI/CD and testing infrastructure should be ready before writing production code. Retrofitting is painful.
