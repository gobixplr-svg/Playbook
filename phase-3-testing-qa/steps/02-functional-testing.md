# Step 2: Functional Testing

## Purpose

Execute the test plan against the running app — verifying every user story works as specified, every state renders correctly, and every edge case is handled. By the end of this step, you have a definitive pass/fail result for every test case and a categorized bug list with reproduction steps.

## Why This Matters

Phase 2 testing verified stories in isolation as they were built. Functional testing verifies the entire app as a product. Features that passed individually can break when combined — a navigation change in one sprint can silently break a flow from an earlier sprint. This is your first complete sweep of the shipped experience.

Functional testing also catches the category of bugs that compilation and unit tests cannot: empty states with no message, error states that show raw error codes, offline behavior that silently fails, and accessibility flows that dead-end. These are the bugs users hit on day one.

## Prerequisites

- Test Plan (Step 1) completed with test cases mapped to acceptance criteria
- App builds and runs on the primary test device (iPhone 16 / iOS 18 or equivalent baseline)
- Test data prepared — accounts, seed data, or mock configurations needed to reach every state
- Bug tracking method chosen (GitHub Issues, a markdown file, a spreadsheet — anything searchable)

## Process

### 2.1 Set Up Test Environment

Before executing test cases, prepare a clean and repeatable starting point.

1. **Fresh install** — Delete the app from the test device/simulator and reinstall. This catches first-launch bugs that persist-state testing misses.
2. **Test accounts** — Create dedicated test accounts (not your development account). If the app uses authentication, have at least:
   - A brand-new account (empty state testing)
   - An account with populated data (normal use testing)
   - An account at limits (edge case testing — max items, long strings, etc.)
3. **Network control** — Know how to toggle between Wi-Fi, cellular, and airplane mode. Use Network Link Conditioner (in Xcode Developer Settings) to simulate slow connections.
4. **Reset between tests** — Define how to reset state between test runs. For some tests, you need a fresh start. For others, you need accumulated state.

### 2.2 Execute Happy Path Tests

Work through every test case marked as Critical or High priority, following the happy path first.

**For each test case:**

```text
Test ID: TC-01
Story: US-01 — View route list
AC: Given routes exist, When the user opens the app, Then the route list displays.

Steps:
1. Launch app with populated test account
2. Verify route list screen appears
3. Verify each route shows name, distance, and thumbnail
4. Verify list scrolls smoothly

Result: PASS / FAIL
Notes: [Any observations]
```

**Record results immediately.** Don't batch — test, record, move on. If you defer recording, you'll forget details.

**Work through flows, not screens.** Test the user journey, not individual views in isolation:

1. Launch → Onboarding → Home
2. Home → Feature → Result → Back to Home
3. Create → Edit → Save → View saved item
4. Error → Recovery → Retry → Success

### 2.3 Test All Application States

Every screen in the app can exist in multiple states. Test each one explicitly.

| State | What to Verify | How to Trigger |
| ----- | -------------- | -------------- |
| **Empty** | Meaningful message, call-to-action, no blank screen | New account with no data |
| **Loading** | Loading indicator visible, no frozen UI, not instant (test with slow network) | Network Link Conditioner set to "Very Bad Network" |
| **Populated** | Data displays correctly, layout handles variable content lengths | Account with realistic data |
| **Error** | User-friendly message, retry option, no raw error codes or stack traces | Airplane mode, invalid data, expired session |
| **Offline** | Graceful degradation — cached data shown, or clear "offline" indicator | Airplane mode after initial data load |
| **Partial** | Handles incomplete data without crashing (some fields nil, some images missing) | Partially populated test data |

**Common misses:**

- Empty state shows a blank white screen instead of a helpful message
- Loading state is so fast on Wi-Fi you never see it — test on slow network
- Error messages expose technical details ("Error 500: Internal Server Error" instead of "Something went wrong. Try again.")
- Offline mode crashes instead of showing cached data or an offline indicator

### 2.4 Test Edge Cases

Edge cases are the unusual-but-possible scenarios that break assumptions in your code.

**Text edge cases:**

- Very long text (200+ character name, multi-paragraph description)
- Special characters: emoji, RTL text, HTML tags, quotes, ampersands
- Empty strings vs. nil — do both behave correctly?
- Single character input
- Text with only whitespace

**Interaction edge cases:**

- Rapid tapping — tap a button 5 times quickly. Does it trigger 5 actions?
- Double submit — tap "Save" twice. Does it create duplicate records?
- Back button during async operation — start a save, hit back before it completes
- Background/foreground — start an action, switch to another app, come back
- Kill and restart — force-quit during an operation, relaunch. Is state consistent?
- Swipe-to-go-back during a transition or modal presentation

**Data edge cases:**

- Zero items, one item, many items (100+)
- Items at the boundary of pagination (if paginated)
- Maximum allowed values (character limits, item counts)
- Deleting the last item — does the view handle returning to empty state?
- Concurrent modification — edit on device while data changes on server (if applicable)

### 2.5 Test Accessibility

Accessibility is not optional — it is an App Store guideline and a legal requirement in many jurisdictions. Test at minimum:

**VoiceOver:**

1. Enable VoiceOver (Settings → Accessibility → VoiceOver)
2. Navigate through every primary flow using only swipe gestures
3. Verify every interactive element has a meaningful label (not "button" or "image")
4. Verify the reading order matches the visual layout
5. Verify custom controls (sliders, toggles, pickers) announce their role and value

**Dynamic Type:**

