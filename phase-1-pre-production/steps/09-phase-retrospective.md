# Step 9: Phase Retrospective

## Purpose

Close Phase 1, capture process learnings, validate Phase 2 readiness, and create continuity artifacts so the first Sprint 1 session starts building features instead of re-establishing context. This is the final step of Pre-Production — it draws a hard line between planning and building.

## Why This Matters

Phase 1 produces 8 specs across 8 steps. Without a deliberate close-out, gaps survive unnoticed into Sprint 1 — where they cost 10x more to fix. A missing API contract discovered mid-sprint means stopping feature work to design it. An inconsistent story ID means the AI workspace gives wrong answers. An unresolved Conditional Go item means building on an unvalidated assumption.

This step formalizes three things:
1. **Phase closure** — every deliverable exists, is complete, and is committed
2. **Phase 2 readiness** — the exit criteria are met and Sprint 1 is actionable
3. **Continuity** — a new session (human or AI) can understand the project in under 5 minutes and start Sprint 1 immediately

Phase 0's retrospective was lightweight because it handed off to more planning. This one is heavier because it hands off to actual sprint work — the next session writes production code.

## Prerequisites

- Steps 1-8 completed
- Dev environment validated (Step 8 checklist passes)
- All pre-production specs finalized and committed

## Process

### 9.1 Review Deliverables

Walk through every Phase 1 deliverable. For each one, confirm it exists, is complete, and lives in the correct location. Don't skim — open each file and verify it has real content, not placeholder sections.

**Step deliverables:**

| Step | Deliverable | Output File | Complete |
| ------ | ------------ | ------------- | ---------- |
| 1 | Requirements Document (user stories, acceptance criteria, test plans) | `docs/pre-production/requirements-document.md` | [ ] |
| 2 | Spike Reports (time-boxed validations with recommendations) | `docs/pre-production/spike-reports/` | [ ] |
| 3 | Architecture Document (system design, data models, protocols, DI) | `docs/pre-production/architecture-document.md` | [ ] |
| 4 | UI/UX Spec (screens, wireframes, user flows) | `docs/pre-production/uiux-spec.md` | [ ] |
| 5 | Design System Spec (tokens, components, SwiftUI mapping) | `docs/pre-production/design-system-spec.md` | [ ] |
| 6 | API & Data Spec (schema, contracts, data flows, Edge Functions) | `docs/pre-production/api-spec.md` | [ ] |
| 7 | Project Plan (sprints, milestones, task breakdowns, risks) | `docs/pre-production/project-plan.md` | [ ] |
| 8 | Dev Environment Setup (project, dependencies, testing, CI/CD, AI workspace, security) | `docs/pre-production/dev-environment-setup.md` | [ ] |

**Supporting artifacts:**

| Artifact | Location | Complete |
| ---------- | ---------- | ---------- |
| Project CLAUDE.md (AI instructions) | `.claude/CLAUDE.md` | [ ] |
| Coding rules files | `.claude/rules/` | [ ] |
| MEMORY.md (session memory) | `.claude/memory/MEMORY.md` | [ ] |
| Session hooks (if applicable) | `.claude/hooks/` or equivalent | [ ] |
| ROADMAP.md (task tracking) | `docs/ROADMAP.md` | [ ] |

**What to look for:**
- **Missing sections** — does every spec have all its sections filled in, or are there "TODO" markers hiding in the document?
- **Stale content** — did a decision change in a later step that wasn't back-propagated to an earlier spec? (e.g., a model renamed in Step 6 that still has the old name in Step 3)
- **Broken references** — do story IDs, model names, and service names match across specs?

If anything is incomplete, fix it now. Don't carry spec debt into Sprint 1.

### 9.2 Evaluate the Process

Reflect honestly on how Phase 1 went. This is not a formality — it's how the Playbook improves.

**What went well?**
- Which steps produced the most useful output for downstream steps?
- Where did the workflow feel natural and efficient?
- Which deliverables will you actually reference during Sprint 1?
- What decisions are you most confident in?

**What was difficult?**
- Which steps took the longest? Was the time well spent, or was there scope drift?
- Where did you get stuck or need to backtrack?
- Which steps had the most rework after a later step revealed a gap?
- What was underestimated in effort or complexity?

**What was missing?**
- Were there gaps the process didn't cover that you had to figure out yourself?
- Were there deliverables you wished existed earlier in the sequence?
- Did context get lost between sessions or steps?

