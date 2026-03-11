# Step 6: Launch Day Monitoring

## Purpose

Monitor your app's first 48 hours in production to catch critical issues early, respond to user feedback, and understand real-world usage. By the end of this step, you have a monitoring dashboard, a hot-fix preparation plan, and actionable data from your first users.

## Why This Matters

The first 48 hours after launch are the most consequential period in your app's life. This is when you discover what testing missed — because real users do things no test plan anticipates. A crash that affects 5% of users goes unnoticed in beta with 20 testers, but hits hundreds when thousands download on launch day.

Your App Store rating starts forming immediately. Early one-star reviews from a preventable crash are hard to overcome. Catching and fixing issues within hours — not days — protects your rating and first impression.

## Prerequisites

- App is live on the App Store (approved and released)
- Launch marketing is executing (Step 5)
- Crash reporting tool is integrated (Xcode Organizer at minimum, or third-party like Firebase Crashlytics, Sentry)
- Analytics tool is integrated and verified (events are logging correctly)
- You are available to monitor for the first 48 hours (do not launch on a day you cannot watch)

## Process

### 6.1 Set Up Your Monitoring Dashboard

Before launch day (ideally T-1), have all monitoring tools open and verified.

**Essential monitoring channels:**

| Channel | Tool | What to watch | Check frequency |
| ------- | ---- | ------------- | --------------- |
| App Store status | App Store Connect | Release status, processing state | Every 30 min until live |
| Crash reports | Xcode Organizer / Crashlytics / Sentry | Crash-free rate, top crashes | Every 1-2 hours |
| App Store reviews | App Store Connect | New reviews and ratings | Every 2-3 hours |
| Downloads | App Store Connect (Analytics) | Download count, sources, territories | Every 4-6 hours |
| Analytics | Your analytics tool | Active users, feature usage, retention triggers | Every 4-6 hours |
| Social mentions | Twitter/X search, Reddit, Product Hunt | User feedback, bug reports, praise | Continuously |
| Support email | Your support inbox | Direct bug reports, questions | Every 1-2 hours |

**Set up notifications:**

- Enable push notifications from App Store Connect (Settings > Notifications)
- Enable crash alerts from your crash reporting tool (most support email/Slack alerts for new crash types)
- Set up Google Alerts for your app name (delayed, but catches blog mentions)

### 6.2 Monitor App Store Connect

**Release verification:**

Once you release the app (manually or it auto-releases), verify:

1. The app appears in the App Store (search for it by name — it can take 15-60 minutes to propagate)
2. The App Store link works (test on a device, not just the web)
3. Screenshots, description, and metadata display correctly
4. The price is correct
5. In-app purchases are available (if applicable — test a sandbox purchase)
6. The correct version and build number are shown

**Sales and downloads:**

App Store Connect Analytics updates with some delay (typically a few hours). On launch day, the numbers you see are not real-time. Do not obsess over hourly numbers — check 2-3 times on day one, then settle into a daily cadence.

**What to look for:**

- Download count — is it trending up or flat?
- Download sources — where are users finding you? (App Store Search, App Store Browse, Web Referrer, App Referrer)
- Conversion rate — impressions to downloads. If many people see your listing but few download, your screenshots or description may need work.
- Territory breakdown — are downloads coming from expected markets?

### 6.3 Watch Crash Reports

Crash monitoring is the highest-priority channel on launch day. A crashing app gets one-star reviews and ruins your first impression.

**Xcode Organizer (built-in):**

- Open Xcode > Window > Organizer > Crashes
- Xcode shows crash logs from App Store users (with a delay of up to 24 hours)
- Symbolicated crash logs show the exact line of code that crashed

**Third-party crash tools (recommended):**

Firebase Crashlytics, Sentry, or Bugsnag provide faster, more detailed crash reporting:

- Near-real-time crash alerts
- Crash-free rate (target: 99.5%+ in the first 48 hours)
- Grouping by crash type and affected device/OS version
- Affected user count per crash

**What demands immediate action:**

| Severity | Criteria | Response |
| -------- | -------- | -------- |
| Critical | Crash-free rate below 99% or crash on launch/first screen | Ship hot-fix immediately (Section 6.6) |
| High | Crash-free rate 99-99.5% or crash in core user flow | Begin hot-fix, ship within 24 hours |
| Medium | Crash-free rate above 99.5%, crash in edge case | Document, fix in next version |
| Low | Rare crash in non-critical path | Track, batch with future updates |

### 6.4 Monitor App Store Reviews

Early reviews have disproportionate influence. Your average rating with 5 reviews can swing wildly with each new review.

**How to check:**

- App Store Connect > My Apps > [Your App] > Ratings and Reviews
- Or use the App Store Connect iOS app for push notifications on new reviews

**Responding to reviews:**

- **Respond to every review in the first week.** This shows users (and Apple) that you are an active developer.
- **Positive reviews:** Thank the user genuinely. A short "Thank you! Glad you are enjoying it." is enough.
- **Constructive criticism:** Acknowledge the feedback, explain if a fix is coming. "Great feedback — we are adding this in the next update" (only if true).
- **Bug reports in reviews:** Respond with empathy and ask them to contact support for details. "Sorry about this! We are investigating. If you email us at [support email], we can help directly."
- **One-star reviews from a crash:** Respond immediately, acknowledge the issue, and mention the fix is coming. If you ship a hot-fix, update your response.

**Do not:**

