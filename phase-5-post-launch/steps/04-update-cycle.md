# Step 4: Update Cycle

## Purpose

Establish a regular release cadence so updates ship predictably and reliably. A consistent update cycle keeps users engaged, maintains App Store visibility, and prevents the "big bang release" anti-pattern where months of work ships in a single risky update.

## Process

### 4.1 Release Cadence

Pick a cadence and stick to it. Consistency matters more than frequency.

**Recommended cadences:**

| Cadence | Best For | Tradeoff |
| ------- | -------- | -------- |
| Weekly | Apps in early post-launch with many bugs | High overhead, fast iteration |
| Every 2 weeks | Active apps with steady feature development | Good balance for solo devs |
| Monthly | Stable apps with fewer changes | Lower overhead, slower feedback loop |

**Starting recommendation:** Every 2-4 weeks for the first 3 months post-launch, then adjust based on how much is changing. If you're only shipping 1-2 small fixes, shift to monthly. If you're actively iterating, stay at 2 weeks.

**Release day tip:** Pick a consistent day (e.g., Tuesday). Avoid Fridays — if something breaks, you're debugging over the weekend. Avoid Mondays — App Store review queues are longest early in the week.

### 4.2 Version Numbering

Use semantic versioning to communicate the nature of each update.

**Format: MAJOR.MINOR.PATCH**

| Component | When to Increment | Example |
| --------- | ----------------- | ------- |
| MAJOR | Breaking changes, major redesigns, significant new direction | 1.0 → 2.0 |
| MINOR | New features, meaningful improvements | 1.0 → 1.1 |
| PATCH | Bug fixes, small tweaks, performance improvements | 1.0.0 → 1.0.1 |

**Build number:** Increment the build number for every submission to App Store Connect, even if the version number stays the same (resubmissions after rejection). Xcode can auto-increment this.

**Practical version strategy:**

- Launch at 1.0.0
- Bug fix updates: 1.0.1, 1.0.2, 1.0.3
- First feature update: 1.1.0 (resets patch to 0)
- Major overhaul or redesign: 2.0.0

### 4.3 What's New Copy

The "What's New" section in the App Store is marketing, not a changelog. Write it for users, not developers.

**Guidelines:**

- **Lead with benefits, not technical details.** "Faster app launch" beats "Optimized Core Data fetch requests."
- **Mention specific features by name.** "Export your data as CSV" is concrete and findable.
- **Keep it scannable.** 3-5 bullet points. No paragraphs.
- **Include a bug fix line if relevant.** "Fixed an issue where..." shows you're responsive to problems.

**Good example:**

```
What's New in 1.2:
- Export your data as CSV — tap the share button on any report
- Faster app launch (up to 40% quicker on older devices)
- New dark mode option in Settings
- Fixed an issue where notifications didn't appear on iOS 17.2
```

**Bad example:**

```
Bug fixes and performance improvements.
```

The bad example tells users nothing and signals that you're not invested in communicating with them.

### 4.4 Regression Testing

Every release needs a testing pass. This is an abbreviated version of Phase 3 — not a full QA cycle.

**Pre-release checklist:**

- [ ] **Smoke test core flows.** Walk through the 3-5 most important user journeys. Do they still work?
- [ ] **Test new features.** Verify the new functionality works as designed, including edge cases.
- [ ] **Test on minimum supported OS.** If you support iOS 16+, test on iOS 16, not just the latest.
- [ ] **Test on a physical device.** Simulator misses performance issues, permission prompts, and real-world network conditions.
- [ ] **Run the test suite.** All unit and integration tests pass. No skipped tests without documented reasons.
- [ ] **Check for regressions in areas adjacent to changes.** If you changed the data layer, test features that read from it.

**Time budget:** 1-2 hours for a patch release. Half a day for a minor release. Full Phase 3 process for a major release.

### 4.5 App Store Submission

The submission process is the same as Phase 4 Step 4, but faster with practice. Here's the abbreviated checklist.

**Pre-submission:**

- [ ] Version and build number updated
- [ ] What's New copy written
- [ ] Screenshots updated (only if UI changed significantly)
- [ ] App review information updated (if login credentials or instructions changed)
- [ ] Privacy nutrition labels still accurate (if data collection changed)

**Submission:**

1. Archive the build in Xcode (Product > Archive)
2. Upload to App Store Connect via Xcode Organizer
3. Select the build in App Store Connect
4. Fill in What's New
5. Submit for review

**Post-submission:**

- Monitor the review status (typically 24-48 hours)
- If rejected, read the rejection reason carefully, fix the issue, and resubmit
- Once approved, either release immediately or schedule the release

**Tip:** Enable automatic release after approval for patch releases. Use manual release for feature updates so you can time the launch.

## Deliverable

A documented release schedule with version numbering conventions and a pre-release checklist. Save to your project's `docs/post-launch/update-cycle.md`.

## Definition of Done

- [ ] Release cadence decided and documented
- [ ] Version numbering convention established
- [ ] What's New copy guidelines documented
- [ ] Pre-release testing checklist created
- [ ] First post-launch update submitted successfully

## Tips

- **Ship small, ship often.** A 1.0.1 with two bug fixes one week after launch builds more trust than silence for a month followed by a big 1.1.
- **Don't let perfection delay updates.** If you have three features planned for 1.1 but two are done, ship the two. The third can go in 1.1.1 or 1.2.
- **Keep a release notes archive.** Copy each What's New into a `CHANGELOG.md` in your repo. This history is valuable for understanding what shipped when.
- **Watch crash rates after each release.** A spike in crashes within 24 hours of release means the update introduced a regression. Have a plan for emergency hotfixes.
- **Automate what you can.** Xcode Cloud or CI/CD tools like Fastlane can automate builds, testing, and uploads. The overhead of setting this up pays off after 3-4 releases.
