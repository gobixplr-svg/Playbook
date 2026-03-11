# Step 1: Test Plan

## Purpose

Translate the acceptance criteria from Phase 1 into a structured test plan that defines exactly what to test, how to test it, and what "ship ready" means. By the end of this step, every user story has mapped test cases, risk-prioritized execution order, and a device matrix — so testing is systematic, not ad-hoc.

## Why This Matters

Without a test plan, testing devolves into "click around and see if anything breaks." That misses edge cases, skips entire features, and gives false confidence. A test plan derived directly from your Given/When/Then acceptance criteria creates a traceable chain: requirement → test case → pass/fail result. Nothing slips through because nothing is invented on the fly.

The test plan also forces you to decide what NOT to test. Solo developers cannot test everything. Prioritizing by risk means you spend time on the flows that would break user trust if they failed, not on cosmetic edge cases that can ship imperfect.

## Prerequisites

- Phase 2 Production complete — all Must Have features implemented
- Phase 1 Requirements Document with user stories and Given/When/Then acceptance criteria
- App builds and runs on all target platforms
- Phase 2 sprint testing (Tier 1-3) complete — this is not your first time running the app

## Process

### 1.1 Gather Acceptance Criteria

Pull every Given/When/Then acceptance criterion from the Phase 1 Requirements Document. Organize them by epic and user story.

```text
Epic: Route System
  Story: US-01 — View route list
    AC1: Given the user opens the app, When routes exist, Then the route list displays with name, distance, and thumbnail.
    AC2: Given the user opens the app, When no routes exist, Then an empty state message appears.
  Story: US-02 — View route detail
    AC1: ...
```

**Count your test cases.** Each acceptance criterion maps to at least one test case. A typical v1 app has 40-80 acceptance criteria. If you have significantly fewer, your Phase 1 requirements may be under-specified — review before proceeding.

### 1.2 Map Stories to Test Cases

For each acceptance criterion, create a test case entry:

| Test ID | Story | AC | Description | Type | Priority |
| ------- | ----- | -- | ----------- | ---- | -------- |
| TC-01 | US-01 | AC1 | Route list displays name, distance, thumbnail | Functional | Critical |
| TC-02 | US-01 | AC2 | Empty state shows when no routes exist | Functional | High |
| TC-03 | US-02 | AC1 | Route detail shows map, stats, progress | Functional | Critical |

**Test types:**

- **Functional** — Does the feature work as specified?
- **Platform** — Does it work across devices, OS versions, accessibility settings?
- **Performance** — Does it meet speed, memory, and battery benchmarks?
- **Edge case** — What happens under unusual conditions?

**Priority levels:**

- **Critical** — Core user flows. Failure means the app is broken. Test these first.
- **High** — Important features. Failure degrades the experience significantly.
- **Medium** — Secondary features. Failure is noticeable but not blocking.
- **Low** — Cosmetic or edge cases. Failure is minor.

### 1.3 Define Test Scope

Explicitly document what is in scope and out of scope for this testing cycle.

**In scope:**

- All Must Have user stories and their acceptance criteria
- All Should Have user stories that were implemented
- Cross-feature integration flows (e.g., complete a route → earn a badge → see it in profile)
- Device and OS matrix (defined in 1.5)
- Accessibility basics (VoiceOver, Dynamic Type)

**Out of scope (with rationale):**

- Features deferred to v2 — not implemented, nothing to test
- Automated regression suite — manual testing is sufficient for v1 scope
- Load testing / concurrent users — single-user app, not applicable
- Penetration testing — handled separately in Step 5 (Security Audit)

Write this down. When testing pressure hits, the scope document prevents you from either skipping critical tests or spiraling into testing things that don't matter for launch.

### 1.4 Prioritize by Risk

Rank test cases by risk using two factors:

1. **Impact** — How bad is it if this breaks? (Critical / High / Medium / Low)
2. **Likelihood** — How likely is it to fail? (High for new/complex code, Low for simple/proven code)

| Risk Level | Impact | Likelihood | Action |
| ---------- | ------ | ---------- | ------ |
| Must test | Critical | Any | Test first, test thoroughly |
| Should test | High | High | Test in main pass |
| Could test | Medium | Low | Test if time permits |
| Skip | Low | Low | Accept the risk |

**Critical user flows to always test first:**

1. First launch / onboarding
2. Core loop (the thing users do every session)
3. Data persistence (close app → reopen → data is still there)
4. Authentication (sign up, sign in, sign out, session restore)
5. Any flow involving real money or irreversible actions

### 1.5 Define Device Matrix

Testing on every device is impossible. Define a matrix that covers the meaningful variation.

**Recommended minimum for iOS:**

| Device | Screen Size | Why |
| ------ | ----------- | --- |
| iPhone SE (3rd gen) | 4.7" | Smallest supported screen — catches layout overflow, truncation |
| iPhone 16 | 6.1" | Most common screen size — the "normal" experience |
| iPhone 16 Pro Max | 6.9" | Largest screen — catches spacing/alignment issues at scale |

**iOS version coverage:**