- Argue with reviewers
- Make excuses
- Ask users to change their rating (Apple prohibits incentivized reviews)
- Ignore negative reviews — silence looks like abandonment

### 6.5 Track Analytics

If you integrated analytics during Phase 2/3, now is when you see real data for the first time.

**Key metrics to watch on launch day:**

| Metric | What it tells you | Healthy range (day 1) |
| ------ | ----------------- | --------------------- |
| Daily Active Users (DAU) | How many people opened the app today | Varies — compare to downloads |
| Session duration | How long users spend per session | Depends on app type (2-5 min for utility, 10+ min for content) |
| Feature usage | Which features are actually used | Core feature should show 60%+ usage among active users |
| Onboarding completion | How many users finish onboarding | Target 70%+ |
| Retention (D1) | How many users return the next day | 25-40% is good for most categories |
| Conversion to premium (if freemium) | How many users hit the paywall, how many convert | 2-5% conversion is typical |

**What to look for:**

- **Drop-off points:** Where do users stop? If 80% start onboarding but only 40% finish, there is a friction point.
- **Unexpected feature usage:** Are users using a feature you thought was secondary more than your "core" feature? This is valuable signal.
- **Device/OS distribution:** Are crashes concentrated on a specific device or iOS version?

### 6.6 Hot-Fix Preparation

Have a plan for shipping an emergency fix before you need one. On launch day, speed matters.

**Pre-launch preparation:**

- [ ] Keep your development environment ready (do not start a major refactor on launch day)
- [ ] Have a release branch strategy — know how to branch, fix, and submit without disrupting main development
- [ ] Know your fastest path to submission:
  - Xcode Archive > Upload > App Store Connect > Submit for Review
  - Request an expedited review if the fix is critical (Apple grants these for crash fixes)

**Hot-fix process:**

1. **Identify the issue** from crash reports or user feedback
2. **Reproduce it** locally if possible
3. **Fix it** with the minimum necessary change — do not bundle other changes
4. **Test the fix** on the affected device/OS combination
5. **Increment the build number** (not the version number — a hot-fix is the same version)
6. **Archive and upload** the new build
7. **Submit for review** with a note explaining this is a critical bug fix
8. **Request expedited review** if the crash affects a large percentage of users (App Store Connect > Your App > Submit for Review > Request Expedited Review)
9. **Update the "What's New"** text to mention the fix

**Expedited review expectations:**

- Apple grants expedited reviews for legitimate critical issues (crashes, security vulnerabilities, data loss)
- Turnaround is typically 24 hours or less
- Do not abuse this for non-critical updates — Apple tracks your request history

### 6.7 The First 48 Hours

The first 48 hours set the trajectory for your app's early life.

**Hour 0-6: Verify and promote**

- Confirm the app is live and accessible worldwide
- Execute launch marketing plan (Step 5)
- Monitor crash reports every hour
- Respond to social media mentions and Product Hunt comments

**Hour 6-24: Monitor and engage**

- Check crash-free rate — if below 99%, prioritize a hot-fix
- Respond to all App Store reviews
- Check download numbers (understanding they are delayed)
- Engage with community posts and press coverage
- Monitor support email

**Hour 24-48: Analyze and adjust**

- Review Day 1 analytics (retention, feature usage, drop-off points)
- Compile crash report summary — any patterns?
- Review Day 1 download numbers and sources
- Draft a "Day 1 learnings" summary for yourself
- Decide: is a hot-fix needed, or is the next update a normal release?
- Thank your supporters, beta testers, and anyone who shared

**After 48 hours:**

- Shift from continuous monitoring to daily check-ins
- Start planning your first feature update (address feedback from launch day reviews)
- Continue responding to App Store reviews (daily for the first week, then 2-3 times per week)
- Write a retrospective note on what you would do differently for your next launch

## Deliverable

Launch day monitoring dashboard established and actively used: crash reporting verified, analytics tracking confirmed, App Store reviews monitored, and a hot-fix process ready if needed. Summary of first 48 hours with key metrics, issues encountered, and user feedback.

## Definition of Done

- [ ] Monitoring dashboard set up with all channels (crash, analytics, reviews, social)
- [ ] App verified live and accessible on the App Store
- [ ] Crash reporting confirmed working (receiving data from real users)
- [ ] Analytics confirmed working (events logging from real users)
- [ ] App Store reviews monitored and responded to
- [ ] Download numbers and sources reviewed
- [ ] Hot-fix process documented and development environment ready
- [ ] First 48-hour summary written with key metrics and learnings
- [ ] Critical issues (if any) addressed with hot-fix or documented for next update

## Tips

- **Do not launch on a day you cannot monitor.** If you have a day job, launch on a day you can check your phone every hour. Friday launches lose Saturday and Sunday for response time.
- **The crash-free rate is your most important number.** Everything else — downloads, reviews, engagement — depends on the app not crashing. If it is below 99%, drop everything and fix it.
- **Respond to every review in the first week.** Even a simple "Thanks!" signals to future users that the developer is active and responsive. Developer responses show publicly on the listing.
- **Do not ship a hot-fix for cosmetic issues.** Hot-fixes are for crashes, data loss, and security issues. A misaligned button can wait for the next regular update. Every submission consumes review team time.
- **Write down what you learn.** Launch day generates more real-user insight than months of beta testing. Capture it while it is fresh — this informs your v1.1 priorities and your next app's Phase 0.
- **It is normal to feel anxious.** Watching real users interact with something you built is nerve-wracking. Most launches go smoother than expected. Trust your testing and be ready to respond.
