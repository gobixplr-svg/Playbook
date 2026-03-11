# Test Plan — [App Name] v[Version]

## Metadata

| Field | Value |
| ---- | ---- |
| App Name | [app name] |
| Version | [version number] |
| Date | [YYYY-MM-DD] |
| Author | [name] |
| Phase | Phase 3: Testing & QA |

## Test Scope

### In Scope

- [Feature or flow 1]
- [Feature or flow 2]
- [Feature or flow 3]

### Out of Scope

- [Feature not being tested and why]
- [Feature deferred to future version]

## Device Matrix

Test on a representative set of devices covering screen sizes, performance tiers, and OS versions.

| Device | OS Version | Screen Size | Performance Tier | Priority |
| ---- | ---- | ---- | ---- | ---- |
| iPhone SE (3rd gen) | iOS [min supported] | 4.7" | Low | High |
| iPhone 15 | iOS [latest] | 6.1" | Mid | High |
| iPhone 15 Pro Max | iOS [latest] | 6.7" | High | Medium |
| iPad (10th gen) | iPadOS [latest] | 10.9" | Mid | [if iPad supported] |
| [additional device] | [version] | [size] | [tier] | [priority] |

**Minimum coverage:** Test every Critical and High priority test case on at least 2 devices (one small screen + one large screen). Test Medium and Low priority cases on at least 1 device.

## Test Cases by User Story

### [Story ID]: [Story Title]

| TC ID | Test Case | Steps | Expected Result | Priority | Status | Device Tested | Notes |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| TC-001 | [brief title] | 1. [step] 2. [step] 3. [step] | [expected outcome] | Critical / High / Medium / Low | Pass / Fail / Blocked / Not Run | [device] | [notes] |
| TC-002 | | | | | Not Run | | |
| TC-003 | | | | | Not Run | | |

### [Story ID]: [Story Title]

| TC ID | Test Case | Steps | Expected Result | Priority | Status | Device Tested | Notes |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| TC-004 | | | | | Not Run | | |
| TC-005 | | | | | Not Run | | |
| TC-006 | | | | | Not Run | | |

### Edge Cases and Error Handling

| TC ID | Test Case | Steps | Expected Result | Priority | Status | Device Tested | Notes |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| TC-E01 | No network connection | 1. Enable airplane mode 2. [action] | [graceful error or offline behavior] | High | Not Run | | |
| TC-E02 | Low storage | 1. Fill device storage 2. [action] | [graceful handling] | Medium | Not Run | | |
| TC-E03 | Background/foreground cycle | 1. Start [action] 2. Background app 3. Return | [state preserved] | High | Not Run | | |
| TC-E04 | Permission denied | 1. Deny [permission] 2. [action] | [graceful degradation] | High | Not Run | | |
| TC-E05 | | | | | Not Run | | |

## Risk Assessment

Identify areas of the app that are most likely to have issues and should receive extra testing attention.

| Risk Area | Likelihood | Impact | Mitigation |
| ---- | ---- | ---- | ---- |
| [e.g., payment flow] | [High / Medium / Low] | [High / Medium / Low] | [Extra test coverage, specific devices] |
| [e.g., offline sync] | | | |
| [e.g., deep links] | | | |
| [e.g., push notifications] | | | |

## Acceptance Criteria for Ship Ready

The app is ready to ship when all of the following are true:

- [ ] All Critical test cases pass on all priority devices
- [ ] All High test cases pass on at least 2 devices
- [ ] No known Critical bugs remain open
- [ ] No known High bugs remain open (or explicitly accepted)
- [ ] Performance benchmarks met (launch time, memory, battery)
- [ ] Security audit passed (Step 5)
- [ ] Beta testing completed with no new Critical bugs in last 2-3 builds
- [ ] [App-specific acceptance criteria]
- [ ] [App-specific acceptance criteria]

## Bug Tracking

Log all bugs found during test execution here (or link to the Bug Tracker template).

**Quick log (for tracking during test execution):**

| Bug ID | TC ID | Title | Severity | Status |
| ---- | ---- | ---- | ---- | ---- |
| BUG-001 | [TC that found it] | [brief description] | Critical / High / Medium / Low | Open |
| BUG-002 | | | | Open |
| BUG-003 | | | | Open |

**Full bug details:** See [Bug Tracker](bug-tracker.md) for complete reproduction steps and resolution tracking.

## Test Summary

Complete this section after all testing is done.

| Metric | Value |
| ---- | ---- |
| Total test cases | [number] |
| Passed | [number] |
| Failed | [number] |
| Blocked | [number] |
| Not run | [number] |
| Total bugs found | [number] |
| Critical bugs | [number] (target: 0 open) |
| High bugs | [number] (target: 0 open) |
| Ship ready | [Yes / No] |
| Sign-off date | [YYYY-MM-DD] |