**What would you change?**
- Should any steps be reordered, combined, or split?
- Should any templates be expanded or simplified?
- Are there new steps or templates that should exist?

**Phase 1-specific process questions:**

- **Cross-step consistency:** Did identifiers (story IDs, model names, service names) stay consistent across all 8 specs, or did naming drift between steps?
- **Spike value:** Were the technical spikes (Step 2) worth the time investment, or could they have been deferred to Sprint 1 without risk?
- **Design system connection:** Did the design system (Step 5) connect cleanly to the UI/UX spec (Step 4)? Were tokens and components usable as defined, or did they need rework?
- **AI workflow evaluation:** If you used AI throughout Phase 1 — what context was missing from the AI? What was over-specified? What prompting patterns worked best? Where did the AI need the most correction?

### 9.3 Validate Phase 2 Readiness

This is the Phase 1 exit gate. Before declaring Phase 1 complete, cross-reference every item on the Phase 1 Exit Criteria from the README. Don't self-certify — actually check.

**Phase 1 Exit Criteria checklist:**

- [ ] Requirements are formalized with acceptance criteria and TDD test plans
- [ ] Technical spikes are completed for all "Complex" or "Unknown" items
- [ ] Architecture is documented with testability patterns
- [ ] UI/UX designs exist for all key screens and user flows
- [ ] Design system is established with reusable components and SwiftUI specs
- [ ] API contracts and database schema are designed
- [ ] Project plan with milestones and sprints exists
- [ ] Dev environment is configured with testing infrastructure and a "hello world" builds
- [ ] Conditional Go items from Phase 0 are resolved

**Sprint 1 readiness checks:**

- [ ] Walking skeleton stories are identified and assigned to Sprint 1-2
- [ ] Sprint 1 stories have task breakdowns with size estimates
- [ ] Sprint 1 dependencies are all resolved (no story depends on unbuilt work)
- [ ] The dev environment builds and at least one test passes
- [ ] External services are configured and verified

**AI workspace readiness check:**

Start a fresh AI session (or simulate one by reading only the context files). Without any manual briefing, the AI should be able to answer:

1. **What am I building?** — app purpose, target audience, core value proposition
2. **What's the architecture?** — tech stack, patterns, key services, data models
3. **What's Sprint 1?** — theme, stories, first task to pick up, what "done" looks like

If the AI can't answer these three questions from the workspace context alone, the continuity artifacts need work before Phase 1 closes.

### 9.4 Document Process Improvements

Translate observations from Section 9.2 into specific, actionable changes. Feed these back into the Playbook so the next project benefits.

The difference between an observation and an improvement: "Step 4 was hard" is an observation. "Step 4 should extract AI tooling setup into a standalone reference doc so the step guide focuses on design decisions" is an improvement.

```markdown
### Add to Playbook
- [Specific change to an existing step guide — which step, which section, what to add/change]

### Change in Process
- [Process order changes, step combinations, new checkpoints, workflow changes]

### New Template/Reference
- [What document should exist, why, and where it fits in the step sequence]
```

**Examples of good process improvements:**
- "Step 3 should include a section on mapping architecture modules to folder structure, since Step 8 requires this mapping and it's easier to define during architecture design"
- "Add a cross-reference check between Steps 1 and 6 — story IDs in the API spec should match the requirements doc, and drift happens silently"
- "Create a reference doc for AI-assisted design tooling (Midjourney, Stitch, Figma) — this was too heavy for Step 4 and kept expanding the step guide"

### 9.5 Create Phase 2 Continuity Package

The continuity package is the "handoff" for Phase 2. Anyone — a new team member, a future you who forgot the details, or an AI starting a fresh session — should understand the project in under 5 minutes and know exactly what to build first.

This is heavier than Phase 0's continuity section because Phase 2 has actual sprint work to prepare for. Phase 0 handed off to more planning. This hands off to production code.

**MEMORY.md — Finalize**

Update MEMORY.md with all Phase 1 decisions. This is the cheat sheet the AI reads at session start. It should include:
- Project overview (one paragraph)
- Current phase: Phase 2 — Production
- Tech stack and versions
- Architecture pattern and DI approach
- Data model summary (names, key relationships)
- Service protocol inventory
- Design tokens (primary colors, typography, spacing)
- Database structure (table names, key relationships, RLS patterns)
- Sprint plan summary (total sprints, current sprint)
- Testing approach and framework
- Key decisions and "do not revisit" items

Update "Current Phase" from Phase 1 to Phase 2. Update any stale information from earlier in Phase 1 that was superseded by later steps.

