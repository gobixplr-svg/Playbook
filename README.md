# Development Playbook

A structured, repeatable development process for building apps from concept to post-launch. Each phase produces two deliverables: a **reusable process template** (lives here) and a **project-specific output** (lives in the project repo).

## Who This Is For

Solo developers and small teams who want a professional, documented development process without enterprise overhead. Every step is designed to be actionable — not bureaucratic.

## How It Works

### Phases

| Phase | Name | Purpose | Exit Criteria |
|-------|------|---------|---------------|
| 0 | [Concept](phase-0-concept/) | Define what you're building and why | Concept Document approved |
| 1 | [Pre-Production](phase-1-pre-production/) | Plan how you'll build it | Architecture, design system, and project plan complete |
| 2 | [Production](phase-2-production/) | Build it | Feature-complete, integrated codebase |
| 3 | [Testing & QA](phase-3-testing-qa/) | Prove it works | All acceptance criteria met, no critical bugs |
| 4 | [Launch](phase-4-launch/) | Ship it | Live in app stores and/or on the web |
| 5 | [Post-Launch](phase-5-post-launch/) | Grow and maintain it | Ongoing — metrics, iteration, support |

### Two Deliverables Per Step

Every step within a phase produces:

1. **Process Template** (`Playbook/phase-X/steps/` and `templates/`)
   - The "how" — reusable instructions and blank templates
   - Platform-agnostic, project-agnostic
   - Improves over time as you run more projects through it

2. **Project Output** (`YourProject/docs/phase-X/`)
   - The "what" — completed deliverables for a specific project
   - References the Playbook step it was created from
   - Lives in the project repo alongside the code

### Using This Playbook

1. **Starting a new project:** Begin at Phase 0, Step 1. Work through each step sequentially.
2. **Copy templates:** Each phase has a `templates/` directory with blank documents to copy into your project's `docs/` folder.
3. **Follow the steps:** Each step file in `steps/` explains what to do, why it matters, and what "done" looks like.
4. **Adapt as needed:** Not every project needs every step. Scale up or down based on complexity. Mark steps as "N/A" with a reason rather than silently skipping them.

### Integration with Claude Code

This Playbook is designed to work alongside Claude Code skills and CLAUDE.md configurations:

- **Skills** are created or updated when a phase reveals a repeatable capability (e.g., a competitive analysis workflow, a deployment checklist)
- **CLAUDE.md** project instructions reference the Playbook phase the project is currently in
- **Memory files** track which phase/step is active for session continuity

## Directory Structure

```
Playbook/
├── README.md                           # This file
├── phase-0-concept/
│   ├── README.md                       # Phase overview, entry/exit criteria
│   ├── steps/                          # Step-by-step process guides
│   │   ├── 01-ideation-and-vision.md
│   │   ├── 02-discovery-questions.md
│   │   ├── 03-competitive-analysis.md
│   │   ├── 04-technical-feasibility.md
│   │   └── 05-concept-document.md
│   └── templates/                      # Blank templates to copy per project
│       ├── vision-statement.md
│       ├── discovery-questionnaire.md
│       ├── competitor-matrix.md
│       ├── technical-feasibility.md
│       └── concept-document.md
├── phase-1-pre-production/
│   ├── README.md
│   ├── steps/
│   └── templates/
├── phase-2-production/
│   ├── README.md
│   ├── steps/
│   └── templates/
├── phase-3-testing-qa/
│   ├── README.md
│   ├── steps/
│   └── templates/
├── phase-4-launch/
│   ├── README.md
│   ├── steps/
│   └── templates/
└── phase-5-post-launch/
    ├── README.md
    ├── steps/
    └── templates/
```

## Principles

- **Always have a working deliverable** — Each step ends with something concrete, not just notes
- **Document decisions, not just outcomes** — Capture the "why" so future-you understands the reasoning
- **Smallest viable process** — Only add process steps that earn their keep. If a step doesn't produce value, cut it
- **Phase gates are checkpoints, not gates** — Review exit criteria honestly, but don't let process block momentum
- **Improve the Playbook as you go** — After each project, update templates and steps with lessons learned
