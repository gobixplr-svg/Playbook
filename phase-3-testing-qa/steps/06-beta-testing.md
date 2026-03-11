# Step 6: Beta Testing

## Purpose

Get the app into the hands of real users before launch. Beta testing validates that the app works in real-world conditions, surfaces usability issues that developer testing misses, and builds confidence that the app is ready to ship. This step covers TestFlight setup, tester management, feedback collection, and triage.

## Why This Matters

You have been looking at this app for weeks or months. You know every screen, every flow, every workaround. You are the worst person to judge whether the app makes sense to someone seeing it for the first time. Beta testers find the bugs you can't see — the confusing onboarding, the flow that makes sense to you but not to anyone else, the crash that only happens on an iPhone SE with low storage.

Beta testing also serves as a dress rehearsal for the App Store review process. TestFlight external review is a lighter version of the full review — if your app can't pass TestFlight review, it won't pass App Store review.

## Prerequisites

- Steps 1-5 completed (functional, platform, performance, and security testing done)
- No known Critical bugs open
- App builds and archives successfully
- Apple Developer account active with App Store Connect access
- App ID, bundle identifier, and provisioning profiles configured
- Privacy policy URL accessible

## Process

### 6.1 TestFlight Setup

Configure TestFlight for both internal and external beta distribution.

**App Store Connect setup:**

1. Create the app record in App Store Connect (if not already done)
2. Fill in required metadata: app name, bundle ID, SKU, primary language
3. Set the privacy policy URL
4. Configure the app's age rating

**Archive and upload:**

1. In Xcode: Product > Archive (or `xcodebuild archive`)
2. Distribute to App Store Connect via the Organizer
3. Wait for processing (usually 10-30 minutes)
4. Verify the build appears in TestFlight

**Compliance:**

- If the app uses encryption (HTTPS counts), answer the export compliance questions
- Most apps using only standard HTTPS can select "No" for custom encryption
- If unsure, check Apple's export compliance documentation

### 6.2 Internal Testing Group

Internal testers are members of your App Store Connect team. Builds are available to them immediately — no Apple review required.

**Setup:**

1. In App Store Connect > TestFlight > Internal Testing, create a group (e.g., "Core Team")
2. Add testers by Apple ID (up to 100 internal testers)
3. Enable automatic distribution so new builds go to testers without manual action

**Use internal testing for:**

- Quick smoke tests after each build
- Verifying fixes before pushing to external testers
- Testing on devices you don't own (if team members have different hardware)
- Your own daily-driver testing

**Internal testing minimum duration:** Use internal testing from the first uploadable build through launch. There is no formal waiting period — it's your fast iteration loop.

### 6.3 External Testing Group

External testers are people outside your App Store Connect team. Builds require Apple's TestFlight review before distribution (usually 24-48 hours for the first build, faster for subsequent builds).

**Setup:**

1. In App Store Connect > TestFlight > External Testing, create a group (e.g., "Beta Testers")
2. Add testers by email (up to 10,000 external testers)
3. Fill in the "What to Test" field — this is the first thing testers see
4. Fill in test information: contact email, privacy policy URL, beta description
5. Submit the build for TestFlight review

**Tester recruitment:**

| Source | Pros | Cons |
| ---- | ---- | ---- |
| Friends and family | Easy to recruit, will be honest | May not be target audience |
| Target audience contacts | Real use cases, relevant feedback | Harder to recruit |
| Online communities | Scale, diverse devices | Lower engagement, noise |
| Social media | Broad reach | Self-selection bias |

**Recommended minimum:** 5-10 external testers for a solo developer project. Quality of feedback matters more than quantity. Five engaged testers who report bugs are worth more than fifty who install and never open the app.

### 6.4 Beta Testing Plan

Define what you want testers to validate, so feedback is focused rather than vague.

**Testing plan structure:**

