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
| 7 | [Project Plan](steps/07-project-plan.md) | Milestones, sprints, task lists | Project Plan |
| 8 | [Dev Environment Setup](steps/08-dev-environment-setup.md) | Repos, CI/CD, testing infrastructure, AI workspace | Working dev environment |
| 9 | [Phase Retrospective](steps/09-phase-retrospective.md) | Capture process learnings, create Phase 2 continuity | Retrospective + continuity artifacts |

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

## AI-Assisted Development Integration

The Playbook is designed to produce specs that serve as both human documentation and AI context. When using AI coding tools (Claude Code, Cursor, etc.), the pre-production deliverables become the AI's knowledge base for Phase 2 development.

### Session Handoff Protocol

At the start of each AI coding session, the AI needs context about where the project stands. Establish a standard briefing pattern:

1. **ROADMAP.md** — current phase, active sprint, what's done vs. pending
2. **Current sprint section** from the project plan — stories and tasks for this sprint
3. **MEMORY.md** (if using Claude Code) — key decisions, patterns, and conventions

At the end of each session, update these artifacts so the next session can pick up cleanly:
- Mark completed tasks in the project plan
- Update ROADMAP.md if stories moved between sections
- Save any new decisions or patterns to memory

### Spec-as-Context Design

The pre-production specs from Steps 1–7 serve as AI reference material during development. To maximize their effectiveness:

- **Use consistent identifiers** — Story IDs (E1-S1), component names, and service names should be identical across all specs. The AI can trace "E3-S2" from requirements → architecture → project plan → code.
- **Keep specs updated** — if a decision changes during development, update the relevant spec. Stale specs produce stale AI suggestions.
- **Reference specs in prompts** — instead of re-explaining architecture, point the AI to the spec: "Implement E2-S1 following the HealthKitService protocol in architecture-document.md"

### Skill Integration Points

If using Claude Code with custom skills, integrate them into the development workflow:

| Development Event | Skill / Action | Purpose |
|-------------------|---------------|---------|
| Completing a story's tasks | `/commit` | Review changes and create structured commit |
| End of sprint | `/wrapup` | Update docs, commit, generate continuation prompt |
| End of session | `/continue` | Generate prompt for next session to resume |
| After significant changes | `/docs` | Keep documentation current |

### AI Workspace Setup (Step 8)

Step 8 (Dev Environment Setup) should include configuring the AI workspace:

- **`.claude/CLAUDE.md`** — Project-specific instructions, conventions, and guardrails. Seed from architecture and design system decisions.
- **`.claude/memory/MEMORY.md`** — Key decisions from Steps 1–7 that the AI should remember across sessions. Include: tech stack, architecture pattern, naming conventions, testing approach, design tokens.
- **Skill configurations** — Custom skills for project-specific workflows (if applicable)

The goal is that a new AI session can read CLAUDE.md + MEMORY.md + ROADMAP.md and understand the project well enough to start coding immediately.

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
│   │   ├── project-plan.md
│   │   └── dev-environment-setup.md
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
