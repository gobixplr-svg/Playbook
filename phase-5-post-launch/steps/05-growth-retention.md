# Step 5: Growth & Retention

## Purpose

Move from "the app is live" to "the app is growing." This step covers the strategies and metrics for acquiring new users, keeping existing users engaged, and re-engaging lapsed users. Growth without retention is a leaky bucket — this step addresses both sides.

## Process

### 5.1 App Store Optimization (ASO)

ASO is SEO for the App Store. It determines whether users find your app when searching for what it does.

**Keyword research:**

- Start with 5-10 terms users would type when looking for an app like yours
- Check autocomplete suggestions in the App Store search bar for related terms
- Use App Store Connect's "Sources" data to see which search terms are already driving impressions
- Prioritize keywords by: relevance > search volume > competition

**Metadata optimization:**

| Element | Character Limit | Best Practice |
| ------- | --------------- | ------------- |
| App Name | 30 characters | Include your top keyword naturally. Don't keyword-stuff. |
| Subtitle | 30 characters | Complement the name — add a benefit or secondary keyword |
| Keyword field | 100 characters | Comma-separated, no spaces after commas, no duplicates of name/subtitle words |
| Description | 4000 characters | First 1-3 sentences matter most (visible before "more" tap). Lead with value. |

Reference Apple's [App Store product page](https://developer.apple.com/app-store/product-page/) documentation for detailed metadata formatting rules and common mistakes.

**A/B testing:**

- App Store Connect supports product page optimization — test up to 3 treatments against your original
- Test one element at a time: screenshots, icon, or subtitle
- Run tests for at least 7 days to get meaningful data
- Prioritize Screenshot 1 tests — it has the highest impact on conversion

### 5.2 Retention Strategies

Retention is harder than acquisition and more valuable. A 5% improvement in retention has more impact than a 20% increase in downloads.

**Push notifications (use carefully):**

- Ask for permission at a meaningful moment (after the user has experienced value), not on first launch
- Send notifications that are useful, not promotional. "Your weekly report is ready" beats "Check out our new feature!"
- Respect frequency — 1-2 per week maximum for most apps
- Let users control notification types in settings

**Streaks and habits:**

- If your app has a daily use case, consider a streak mechanic (consecutive days of use)
- Keep it lightweight — streaks should encourage, not punish. Missing a day shouldn't feel like a failure.
- Show progress: "You've used this 14 of the last 30 days" is motivating without being punitive

**Personalization:**

- Use app usage data to surface relevant content (recently viewed, most-used features)
- Customize the home screen based on user behavior patterns
- Remember user preferences and reduce friction for repeat actions

### 5.3 Referral Mechanics

Word of mouth is the most trusted acquisition channel. Make it easy for happy users to share.

**Share cards:**

- Create a visually appealing share image that users can post to social media
- Include the app name and a compelling reason to try it
- Generate these dynamically based on user data (e.g., "I tracked 30 activities this month with [App Name]")

**Invite friends:**

- Add a "Share with a friend" action in the app
- Pre-populate a share message that the user can customize
- If your business model supports it, offer a mutual benefit (both users get something)

**Organic sharing moments:**

- Identify moments of delight or achievement in your app — these are natural sharing triggers
- Add a share prompt at these moments (but make it dismissible, not forced)
- Track which sharing triggers actually get used

### 5.4 Re-engagement

Users who installed but stopped using the app are easier to win back than acquiring new users.

**Lapsed user campaigns:**

- Define "lapsed" for your app (e.g., no session in 14+ days)
- Send a re-engagement push notification with a specific reason to return (new feature, new content, saved progress)
- Use App Store Connect's "win-back offers" for subscription apps — offer a discounted rate to lapsed subscribers
- Don't spam. One re-engagement attempt is respectful. Three is annoying.

**App updates as re-engagement:**

- The "What's New" copy (Step 4) can pull users back — compelling updates trigger re-downloads
- Feature updates that address common feedback show users you're listening

### 5.5 Monitor Growth Metrics

Track these metrics to understand whether growth and retention strategies are working.

**Retention curves:**

| Metric | Target (varies by category) | What It Tells You |
| ------ | --------------------------- | ----------------- |
| D1 retention | 25-40% | First impression — did onboarding work? |
| D7 retention | 15-25% | Habit formation — is the app part of their routine? |
| D30 retention | 8-15% | Long-term value — does the app solve an ongoing need? |

If D1 is low, the problem is onboarding or first-time experience. If D7 drops sharply from D1, the app doesn't build a habit. If D30 is low, the long-term value proposition isn't strong enough.

**Growth metrics dashboard:**

- Track acquisition source (organic search, browse, referral, web referral)
- Monitor conversion rate (impressions to downloads) — this tells you if ASO is working
- Track viral coefficient (referrals per user) if you have referral mechanics
- Compare cohorts — are newer users retaining better than older ones? If yes, your improvements are working.

**Review cadence:**

- Weekly: Check retention curves and download trends
- Monthly: Full growth metrics review, ASO keyword performance, referral program performance
- Quarterly: Re-evaluate growth strategy, competitive landscape, and whether to invest in paid acquisition

## Deliverable

A growth metrics dashboard and an optimization plan. Save your growth strategy and metrics targets to your project's `docs/post-launch/growth-plan.md`.

## Definition of Done

- [ ] ASO metadata reviewed and optimized (keywords, screenshots, description)
- [ ] At least one retention strategy implemented (push notifications, streaks, or personalization)
- [ ] Share/referral mechanic exists in the app
- [ ] D1/D7/D30 retention is being tracked
- [ ] Growth metrics review cadence is established (weekly, monthly, quarterly)

## Tips

- **Retention before acquisition.** There is no point driving downloads to an app that doesn't retain. Fix the bucket before pouring in more water.
- **ASO is ongoing, not one-time.** Keywords that work at launch may not be optimal three months later. Review and adjust quarterly.
- **Push notifications are a double-edged sword.** Done well, they drive retention. Done poorly, they drive uninstalls. When in doubt, send fewer.
- **Measure before optimizing.** Don't add streaks, referrals, and personalization all at once. Add one, measure the impact, then decide whether to add the next.
- **Your best marketing is a great app.** No growth hack compensates for a poor user experience. If retention is low, improve the product before investing in acquisition.