1. Set text size to the largest non-accessibility size (Settings → Display & Brightness → Text Size)
2. Set text size to the largest accessibility size (Settings → Accessibility → Larger Text)
3. Verify text does not truncate or overlap at both sizes
4. Verify layouts adapt — stacked layouts may need to become scrollable
5. Verify tap targets remain at least 44x44 points

**Color contrast:**

1. Verify text is readable against its background in both light and dark mode
2. Verify interactive elements are distinguishable from non-interactive elements
3. Verify information is not conveyed by color alone (e.g., red/green status without an icon or label)

**Reduce Motion:**

1. Enable Reduce Motion (Settings → Accessibility → Motion → Reduce Motion)
2. Verify animations are replaced with fades or removed entirely
3. Verify no auto-playing animations or parallax effects remain

### 2.6 Document Bugs

When a test case fails, document the bug immediately with enough detail for someone (including future-you) to reproduce it.

**Bug report template:**

```text
Bug ID: BUG-001
Title: Route list shows blank screen when no routes exist
Severity: High
Test Case: TC-02
Steps to Reproduce:
  1. Create a new account with no routes
  2. Launch the app
  3. Navigate to the Routes tab
Expected: Empty state message with "Create your first route" call-to-action
Actual: Blank white screen with no text or buttons
Device: iPhone 16, iOS 18.2
Screenshot: [attached]
```

**Severity levels:**

| Severity | Definition | Action |
| -------- | ---------- | ------ |
| **Critical** | App crashes, data loss, security vulnerability, core flow blocked | Must fix before ship |
| **High** | Feature broken, bad user experience on primary flow, accessibility failure | Must fix before ship |
| **Medium** | Feature partially broken, workaround exists, cosmetic issue on primary flow | Fix if time permits |
| **Low** | Minor cosmetic issue, edge case with minimal impact | Log for future release |

### 2.7 Categorize and Triage

After completing all test cases, categorize the bug list:

1. **Group by severity** — Critical and High bugs go to the fix sprint (Step 7). Medium and Low go to the backlog.
2. **Group by area** — Cluster bugs by feature/epic. If one area has many bugs, it may indicate a systemic issue worth investigating rather than fixing individually.
3. **Identify patterns** — Multiple bugs with the same root cause (e.g., all empty states are broken) should be fixed as one issue, not individually.
4. **Estimate fix effort** — For each Critical and High bug, estimate whether it's a quick fix (< 1 hour) or a deeper issue (requires investigation).

## Anti-Patterns

- **Testing your own assumptions** — You built the feature, so you instinctively use it the way it was designed. Consciously try to break it. Use wrong inputs. Navigate backwards. Interrupt flows.
- **Skipping empty and error states** — These are the states users hit most on day one (before they have data) and whenever something goes wrong. They're also the states developers test least because they require deliberate setup.
- **Logging bugs without reproduction steps** — "The route list is broken sometimes" is not actionable. Specific steps, expected vs. actual, and device info make the difference between a fixable bug and a mystery.
- **Fixing bugs during testing** — When you find a bug, the temptation is to fix it immediately. Resist. Document it, finish the test pass, then fix in a batch. Fixing mid-test changes the state of the app and invalidates subsequent tests.
- **Testing only on the simulator** — The simulator runs on your Mac's hardware with your Mac's memory and CPU. It does not replicate real device performance, permissions flows, or background behavior. Functional testing should happen on a real device. (Full device matrix testing is Step 3.)
- **Ignoring accessibility** — "I'll add VoiceOver support later" means never. Test it now, file bugs for failures, fix them in the bug fix sprint. Accessibility issues are bugs, not nice-to-haves.

## Deliverable

**Test Results and Bug List** saved to your project's `docs/testing/` directory:

- `test-results.md` — Pass/fail status for every test case, with notes on failures
- `bug-list.md` — All bugs found, with severity, reproduction steps, and triage status

## Definition of Done

- [ ] All Critical priority test cases executed and results recorded
- [ ] All High priority test cases executed and results recorded
- [ ] Medium and Low priority test cases executed (or explicitly deferred with rationale)
- [ ] All five application states tested for every primary screen (empty, loading, populated, error, offline)
- [ ] Edge cases tested (long text, rapid tapping, background/foreground, kill/restart)
- [ ] VoiceOver navigation tested on all primary flows
- [ ] Dynamic Type tested at default and largest sizes
- [ ] Color contrast verified in light and dark mode
- [ ] Reduce Motion verified
- [ ] All bugs documented with severity, reproduction steps, and device info
- [ ] Bug list categorized and triaged (Critical/High vs. Medium/Low)
- [ ] Test results and bug list saved to `docs/testing/`

## Tips

- **Test on a real device from the start.** Simulator testing is fine for development, but functional testing should happen on hardware. Plug in your phone, build to device, and test the real experience.
- **Use screen recording.** iOS has built-in screen recording (Control Center). Record your test sessions — when you find a bug, you have instant reproduction evidence without re-triggering it.
- **Test as a new user first.** Delete the app, reinstall, and go through the entire first-time experience. This is what every user will do on launch day. If onboarding is broken, nothing else matters.
- **Batch your state testing.** Instead of testing empty/loading/error for each screen individually, test all empty states in one pass, all error states in another. This is faster and catches inconsistencies between screens.
- **Keep a "paper cuts" list.** Things that aren't bugs but feel wrong — awkward wording, confusing navigation, too many taps to complete an action. These are UX improvements for the next release.
- **AI tools can help with test data.** Ask an AI assistant to generate edge case test data: strings with special characters, extremely long text, Unicode edge cases, boundary values. This is tedious work that AI handles well.