```text
Beta Testing Plan — [App Name] v[version]

Duration: [start date] to [end date] (minimum 1-2 weeks)

Focus Areas:
1. [Core user flow — e.g., "Create an account, complete onboarding, use main feature"]
2. [Secondary flow — e.g., "Edit profile, change settings"]
3. [Edge case — e.g., "Use offline, switch between WiFi and cellular"]
4. [Platform-specific — e.g., "Test on iPad, test on older iPhone models"]

NOT testing (save feedback for later):
- [Features intentionally incomplete or disabled]
- [Known issues listed below]

Known Issues:
- [Issue 1 — workaround if any]
- [Issue 2 — planned fix timeline]
```

### 6.5 Beta Tester Instructions

Write clear instructions that tell testers what to focus on and how to report issues. Most beta testers want to help but don't know how. Give them structure.

**Include in your tester instructions:**

1. **What to test** — specific flows and features, in priority order
2. **How to report bugs** — screenshots, steps to reproduce, device info
3. **How to share feedback** — in-app feedback (if available), email, or a shared form
4. **What not to worry about** — known issues, incomplete features, placeholder content
5. **Timeline** — when the beta ends and when you'll follow up

**Bug report template for testers:**

```text
What happened:
[One sentence description]

What I expected:
[What should have happened]

Steps:
1. [What I did first]
2. [What I did next]
3. [Where things went wrong]

Device: [iPhone model, iOS version]
Screenshot: [attached if possible]
```

**Distribution:** Send instructions when testers first get access. Don't rely on the TestFlight "What to Test" field alone — it's easy to skip.

### 6.6 Feedback Collection

Set up a system to collect, organize, and track feedback from all testers. Don't let feedback scatter across email, text messages, and verbal conversations.

**Feedback channels (pick one primary):**

| Channel | Best For | Setup Cost |
| ---- | ---- | ---- |
| Shared spreadsheet (Google Sheets, Notion) | Small groups, structured feedback | Low |
| GitHub Issues | Developer-friendly, links to code | Low |
| In-app feedback SDK (e.g., TestFlight screenshot feedback) | Lowest friction for testers | None (built into TestFlight) |
| Google Form | Structured responses, non-technical testers | Low |
| Dedicated Slack/Discord channel | Real-time discussion, community feel | Medium |

**TestFlight built-in feedback:**

- Testers can take a screenshot and submit feedback directly from the TestFlight app
- Feedback appears in App Store Connect > TestFlight > Feedback
- Includes device info, OS version, and app version automatically
- This is the lowest-friction option — emphasize it to testers

**Categorize feedback as it arrives:**

| Category | Examples |
| ---- | ---- |
| Bug | Crash, incorrect behavior, data loss |
| Usability | Confusing flow, unclear label, hard to find feature |
| Performance | Slow load, battery drain, high memory |
| Feature request | "It would be nice if..." |
| Positive | "This works great" (track these too — they validate decisions) |

### 6.7 Triage and Prioritization

Not everything found in beta needs to be fixed before launch. Triage feedback into actionable categories.

**Triage framework:**

| Priority | Criteria | Action |
| ---- | ---- | ---- |
| Fix before launch | Crashes, data loss, security issues, App Store rejection risks, core flow broken | Fix in Step 7 (Bug Fix Sprint) |
| Fix in v1.1 | Usability issues that have workarounds, minor visual bugs, edge case failures | Log in backlog with target version |
| Consider for future | Feature requests, nice-to-have improvements, cosmetic preferences | Add to Icebox in ROADMAP.md |
| Won't fix | Subjective preferences, out-of-scope requests, works-as-designed | Document rationale and close |

**Triage questions for each item:**

1. Does this block a core user flow? (If yes: fix before launch)
2. Does this affect data integrity or security? (If yes: fix before launch)
3. Will this cause App Store rejection? (If yes: fix before launch)
4. Can users complete the core flow despite this issue? (If yes: consider post-launch)
5. How many testers reported this independently? (Higher count = higher priority)

