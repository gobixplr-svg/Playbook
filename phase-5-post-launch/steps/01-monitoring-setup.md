# Step 1: Monitoring Setup

## Purpose

Establish the dashboards, alerts, and review cadence that turn raw telemetry into actionable insight. Without monitoring, you're flying blind — problems surface as one-star reviews instead of alerts you can fix proactively.

## Process

### 1.1 Crash Reporting

Set up crash reporting so you know about failures before users complain.

**Primary: Xcode Organizer**

- Open Xcode > Window > Organizer > Crashes
- Xcode automatically collects crash logs from users who have opted into sharing diagnostics
- Review crash clusters — Xcode groups identical crashes and shows affected device/OS combinations
- Symbolicate crashes to get readable stack traces

**Optional: Third-Party Services**

If you need faster alerts, more granularity, or cross-platform coverage, add a dedicated service:

| Service | Strengths | Cost |
| ------- | --------- | ---- |
| Firebase Crashlytics | Free, real-time, integrates with Firebase Analytics | Free |
| Sentry | Detailed breadcrumbs, release tracking, performance monitoring | Free tier available |
| Bugsnag | Stability scoring, smart grouping | Free tier available |

Pick one. Don't run multiple crash reporters — they interfere with each other and create duplicate noise.

**Setup checklist:**

- [ ] Crash reporting SDK integrated and tested (force a test crash)
- [ ] dSYM upload configured (automatic via build phase or CI)
- [ ] Crash alerts enabled (email or Slack notification on new crash cluster)

### 1.2 Analytics Dashboard

Track the metrics that tell you whether the app is healthy and growing. Focus on four categories.

**Key metrics:**

| Category | Metric | Why It Matters |
| -------- | ------ | -------------- |
| Engagement | DAU / MAU | Are people using the app regularly? |
| Engagement | Session length | Are sessions meaningful or bounce-outs? |
| Retention | D1 / D7 / D30 | Are users coming back? |
| Revenue | MRR / ARPU | Is the business model working? |
| Quality | Crash-free rate | Is the app stable? Target >99.5% |
| Quality | App Store rating | How do users perceive quality? |

**Where to track:**

- **App Store Connect > App Analytics** — Downloads, impressions, conversion rate, retention (free, no SDK needed)
- **Firebase Analytics / Mixpanel / PostHog** — Custom events, funnels, user properties (requires SDK)
- **RevenueCat / StoreKit 2 dashboard** — Subscription metrics, trial conversions, churn

Start with App Store Connect. Add a custom analytics service only when you need event-level granularity that App Store Connect doesn't provide.

### 1.3 App Store Review Monitoring

Reviews are unstructured feedback at scale. Monitor them systematically.

- **App Store Connect > Ratings and Reviews** — Check weekly at minimum
- **Set up email notifications** for new reviews (App Store Connect > Users and Access > Notifications)
- **Respond to negative reviews** — Acknowledge the issue, say what you're doing about it. Even a brief response improves perception.
- **Track review sentiment over time** — A sudden drop in average rating after an update signals a regression

### 1.4 Alert Thresholds

Alerts prevent you from discovering problems days late. Set thresholds for automatic notification.

| Alert | Threshold | Channel |
| ----- | --------- | ------- |
| Crash rate spike | >1% crash-free sessions drop in 24h | Email / Slack |
| Rating drop | Average drops below 4.0 (or drops 0.5+ in a week) | Email |
| Revenue anomaly | Daily revenue drops >30% vs 7-day average | Email |
| Error rate | Backend error rate >5% for 15+ minutes | Email / Slack |

Adjust thresholds based on your app's baseline. An app with 100 DAU has noisier metrics than one with 10,000. Don't set alerts so sensitive that you ignore them.

### 1.5 Weekly Review Cadence

Monitoring only works if you actually look at it. Block 30 minutes weekly.

**Weekly review checklist:**

1. **Crashes** — Any new crash clusters? Any increasing in frequency?
2. **Reviews** — Read all new reviews. Flag bugs and feature requests.
3. **Metrics** — DAU/MAU trending up, flat, or down? Any anomalies?
4. **Revenue** — On track? Trial conversion rate stable?
5. **Action items** — File bugs, add feature requests to backlog, note anything that needs investigation

Keep a running log of weekly observations. Patterns emerge over weeks, not days.

## Deliverable

A monitoring dashboard with alerts configured and a documented weekly review process. Save your monitoring configuration notes to your project's `docs/post-launch/monitoring-setup.md`.

## Definition of Done

- [ ] Crash reporting is active and verified (test crash sent and received)
- [ ] Analytics dashboard tracks DAU/MAU, retention, and revenue metrics
- [ ] App Store review notifications are enabled
- [ ] Alert thresholds are configured for crash rate, rating, and revenue
- [ ] Weekly review cadence is scheduled and documented

## Tips

- **Start simple.** App Store Connect analytics plus Xcode Organizer covers most solo devs. Don't add Firebase, Mixpanel, and Sentry on day one — add tools when you have specific questions they answer.
- **Crash-free rate is your north star quality metric.** Below 99% means users are hitting crashes regularly. Below 98% is a hair-on-fire situation.
- **Reviews are qualitative analytics.** A single detailed review often reveals more than a dashboard of numbers. Read them.
- **Don't alert on everything.** Alert fatigue is real. Only alert on things that require action within 24 hours. Everything else goes in the weekly review.
- **Log your weekly reviews.** Even a few bullet points per week creates a valuable history when you need to understand trends or debug a gradual decline.
