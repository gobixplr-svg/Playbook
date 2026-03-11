# Step 2: User Feedback Loop

## Purpose

Build a systematic process for collecting, categorizing, and acting on user feedback. Feedback is the signal that tells you what to build next — but only if you capture it consistently and prioritize it honestly.

## Process

### 2.1 Collect Feedback

Feedback arrives through multiple channels. Cast a wide net, but funnel everything into one place.

**Feedback sources:**

| Source | Type of Feedback | How to Capture |
| ------ | ---------------- | -------------- |
| App Store reviews | General sentiment, bugs, feature requests | Check weekly (Step 1 review cadence) |
| Support emails | Specific issues, detailed bug reports | Dedicated support email address |
| In-app feedback | Contextual feedback while using the app | Feedback form or shake-to-report |
| Beta testers | Pre-release feedback from TestFlight users | TestFlight feedback + direct messages |
| Social media | Public sentiment, feature ideas, complaints | Monitor mentions and hashtags |

**In-app feedback options (pick one):**

- **Simple:** A "Send Feedback" button in Settings that opens a pre-addressed email compose sheet
- **Medium:** A feedback form with category picker (Bug / Feature Request / Other) that sends to your backend or email
- **Full:** Shake-to-report with automatic screenshot attachment (libraries like Instabug or custom implementation)

Start with Simple. Upgrade when volume justifies it.

### 2.2 Categorize Feedback

Every piece of feedback gets categorized. This turns a pile of comments into a structured input for planning.

**Categories:**

| Category | Description | Example |
| -------- | ----------- | ------- |
| Bug | Something is broken or behaves unexpectedly | "The app crashes when I tap the share button" |
| Feature Request | Something the user wants that doesn't exist | "I wish I could export my data as CSV" |
| UX Confusion | The feature exists but the user can't find or understand it | "I didn't know I could swipe to delete" |
| Praise | Positive feedback worth noting | "Love the new dark mode" |

**For each feedback item, record:**

- Source (where it came from)
- Date
- Category
- Description (user's words, not your interpretation)
- Frequency (is this the first time or a recurring theme?)

Use the [Feedback Log Template](../templates/feedback-log.md) to track this.

### 2.3 Prioritize Using RICE

Not all feedback is equal. Use the RICE framework to score feature requests and decide what to build next.

**RICE scoring:**

| Factor | Question | Scale |
| ------ | -------- | ----- |
| **R**each | How many users will this affect per quarter? | Number of users |
| **I**mpact | How much will this move the needle per user? | 3 = massive, 2 = high, 1 = medium, 0.5 = low, 0.25 = minimal |
| **C**onfidence | How sure are you about reach and impact estimates? | 100% = high, 80% = medium, 50% = low |
| **E**ffort | How many person-weeks to implement? | Number (higher = worse) |

**RICE Score = (Reach x Impact x Confidence) / Effort**

Higher scores = higher priority. Bugs bypass RICE scoring — they're prioritized by severity (critical > high > medium > low).

**Practical tips for RICE:**

- Feedback that appears from 3+ independent sources gets a Reach boost
- UX Confusion items often have high Impact with low Effort (just improve discoverability)
- Be honest about Confidence — if you're guessing, use 50%

### 2.4 Close the Loop

Feedback without follow-up erodes trust. Users who feel heard become advocates. Users who feel ignored leave.

**App Store reviews:**

- Respond to every negative review (1-3 stars). Acknowledge the issue, state what you're doing about it.
- Respond to detailed positive reviews with a brief thank-you.
- When a reported bug is fixed, update your response: "This is fixed in version X.Y — thanks for reporting it."

**Support emails:**

- Acknowledge receipt within 24-48 hours, even if the fix will take longer
- When the issue is resolved, follow up with the specific version that contains the fix

**Feature requests:**

- Don't promise timelines. Say "This is on our radar" or "We're considering this for a future update."
- When you ship a requested feature, mention it in What's New and notify users who requested it (if you have their contact)

## Deliverable

A feedback log with categorized entries and RICE-scored priorities. Save to your project's `docs/post-launch/feedback-log.md` using the [Feedback Log Template](../templates/feedback-log.md).

## Definition of Done

- [ ] All active feedback channels are identified and monitored
- [ ] Feedback log is created with at least one entry per active channel
- [ ] Entries are categorized (Bug / Feature Request / UX Confusion / Praise)
- [ ] Feature requests are scored using RICE
- [ ] Process for responding to App Store reviews is established

## Tips

- **One feedback log, not five.** Funnel everything into a single document or tool. Scattered feedback means missed patterns.
- **Quote users directly.** Record their words, not your interpretation. "I can't find the settings" is different from "Settings button needs to be more prominent" — the first is a fact, the second is your assumption about the fix.
- **Frequency is the strongest signal.** A feature request from one user is an anecdote. The same request from ten users is a pattern. Track how often themes repeat.
- **UX Confusion is your cheapest win.** The feature already exists — you just need to make it discoverable. These often have the best RICE scores.
- **Don't build everything users ask for.** Users describe problems, not solutions. "Add a dark mode" might really mean "the app is hard to use at night." Understand the underlying need.