**ROADMAP.md — Update**

- Move all Phase 1 tasks to the Done section with completion dates
- Move Phase 2 / Sprint 1 stories to the Active section
- Ensure Sprint 1 stories have enough detail to be actionable (story ID, brief description, priority)
- Verify the Backlog section has Sprint 2+ stories queued

**Sprint 1 Kickoff Brief**

Write a concise Sprint 1 kickoff section in the retrospective deliverable (not a separate file). This is the single most valuable continuity artifact — it's what the AI reads first in the next session.

The kickoff brief should answer:
- **Sprint 1 theme:** What capability is this sprint delivering? (e.g., "Walking Skeleton — core data loop from activity import to route progress")
- **Stories:** Which stories are in Sprint 1, with IDs and one-line descriptions
- **First task to pick up:** The specific task to start with and why (usually the foundational data layer or service that other tasks depend on)
- **Key dependencies:** External services, APIs, or infrastructure that Sprint 1 tasks rely on
- **What "done" looks like:** The demo scenario for Sprint 1 — what can the user do at the end that they couldn't before?

### 9.6 Close Phase 1

Final sign-off checklist. Every item must pass before Phase 1 is officially closed.

- [ ] All 8 step deliverables confirmed complete (Section 9.1)
- [ ] All supporting artifacts confirmed complete (Section 9.1)
- [ ] Process evaluation completed with all 4 reflection areas (Section 9.2)
- [ ] Phase 2 readiness validated against exit criteria (Section 9.3)
- [ ] Sprint 1 readiness checks pass (Section 9.3)
- [ ] AI workspace readiness verified (Section 9.3)
- [ ] Process improvements documented and fed back to Playbook (Section 9.4)
- [ ] MEMORY.md finalized with Phase 2 as current phase (Section 9.5)
- [ ] ROADMAP.md updated — Phase 1 Done, Phase 2 Active (Section 9.5)
- [ ] Sprint 1 kickoff brief written (Section 9.5)
- [ ] No unresolved Conditional Go items from Phase 0
- [ ] Dev environment validation passes (Step 8 checklist)
- [ ] Everything committed to version control

Once every item is checked, Phase 1 is closed. The next session is Sprint 1.

## Key Principle

> The retrospective isn't about being thorough — it's about being honest. A two-line note about what actually needs to change is worth more than a page of vague observations.

## Deliverable

Complete the [Phase Retrospective Template](../templates/phase-retrospective.md) and save it to your project's `docs/pre-production/phase-retrospective.md`.

## Definition of Done

- [ ] All Phase 1 deliverables confirmed complete (8 step outputs + supporting artifacts)
- [ ] Process evaluation completed (4 reflection areas + Phase 1-specific questions)
- [ ] Phase 2 readiness validated against exit criteria
- [ ] Process improvements documented and fed back to Playbook
- [ ] Phase 2 continuity package complete (MEMORY.md, ROADMAP.md, Sprint 1 brief)
- [ ] Everything committed to version control
- [ ] ROADMAP.md shows Phase 1 Done, Phase 2 Active

## Tips

- **"The retrospective isn't about being thorough — it's about being honest."** The same principle from Phase 0 applies here. Two specific observations beat ten vague ones.
- **Don't skip the Phase 2 readiness check.** Rushing into Sprint 1 with unresolved gaps costs more than the 30 minutes it takes to verify. A missing API contract discovered mid-sprint means stopping feature work to design it.
- **The Sprint 1 kickoff brief is the most valuable continuity artifact.** It's what the AI reads first in the next session. If it's wrong or vague, Sprint 1 starts with confusion instead of momentum.
- **Process improvements should be specific and actionable.** "Step 4 was hard" is an observation. "Step 4 should extract AI tooling into a reference doc" is an improvement. Only the second one changes the Playbook.
- **Check cross-step consistency during the deliverable review.** Story IDs, model names, service names, and design tokens should be identical across all specs. Inconsistencies create ambiguity during implementation — the AI will pick whichever name it finds first.
- **If you used AI throughout Phase 1, evaluate the AI workflow too.** What context was missing at session start? What was over-specified and ignored? What prompting patterns produced the best output? This is process improvement that directly impacts Sprint 1 productivity.
- **Fix spec issues now, not in Sprint 1.** If the deliverable review surfaces inconsistencies or gaps, resolve them before closing Phase 1. Carrying spec debt into production is like starting a road trip with a flat tire — you can do it, but you'll stop soon.
