# Step 7: Bug Fix Sprint

## Purpose

Resolve all launch-blocking bugs found during Steps 2-6, verify the fixes don't introduce regressions, and produce a production readiness sign-off that bridges Phase 3 (Testing & QA) to Phase 4 (Launch). This is the final step before the app enters the launch pipeline.

## Why This Matters

Testing without fixing is just documentation. Steps 2-6 generated bug reports, performance findings, security audit items, and beta feedback. This step is where that work pays off — every Critical and High bug gets resolved, and every fix gets verified. Skipping this step (or doing it informally) means shipping with known issues hidden in scattered notes across five different reports.

This step also serves as the quality gate between "it works in testing" and "it's ready for production." The production readiness checklist at the end is the formal handoff to Phase 4 — if any item fails, the app isn't ready to submit.

## Prerequisites

- Steps 2-6 completed (functional, platform, performance, security, and beta testing done)
- All bug reports, audit findings, and beta feedback collected
- Access to the bug tracker (or the template from this phase)
- App builds and runs on real devices

## Process

### 7.1 Consolidate All Issues

Gather every issue found across Steps 2-6 into a single bug tracker. Issues are currently scattered across test results, platform reports, performance benchmarks, security audit findings, and beta feedback logs. Consolidation creates a single source of truth.

**Sources to collect from:**

| Source | Step | Typical Issues |
| ---- | ---- | ---- |
| Functional test results | Step 2 | Failed test cases, logic errors, edge case failures |
| Platform test results | Step 3 | Device-specific crashes, layout issues, OS version incompatibilities |
| Performance benchmarks | Step 4 | Slow operations, memory leaks, battery drain |
| Security audit report | Step 5 | Hardcoded secrets, missing RLS policies, privacy gaps |
| Beta feedback log | Step 6 | User-reported bugs, usability issues, crashes in the wild |

**For each issue, record:**

- Bug ID (sequential: BUG-001, BUG-002, etc.)
- Title (concise, describes the symptom)
- Source step (where it was found)
- Severity (assigned in Section 7.2)
- Steps to reproduce
- Expected vs. actual behavior
- Device/OS (if platform-specific)
- Status (Open)

Use the [Bug Tracker Template](../templates/bug-tracker.md) if you don't have a tracker set up.

### 7.2 Triage and Prioritize

Assign a severity to every issue. This is the single most important decision in this step — it determines what gets fixed before launch and what gets deferred.

**Severity definitions:**

| Severity | Criteria | Examples | Action |
| ---- | ---- | ---- | ---- |
| **Critical** | Blocks launch. Crash, data loss, security vulnerability, guaranteed App Store rejection. | Crash on launch, data corruption, hardcoded API key, missing privacy manifest | Must fix before launch |
| **High** | Degrades the core experience. Users will notice and be frustrated, but the app doesn't crash or lose data. | Broken navigation flow, incorrect calculations, slow load time (5+ seconds), confusing onboarding step | Should fix before launch |
| **Medium** | Cosmetic or minor functional issue. Users may notice but can complete their tasks. | Misaligned UI element, truncated text, minor color inconsistency, animation glitch | Document as known issue, fix in v1.1 |
| **Low** | Nice to fix. Improvement opportunity that doesn't affect the user experience in a meaningful way. | Placeholder text in debug screen, suboptimal error message wording, minor accessibility label | Track for future work |

**Triage rules:**

1. Security findings from Step 5 rated Critical or High are automatically Critical or High here — don't downgrade them
2. Crashes are always Critical, regardless of how rare they are
3. Data loss or corruption is always Critical
4. If 3+ beta testers reported the same issue independently, bump its severity up one level
5. App Store rejection risks (missing privacy manifest, incorrect age rating, missing required metadata) are Critical

**After triage, your tracker should show:**

- Total issues by severity
- Clear separation between "must fix" (Critical + High) and "defer" (Medium + Low)
- No issues left at "Unassigned" severity

### 7.3 Fix Critical Bugs

Work through all Critical bugs first. These are launch blockers — the app cannot ship until every Critical bug is resolved.

**For each Critical bug:**