| Version | Why |
| ------- | --- |
| Current (iOS 18.x) | Primary target — most users |
| Current minus 1 (iOS 17.x) | Catches API availability issues, deprecation warnings |

**Test configuration matrix:**

| Configuration | Variations to Test |
| ------------- | ------------------ |
| Appearance | Light mode, Dark mode |
| Text size | Default, Largest (accessibility) |
| Motion | Standard, Reduce Motion enabled |
| Network | Wi-Fi, Cellular, Airplane mode |
| Orientation | Portrait only (if locked), Portrait + Landscape (if supported) |

You do not need to test every combination. Use a pragmatic approach:

- Test all features on iPhone 16 / iOS 18 / Light mode / Default text (baseline)
- Test critical flows on SE and Pro Max (layout extremes)
- Test critical flows in Dark mode and Large Text
- Test offline behavior with Airplane mode

### 1.6 Reference the 3-Tier Testing Cadence

Your test plan builds on top of the testing already done during Phase 2 sprints:

- **Tier 1 (Build Check)** — Already caught compilation errors during development
- **Tier 1.5 (Visual Smoke Test)** — Already verified views render correctly in simulator
- **Tier 2 (Test Plan per Story)** — Already documented test coverage per story
- **Tier 3 (Full Test Suite)** — Already run at sprint boundaries

Phase 3 testing is **Tier 4: Ship Readiness**. It covers:

- End-to-end flows across the full app (not just per-story)
- Real device testing (not just simulator)
- Platform variation (devices, OS versions, accessibility settings)
- Performance profiling (not just "it works" but "it works well")
- Edge cases that span multiple stories

### 1.7 Define "Ship Ready" Criteria

Document the specific, measurable criteria that must be met before submitting to the App Store.

```text
Ship Ready Criteria:
- [ ] All Critical test cases pass
- [ ] All High test cases pass (or failures are documented with workarounds)
- [ ] No crash bugs remain open
- [ ] No data loss bugs remain open
- [ ] App launches in under 2 seconds on oldest supported device
- [ ] Memory usage stays under [X] MB during normal use
- [ ] Scroll frame rate maintains 60fps on list views
- [ ] VoiceOver can navigate all primary flows
- [ ] Dynamic Type does not break layout at largest size
- [ ] App works offline for core features (or degrades gracefully)
- [ ] App Store metadata and screenshots are prepared
```

Adjust thresholds based on your app's complexity. The point is to define them now, before testing starts, so "ship ready" is a checklist — not a feeling.

## Anti-Patterns

- **Testing without a plan** — "I'll just use the app and see if anything breaks" misses systematic coverage. You'll test your favorite features and skip the ones you're less familiar with.
- **Testing everything equally** — Spending the same time on a settings toggle as on the core user flow. Prioritize by risk.
- **Testing only the happy path** — The app works when everything goes right. It crashes when the network drops, the database is empty, or the user does something unexpected. Test those too.
- **Skipping the device matrix** — "It works on my phone" is not a test strategy. The iPhone SE's smaller screen and the Pro Max's larger screen expose different layout bugs.
- **Moving the goalposts** — Changing "ship ready" criteria during testing to accommodate bugs you don't want to fix. Define the bar before you know what's under it.
- **Treating Phase 2 testing as sufficient** — Sprint-level testing validates individual stories. Phase 3 validates the whole product. Features that work in isolation can break when combined.

## Deliverable

A **Test Plan Document** saved to your project's `docs/testing/test-plan.md` containing:

- Test case inventory mapped to user stories and acceptance criteria
- Test scope (in scope / out of scope with rationale)
- Risk-prioritized execution order
- Device and configuration matrix
- Ship ready criteria with measurable thresholds

## Definition of Done

- [ ] All acceptance criteria from Phase 1 Requirements Document extracted and counted
- [ ] Test cases created for every acceptance criterion with ID, description, type, and priority
- [ ] Test scope defined (in scope and out of scope with rationale)
- [ ] Test cases prioritized by risk (impact x likelihood)
- [ ] Device matrix defined (devices, iOS versions, configurations)
- [ ] Ship ready criteria documented with measurable thresholds
- [ ] Test plan reviewed against the Requirements Document — no orphan requirements without test cases
- [ ] Test plan saved to `docs/testing/test-plan.md`

## Tips

- **Start from the requirements, not from the app.** If you build your test plan by clicking through the app, you'll test what's there and miss what's missing. Start from the acceptance criteria and verify each one exists and works.
- **Use your acceptance criteria verbatim.** The Given/When/Then format from Phase 1 translates directly into test steps. "Given X, When Y, Then Z" becomes: set up X, do Y, verify Z. No translation needed.
- **The "out of scope" section is as important as "in scope."** It prevents scope creep during testing and gives you permission to stop testing things that don't matter for launch.
- **Count your test cases.** A number makes progress trackable. "We've passed 47 of 62 test cases" is actionable. "Testing is going well" is not.
- **Keep the test plan lightweight.** A spreadsheet or markdown table is fine. Don't build a test management system — you're a solo developer, not a QA department.
- **AI tools can help generate test cases.** Feed your acceptance criteria to an AI assistant and ask it to generate test case tables. Review and adjust — but let it handle the mechanical mapping.
