# Phase Transition Guide

How to know when you're ready to move to the next phase — and what to watch out for if you move too early.

---

## Phase 0 → Phase 1 (Concept → Pre-Production)

### Readiness Checklist

- [ ] Concept Document is written with a clear Go decision
- [ ] Vision statement articulates the problem, audience, and solution
- [ ] Competitive landscape is mapped with a differentiation strategy
- [ ] Technical feasibility is confirmed with no unresolved blockers
- [ ] Discovery questions are answered (or explicitly deferred with rationale)
- [ ] Working app name is selected with availability confirmed
- [ ] Phase 0 retrospective is completed
- [ ] ROADMAP.md has Phase 1 tasks defined

### Common Mistakes When Transitioning Too Early

- **Skipping competitive analysis.** You assume your idea is unique. It probably isn't. Discovering a dominant competitor in Phase 2 means pivoting under pressure instead of pivoting cheaply.
- **Vague concept.** "An app that helps people be productive" is not a concept. If you can't describe the core user flow in one sentence, the concept isn't ready.
- **Unresolved feasibility questions.** Moving forward with a "probably possible" technical assumption means building on hope. Resolve unknowns or explicitly accept the risk.

### What to Carry Forward

- Concept Document and Concept Brief (read first in Phase 1 sessions)
- CLAUDE.md with project context and key decisions
- ROADMAP.md with Phase 1 tasks queued
- Any deferred questions as explicit Phase 1 investigation items

---

## Phase 1 → Phase 2 (Pre-Production → Production)

### Readiness Checklist

- [ ] All 8 step deliverables exist and are complete (requirements, spikes, architecture, UI/UX, design system, API spec, project plan, dev environment)
- [ ] Dev environment builds successfully and at least one test passes
- [ ] Sprint 1 stories are broken into tasks with size estimates
- [ ] Walking skeleton stories are identified for Sprint 1-2
- [ ] Conditional Go items from Phase 0 are resolved
- [ ] Cross-spec consistency verified (story IDs, model names, service names match)
- [ ] MEMORY.md is updated for Phase 2 with architecture and sprint context
- [ ] AI workspace can answer: What am I building? What's the architecture? What's Sprint 1?

### Common Mistakes When Transitioning Too Early

- **Incomplete architecture.** "We'll figure out the data model during Sprint 1" means Sprint 1 is actually still Phase 1. Design the data model first.
- **No task breakdowns.** Sprint 1 has stories but no tasks. You start the sprint and immediately spend half of it decomposing work instead of building.
- **Dev environment not validated.** The project "should build" but nobody has actually run it. First sprint starts with environment debugging.
- **Specs that don't agree.** The API spec uses different model names than the architecture document. Every sprint task requires cross-referencing multiple specs to figure out the truth.

### What to Carry Forward

- All Phase 1 specs (referenced throughout production)
- Sprint plan with task breakdowns
- Design system tokens and component specs
- MEMORY.md and ROADMAP.md (session continuity)

---

## Phase 2 → Phase 3 (Production → Testing & QA)

### Readiness Checklist

- [ ] All "Must Have" features are implemented and functional
- [ ] App builds and runs on all target platforms without crashes on the happy path
- [ ] Unit and integration tests pass (no skipped tests without documented reasons)
- [ ] Known bugs are logged with severity levels
- [ ] Tech debt is documented (not necessarily resolved, but visible)
- [ ] All third-party integrations are connected and working

### Common Mistakes When Transitioning Too Early

- **"Almost done" features.** A feature that works 80% of the time is not done. QA will immediately find the 20% and send it back. Finish it or cut it from v1.
- **No test suite.** Moving to QA without automated tests means every future change requires full manual regression. This gets exponentially more expensive.
- **Ignoring known bugs.** Entering QA with a pile of known issues means QA rediscovers and re-reports bugs you already know about. Fix critical and high bugs first.

### What to Carry Forward

- Requirements document with acceptance criteria (QA tests against these)
- Known bug list with severity levels
- Test suite (unit and integration tests transfer directly into QA validation)
- Architecture document (for security and performance audit context)

---

## Phase 3 → Phase 4 (Testing & QA → Launch)

### Readiness Checklist

- [ ] All acceptance criteria from requirements are verified as passing
- [ ] No critical or high-priority bugs remain open
- [ ] App performs within defined benchmarks (launch time, memory, battery)
- [ ] Privacy and security audit passes (privacy manifests, data handling, auth)
- [ ] Beta testers have validated core flows
- [ ] App Store / Play Store compliance is verified (age rating, content declarations, ATT)
- [ ] Store assets are ready (screenshots, icon, preview video, description)

### Common Mistakes When Transitioning Too Early

- **Skipping beta testing.** Internal testing misses real-world usage patterns. Beta testers find the bugs you can't — weird device configurations, unexpected workflows, network conditions you don't test for.
- **Unresolved compliance issues.** App Store rejections cost 1-7 days per round. Discovering a privacy manifest issue after submission means a week of delay. Audit compliance before submitting.
- **Performance not profiled on real devices.** The app runs fine in Simulator but is slow on older phones. Profile on the oldest device you support.
- **"Ship it and fix it later."** Critical bugs don't get better after launch. They get one-star reviews.

### What to Carry Forward

- Test results and QA sign-off (evidence that the app was validated)
- Store asset files (screenshots, videos, icon at all sizes)
- Privacy manifests and compliance documentation
- Known issues list (minor items deferred to post-launch)

---

## Phase 4 → Phase 5 (Launch → Post-Launch)

### Readiness Checklist

- [ ] App is live and downloadable on all target platforms
- [ ] Analytics are collecting data (verified with real events, not just SDK initialization)
- [ ] Crash reporting is active (verified with a test crash)
- [ ] App Store listing is published with optimized metadata
- [ ] Support channel exists (at minimum, a support email in the App Store listing)
- [ ] First 48 hours of post-launch monitoring completed without critical issues

### Common Mistakes When Transitioning Too Early

- **No monitoring in place.** The app is live but you have no visibility into crashes, usage, or revenue. You discover problems from one-star reviews instead of dashboards.
- **Skipping launch-day monitoring.** The first 48 hours are when you'll see issues that testing missed — edge-case crashes, server load problems, unexpected user behavior. Don't walk away after hitting "Release."
- **No feedback channel.** Users have no way to reach you except App Store reviews (public and permanent). A support email gives frustrated users a private path to resolution.

### What to Carry Forward

- Analytics and crash reporting configuration
- App Store Connect access and store listing
- Launch metrics as a baseline (downloads, conversion rate, initial retention)
- Known issues list from Phase 3/4 (deferred minor bugs to address in first updates)
- All Playbook specs (referenced when planning iterations and updates)