1. Reproduce the bug (if you can't reproduce it, add debugging instrumentation and ask beta testers to retry)
2. Identify the root cause (not just the symptom)
3. Implement the fix
4. Write or update the test case that should have caught this
5. Verify the fix on the device/OS where it was reported
6. Update the bug tracker: status = Fixed, resolution commit/PR

**Escalation:** If a Critical bug cannot be fixed without significant rework, evaluate whether the affected feature can be disabled or removed for v1.0. Shipping without a feature is better than shipping with a feature that crashes.

### 7.4 Fix High Bugs

After all Critical bugs are resolved, work through High bugs. These degrade the user experience but don't block the app from functioning.

**For each High bug:**

1. Reproduce and identify root cause
2. Implement the fix
3. Verify the fix
4. Update the bug tracker

**Time-boxing:** If the bug fix sprint has a deadline (it should — don't let it expand indefinitely), High bugs that can't be fixed in time should be re-triaged. Can the severity be reduced to Medium with a workaround? Can the affected flow be simplified? Be honest about what's achievable.

### 7.5 Document Deferred Issues

Medium and Low bugs are not being fixed before launch. That's intentional. But they need to be documented so they don't get lost.

**For each deferred issue:**

1. Confirm the severity is correct (last chance to bump anything up)
2. Write a brief note explaining why it's deferred and when it should be fixed
3. Add it to ROADMAP.md in the Backlog section with a target version (v1.1, v1.2, etc.)

**Known issues document:**

Create a brief known issues section in the bug tracker or release notes. This serves two purposes:

- Internal: ensures deferred bugs are tracked and not forgotten
- External (optional): if you publish known issues, it shows users you're aware and working on them

### 7.6 Regression Testing

Fixing bugs introduces new bugs. Every fix must be verified in context, not just in isolation.

**Re-run affected test cases:**

1. For each bug fix, identify which test cases from Steps 2-3 cover the affected area
2. Re-run those test cases (not the entire suite — targeted regression)
3. If any test cases fail, triage the new failure before moving on

**Full user flow walkthrough:**

After all Critical and High bugs are fixed, do a complete walkthrough of every core user flow:

1. Fresh install (delete app, reinstall from TestFlight or Xcode)
2. Onboarding / first-run experience
3. Account creation or sign-in
4. Primary feature flow (the main thing the app does)
5. Secondary feature flows
6. Settings, profile, account management
7. Edge cases: offline mode, backgrounding, low storage, accessibility settings
8. Both light and dark mode
9. On at least 2 different device sizes (e.g., iPhone SE and iPhone 15 Pro Max)

**If the walkthrough reveals new issues:** Triage them using the same severity framework from Section 7.2. Only Critical issues found during regression should block launch.

### 7.7 Code Review on Bug Fixes

Bug fix code is written under time pressure and focused on a specific symptom. This makes it prone to shortcuts, incomplete fixes, and side effects. Review all bug fix changes before declaring them done.

**Run a structured code review on the bug fix diff:**

```text
git diff <pre-bugfix-commit>..HEAD
```

**Review focus areas for bug fixes:**

| Dimension | What to Check |
| ---- | ---- |
| **Completeness** | Does the fix address the root cause, or just the symptom? |
| **Side effects** | Could the fix break other features? Check callers of modified functions. |
| **Test coverage** | Is there a test that would catch this bug if it regressed? |
| **Consistency** | Does the fix follow the same patterns as the rest of the codebase? |
| **Security** | Did the fix introduce any new security concerns? (especially for auth/data fixes) |

### 7.8 Production Readiness Checklist

This checklist is the bridge between Phase 3 (Testing & QA) and Phase 4 (Launch). Every item must pass before the app enters the launch pipeline.

**App quality:**

- [ ] Zero Critical bugs open
- [ ] Zero High bugs open (or explicitly accepted with documented rationale)
- [ ] All deferred bugs documented in ROADMAP.md with target version
- [ ] Full user flow walkthrough completed without issues
- [ ] Code review completed on all bug fix changes

**Performance:**

- [ ] App launch time within target (Step 4 benchmarks)
- [ ] Memory usage stable, no leaks detected in extended use
- [ ] No excessive battery drain (Step 4 benchmarks)
- [ ] Network requests complete within acceptable timeframes

**Privacy and security:**

- [ ] Security audit findings resolved (Step 5 — all Critical and High)
- [ ] PrivacyInfo.xcprivacy complete and accurate
- [ ] App Privacy Details match actual data collection
- [ ] No hardcoded secrets in codebase
- [ ] RLS policies verified on all database tables

**App Store readiness:**

- [ ] App builds and archives without warnings or errors
- [ ] App icon in all required sizes
- [ ] Launch screen configured
- [ ] Required device capabilities declared correctly
- [ ] Privacy usage descriptions present for all requested permissions
- [ ] Age rating questionnaire answers accurate
- [ ] App Store screenshots captured (or ready to capture in Phase 4)

**Beta validation:**

- [ ] Beta testing ran for at least 1-2 weeks
- [ ] No new Critical bugs in the last 2-3 TestFlight builds
- [ ] Core flows validated by external testers on real devices

**Sign-off:**

If every item above passes, the app is production-ready. Record the sign-off date and the build number/version that passed.

```text
Production Readiness Sign-off
Date: [YYYY-MM-DD]
Build: [version] ([build number])
Signed off by: [name]
Notes: [any caveats or conditions]
```

## Anti-Patterns

- **Fixing bugs without reproducing them first** — If you can't reproduce a bug, you can't verify the fix. Guessing at the cause leads to incomplete fixes that resurface later.
- **Skipping triage** — Fixing bugs in the order they were reported (or the order that's most interesting) instead of by severity. Critical bugs first, always.
- **Downgrading severity to avoid work** — Calling a crash "Medium" because it only happens on older devices. A crash is Critical regardless of how many users it affects.
- **Infinite bug fix sprint** — Letting the sprint expand as new issues are found during regression. Set a deadline. Triage new findings the same way — only Critical blocks launch.
- **Fixing and forgetting** — Fixing a bug without adding a test that catches the regression. If it broke once, it can break again.
- **Bug fix tunnel vision** — Fixing the symptom without understanding the root cause. A band-aid fix that works today may fail when conditions change.
- **Skipping code review on fixes** — Bug fix code is high-risk code written under pressure. It deserves more review attention, not less.
- **Perfection paralysis** — Waiting for zero bugs before shipping. Zero bugs doesn't exist. The bar is: zero Critical, zero High, documented Medium/Low. Ship and iterate.

## Deliverable

Updated bug tracker (using the [Bug Tracker Template](../templates/bug-tracker.md) or equivalent) saved to your project's `docs/testing/bug-tracker.md`, plus a production readiness sign-off section. The tracker should include:

- Every issue from Steps 2-6 with severity, status, and resolution
- Deferred issues with rationale and target version
- Regression test results
- Production readiness checklist with all items checked
- Sign-off with date, build, and any caveats

## Definition of Done

- [ ] All issues from Steps 2-6 consolidated into a single bug tracker
- [ ] Every issue triaged with a severity assignment
- [ ] All Critical bugs fixed and verified
- [ ] All High bugs fixed and verified (or explicitly accepted with rationale)
- [ ] Medium and Low bugs documented as known issues with target versions
- [ ] Regression testing completed — re-ran affected test cases, full user flow walkthrough passed
- [ ] Code review completed on all bug fix changes
- [ ] Production readiness checklist passes — every item checked
- [ ] Bug tracker and sign-off committed to version control
- [ ] ROADMAP.md updated with deferred bugs in Backlog

## Tips

- **Fix Critical bugs before touching anything else.** It's tempting to knock out a quick Medium bug for momentum. Don't. A Critical bug that lingers is a launch that slips.
- **Time-box the sprint.** Set a deadline (1-2 weeks is typical for a solo developer). Bugs you can't fix in that window get re-triaged. If you're still finding Critical bugs after 2 weeks, the problem is upstream — go back to development.
- **One fix, one commit.** Don't batch multiple bug fixes into a single commit. Atomic commits make it easy to revert a fix that causes a regression without losing other work.
- **Write the test first if you can.** For bugs with clear reproduction steps, write a failing test before implementing the fix. This proves the fix works and prevents regression.
- **Review every fix before merging.** Bug fix code is written fast and focused on a symptom. A structured review (AI-assisted or manual) catches the shortcuts.
- **Update the beta build after fixing Critical bugs.** Don't wait until all bugs are fixed. Let beta testers verify Critical fixes early — they may find that the fix doesn't work in their environment.
- **The production readiness checklist is a hard gate, not a suggestion.** If any item fails, the app isn't ready. Shipping with a failed checklist item is how post-launch emergencies happen.
- **Keep the energy focused.** Bug fix sprints are a grind. Track your progress visually (bugs remaining graph, severity breakdown) so you can see the finish line approaching.
