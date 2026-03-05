# Phase 0: Concept

## Purpose

Transform a raw idea into a validated concept with enough clarity to commit resources. This phase answers: **What are we building, who is it for, and is it worth pursuing?**

## When to Enter

- You have an app idea (even a vague one)
- You want to evaluate whether an idea is viable before committing to building it

## Steps

| Step | Name | Purpose | Key Output |
|------|------|---------|------------|
| 0 | [Guided Concept Session](steps/00-guided-concept-session.md) | Interactive Q&A that drives Steps 1-5 conversationally | Draft deliverables for all steps |
| 1 | [Ideation & Vision](steps/01-ideation-and-vision.md) | Define the core idea and who it serves | Vision Statement |
| 2 | [Discovery Questions](steps/02-discovery-questions.md) | Surface unknowns and scope decisions | Completed Questionnaire |
| 3 | [Competitive Analysis](steps/03-competitive-analysis.md) | Understand the market and differentiation | Competitor Matrix + Positioning |
| 4 | [Technical Feasibility](steps/04-technical-feasibility.md) | Validate that the idea is buildable | Feasibility Assessment |
| 5 | [Concept Document](steps/05-concept-document.md) | Synthesize everything into a Go/No-Go decision | Concept Document |
| 6 | [Phase Retrospective](steps/06-phase-retrospective.md) | Capture process learnings and set up Phase 1 | Retrospective notes, continuity artifacts |

### How to Start

**Recommended:** Begin with **Step 0 (Guided Concept Session)**. It runs an iterative Q&A conversation that produces draft deliverables for Steps 1-4 and feeds into Step 5. This is faster and more thorough than working through templates in isolation.

**Alternative:** Work through Steps 1-5 independently using the templates in `templates/`. Better suited for teams who prefer async documentation or already have a well-formed concept.

## Exit Criteria

Phase 0 is complete when:
- [ ] Vision statement clearly articulates the problem, audience, and solution
- [ ] All discovery questions are answered (or explicitly deferred with rationale)
- [ ] Competitive landscape is mapped with clear differentiation strategy
- [ ] Technical feasibility is assessed with no unresolved blockers
- [ ] Concept Document is written and approved
- [ ] Go/No-Go decision is made
- [ ] Concept Brief created (one-page summary for fast context)
- [ ] Session continuity artifacts created (CLAUDE.md, ROADMAP.md)
- [ ] Phase retrospective completed

## Deliverables

### Process (Playbook)
- Step guides with instructions for each activity
- Blank templates ready to copy into any project

### Project-Specific
Copy templates into your project at `docs/concept/`:
```
your-project/
├── .claude/
│   └── CLAUDE.md                  # AI context file — key decisions, tech stack, current phase
├── docs/
│   ├── concept/
│   │   ├── BRIEF.md               # One-page concept summary (read first)
│   │   ├── vision-statement.md
│   │   ├── discovery-questionnaire.md
│   │   ├── competitor-matrix.md
│   │   ├── technical-feasibility.md
│   │   └── concept-document.md
│   └── ROADMAP.md                 # Task tracking (initialized for Phase 1)
```

## Time Investment

- **Solo dev, simple app:** 1-2 days
- **Solo dev, complex app:** 3-5 days
- **Team, complex app:** 1-2 weeks

Don't rush this phase. Decisions made here compound through every future phase. An hour spent here saves days in production.

## Common Pitfalls

- **Skipping competitive analysis** — You think your idea is unique. It probably isn't, and that's fine. Understanding the market is how you differentiate.
- **Scope creep during ideation** — The concept phase defines what v1 is. Everything else goes on the "future" list.
- **Analysis paralysis on tech stack** — You don't need to finalize architecture here. You need to confirm the idea is *technically possible*.
- **Falling in love with the idea** — The concept phase should be willing to kill an idea. That's its job. A strong "no-go" saves months of wasted effort.
