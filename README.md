# Development Playbook

A structured, repeatable development process for building apps from concept to post-launch. Each phase produces two deliverables: a **reusable process template** (lives here) and a **project-specific output** (lives in the project repo).

## See It in Action

Visit [thedevplaybook.com](https://thedevplaybook.com) to see 9 real projects built with this Playbook, documented from concept to launch. The dev journal shows every phase working on real code, with real decisions, pivots, and mistakes included.

## Who This Is For

Solo developers and small teams who want a professional, documented development process without enterprise overhead. Every step is designed to be actionable — not bureaucratic.

## How It Works

### Phases

| Phase | Name | Purpose | Exit Criteria |
| ----- | ---- | ------- | ------------- |
| 0 | [Concept](phase-0-concept/) | Define what you're building and why | Concept Document approved |
| 1 | [Pre-Production](phase-1-pre-production/) | Plan how you'll build it | Architecture, design system, and project plan complete |
| 2 | [Production](phase-2-production/) | Build it | Feature-complete, integrated codebase |
| 3 | [Testing & QA](phase-3-testing-qa/) | Prove it works | All acceptance criteria met, no critical bugs |
| 4 | [Launch](phase-4-launch/) | Ship it | Live in app stores and/or on the web |
| 5 | [Post-Launch](phase-5-post-launch/) | Grow and maintain it | Ongoing — metrics, iteration, support |

See [TRANSITIONS.md](TRANSITIONS.md) for readiness checklists and common mistakes when moving between phases.

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

### Token Optimization

AI-assisted development is powerful but token-expensive. The [AI Token Optimization Guide](docs/ai-token-optimization.md) covers universal techniques for reducing credit usage, plus phase-specific tips embedded in each phase README. Key principle: control what enters context — 99% of token cost is input, not output.

## Directory Structure

```text
Playbook/
├── README.md                           # This file
├── CONTRIBUTING.md                     # How to contribute
├── LICENSE                             # MIT License
├── TRANSITIONS.md                      # Phase transition readiness checklists
├── docs/
│   ├── ROADMAP.md                      # Playbook development roadmap
│   ├── doc-surface-discipline.md       # Bounding doc sprawl: size + count/vocabulary/freshness + the re-scoping ritual
│   └── ai-token-optimization.md        # AI credit/token optimization guide
├── phase-0-concept/
│   ├── README.md                       # Phase overview, entry/exit criteria
│   ├── steps/
│   │   ├── 00-guided-concept-session.md  # Interactive Q&A shortcut for Steps 1-5
│   │   ├── 01-ideation-and-vision.md
│   │   ├── 02-discovery-questions.md
│   │   ├── 03-competitive-analysis.md
│   │   ├── 04-technical-feasibility.md
│   │   ├── 05-concept-document.md
│   │   ├── 06-app-naming.md
│   │   └── 07-phase-retrospective.md
│   └── templates/
│       ├── concept-brief.md
│       ├── concept-document.md
│       ├── competitor-matrix.md
│       ├── discovery-questionnaire.md
│       ├── technical-feasibility.md
│       └── vision-statement.md
├── phase-1-pre-production/
│   ├── README.md
│   ├── steps/
│   │   ├── 01-requirements-specification.md
│   │   ├── 02-technical-spikes.md
│   │   ├── 03-architecture-design.md
│   │   ├── 04-uiux-design.md
│   │   ├── 05-design-system.md          # Includes asset planning pipeline
│   │   ├── 06-api-data-design.md
│   │   ├── 07-project-plan.md
│   │   ├── 08-dev-environment-setup.md
│   │   └── 09-phase-retrospective.md
│   ├── templates/
│   │   ├── api-spec.md
│   │   ├── architecture-document.md
│   │   ├── content-authoring-brief.md   # Content planning for content-dependent apps
│   │   ├── design-system-spec.md
│   │   ├── dev-environment-setup.md
│   │   ├── phase-retrospective.md
│   │   ├── project-plan.md
│   │   ├── requirements-document.md
│   │   ├── spike-report.md
│   │   └── uiux-spec.md
│   └── references/
│       ├── ai-design-pipeline.md        # AI-assisted design workflows
│       ├── app-store-compliance-checklist.md  # iOS App Store submission requirements
│       ├── asset-pipeline.md            # Asset creation tools and workflows
│       └── platform-adaptation-guide.md # Web + game adaptations for each step
├── phase-2-production/
│   ├── README.md
│   ├── steps/
│   │   ├── 01-sprint-execution.md       # Includes code review + 3-tier testing
│   │   └── 02-retrospective.md
│   └── templates/
│       ├── retro-tracker.md
│       ├── sprint-0-spike-tracker.md    # Spike-driven first sprint template
│       └── sprint-tracker.md
├── phase-3-testing-qa/
│   ├── README.md
│   ├── steps/
│   │   ├── 01-test-plan.md
│   │   ├── 02-functional-testing.md
│   │   ├── 03-platform-testing.md
│   │   ├── 04-performance-testing.md
│   │   ├── 05-security-privacy-audit.md
│   │   ├── 06-beta-testing.md
│   │   └── 07-bug-fix-sprint.md
│   └── templates/
│       ├── test-plan.md
│       └── bug-tracker.md
├── phase-4-launch/
│   ├── README.md
│   ├── steps/
│   │   ├── 01-store-preparation.md
│   │   ├── 02-store-marketing-assets.md  # AppShot + BatchSnap references
│   │   ├── 03-compliance-final-check.md
│   │   ├── 04-submission.md
│   │   ├── 05-launch-marketing.md
│   │   ├── 06-launch-day.md
│   │   └── 07-web-deployment.md
│   └── templates/
│       └── store-asset-checklist.md
└── phase-5-post-launch/
    ├── README.md
    ├── steps/
    │   ├── 01-monitoring-setup.md
    │   ├── 02-user-feedback-loop.md
    │   ├── 03-iteration-planning.md
    │   ├── 04-update-cycle.md
    │   ├── 05-growth-retention.md
    │   └── 06-retrospective.md
    └── templates/
        ├── feedback-log.md
        └── project-retrospective.md
```

## Principles

- **Always have a working deliverable** — Each step ends with something concrete, not just notes
- **Document decisions, not just outcomes** — Capture the "why" so future-you understands the reasoning
- **Smallest viable process** — Only add process steps that earn their keep. If a step doesn't produce value, cut it
- **One plan, one vocabulary, one home for frozen** — Keep the planning surface small and legible: a single forward plan, a single work-item scheme, and `docs/archive/` as the only home for anything frozen. When you re-scope how work is broken down, run the re-scoping ritual so old schemes and dead trackers don't accrete. See [doc-surface-discipline.md](docs/doc-surface-discipline.md)
- **Phase gates are checkpoints, not gates** — Review exit criteria honestly, but don't let process block momentum
- **Improve the Playbook as you go** — After each project, update templates and steps with lessons learned
