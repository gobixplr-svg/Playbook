# Sprint Tracker — [App Name]

## Sprint Info

| Field | Value |
| ------- | ------- |
| **Sprint** | # |
| **Theme** | |
| **Sprint Type** | Construction / Convergence / Polish |
| **Goal** | |
| **Dates** | YYYY-MM-DD — YYYY-MM-DD |
| **Estimate** | N sessions / ~N hours (log actuals at close; copy row into `docs/estimation-tracker.md`) |
| **Demo Scenario** | |

---

## Stories

### [Story ID] — [Story Title]

**Status:** Not Started / In Progress / Done / Deferred
**Priority:** Must Have / Should Have

| Task | Layer | Est | Status | Notes |
| ------ | ------- | ----- | -------- | ------- |
| | Data | S/M/L | | |
| | Service | S/M/L | | |
| | View | S/M/L | | |
| | Integration | S/M/L | | |
| | Tests | S/M/L | | |

**Test Plan:**
- New tests: N | Modified: N | Running total: N
- [ ] TestSuite (count): testName1, testName2...

**Visual Smoke Test (required for View layer):**
- [ ] Ran in simulator — all elements visible (light mode)
- [ ] Ran in simulator — all elements visible (dark mode)
- [ ] Colors match design spec
- [ ] Navigation works (push, back, dismiss)
- [ ] Layout is intentional — no clipping, overflow, or invisible text

---

<!-- Copy the story block above for each story in the sprint -->

## Session Log

<!-- Log a summary at the end of each work session. Number sequentially. -->

### Session 1 — [Date]

- **Done:**
- **Discovered:**
- **Next:**

---

## First Full Playthrough (Convergence Sprints Only)

<!-- Skip this section for Construction sprints. For Convergence sprints, play the full user flow early and log findings. -->

- [ ] Full user flow played end-to-end (not debugging — playing)

| Category | Count | Issues Found |
| ---- | ---- | ---- |
| Broken (doesn't work) | | |
| Wrong (works but incorrect) | | |
| Missing (design assumed it exists) | | |

**Re-plan needed?** Yes / No
**Stories deferred to make room:**

---

## Integration Buffer

- [ ] Full test suite passes (Tier 3: `xcodebuild test`)
- [ ] **User Flow Walkthrough:**
  - [ ] Cold start (Command+R) — app lands in correct state
  - [ ] New user flow: discover → start → complete core loop
  - [ ] Returning user flow: restart → resume previous state (mock seed data)
  - [ ] Edge cases: double-tap, swipe back mid-action, abandon/cancel
  - [ ] Light mode verified
  - [ ] Dark mode verified
  - [ ] Screenshots compared to mockups/design spec
- [ ] ROADMAP.md updated
- [ ] No regressions from prior sprints

## Blockers

| Blocker | Impact | Resolution | Status |
| --------- | -------- | ------------ | -------- |
| | | | |

## Sprint Review

### What shipped
-

### What didn't ship (carryover)
-

### Notes for next sprint
-