**Common trap:** Trying to fix everything testers mention. Beta testing always surfaces more issues than you can address before launch. The triage framework exists to prevent scope creep. Ship with known minor issues rather than delaying launch indefinitely.

### 6.8 Iterate on Builds

Beta testing is not a one-shot process. Upload new builds as you fix issues, and let testers verify the fixes.

**Iteration cadence:**

- Upload new builds as you fix batches of issues (not one build per fix)
- Update the "What to Test" field to highlight what changed
- Notify testers of new builds (TestFlight sends automatic notifications, but a personal message improves engagement)
- Ask testers to re-test specific flows after fixes

**When to stop iterating:**

- All "fix before launch" items are resolved
- Core flows work reliably across tester devices
- No new Critical or High bugs reported in the last 2-3 builds
- Tester feedback shifts from bugs to feature requests (signal that the core experience is solid)

## Anti-Patterns

- **Skipping external testers** — Internal testing catches technical bugs. External testing catches usability and real-world issues. They are not interchangeable.
- **No testing plan** — "Just use the app and tell me what you think" produces vague, unfocused feedback. Give testers specific flows to validate.
- **Too many testers, too little engagement** — Fifty testers who never open the app is worse than five who test thoroughly. Recruit for quality, not quantity.
- **Fixing everything immediately** — Reacting to every piece of feedback without triaging leads to scope creep and launch delays. Triage first, fix second.
- **One build, no iteration** — Uploading a single beta build and waiting for feedback misses the point. Beta testing is a feedback loop — fix, rebuild, re-test.
- **Ignoring positive feedback** — "Works great" is data too. It validates that a feature is ready and doesn't need more work.
- **Treating beta as the only testing** — Beta testing supplements Steps 1-5, it doesn't replace them. Don't ship a build to testers that hasn't passed functional and platform testing.

## Deliverable

Beta feedback log saved to your project's `docs/testing/beta-feedback-log.md`. The log should include:

- List of all feedback items with category and severity
- Triage decision for each item (fix before launch / v1.1 / future / won't fix)
- Summary of tester participation (how many testers, how many active, devices covered)
- List of builds uploaded with key changes in each
- Final "ready to ship" determination based on feedback trends

## Definition of Done

- [ ] TestFlight configured with both internal and external testing groups
- [ ] At least one build uploaded and distributed to external testers
- [ ] Beta testing ran for at least 1-2 weeks
- [ ] Tester instructions distributed with clear focus areas
- [ ] All feedback collected and categorized
- [ ] Every feedback item triaged (fix before launch / post-launch / won't fix)
- [ ] "Fix before launch" items documented for Step 7 (Bug Fix Sprint)
- [ ] No new Critical bugs in the last 2-3 builds
- [ ] Beta feedback log committed to version control

## Tips

- **Start internal testing as early as possible.** Don't wait until the app is "ready." Internal TestFlight testing can begin as soon as the app archives — even with incomplete features. The sooner you start, the more build-upload muscle memory you develop.
- **Write the "What to Test" field carefully.** It's the first thing testers see when they open a new build. Make it specific: "Please test the new onboarding flow — sign up with email, complete the profile setup, and try creating your first [item]."
- **Follow up personally with testers.** A direct message ("Hey, did the crash on the settings screen happen again with the new build?") gets 10x more engagement than a broadcast notification.
- **Track which testers are active.** App Store Connect shows who installed each build. If a tester hasn't installed in a week, reach out or remove them and recruit someone engaged.
- **Use TestFlight's built-in screenshot feedback.** It's the lowest-friction way for testers to report issues. Mention it explicitly in your tester instructions — many people don't know it exists.
- **Don't beta test for longer than 3-4 weeks.** Diminishing returns set in fast. If you're still finding Critical bugs after 3 weeks, the problem isn't insufficient testing — it's insufficient development. Go back to Step 7.
- **Keep a "ready to ship" bar in mind.** The goal is not zero bugs. The goal is: core flows work, no data loss, no crashes in normal usage, no App Store rejection risks. Ship, then iterate.
