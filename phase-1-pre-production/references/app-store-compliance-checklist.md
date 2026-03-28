# App Store Compliance Checklist

A reference checklist for Apple App Store submission requirements. Use this during [Step 7: Expert Review Checkpoint](../steps/07-project-plan.md) to surface compliance gaps before they become sprint blockers.

Last reviewed against: [Apple App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/) (2025)

---

## Privacy & Data

- [ ] **PrivacyInfo.xcprivacy manifest** — required for all apps. Declares API usage reasons for required reason APIs (UserDefaults, file timestamp, disk space, system boot time, etc.)
- [ ] **Privacy nutrition labels** — App Store Connect privacy questionnaire completed accurately. Declares all data collected, linked to user, and used for tracking.
- [ ] **App Tracking Transparency (ATT)** — if using any form of tracking (ad SDKs, analytics that share data with third parties), must show ATT prompt before tracking. Prompt must include a clear purpose string.
- [ ] **ATT consent flow order** — if using ads: UMP consent → ATT prompt → initialize ad SDK. Getting this order wrong causes compliance issues or missing ad revenue.
- [ ] **Privacy policy URL** — required. Must be accessible from within the app and from the App Store listing.
- [ ] **Data deletion** — if collecting any user data, must provide a way for users to request deletion. This can be in-app, via a web form, or via email.

## Account & Authentication

- [ ] **Account deletion** — if the app creates accounts, it must offer account deletion (not just sign-out). This is required by App Store Guidelines 5.1.1(v) and GDPR.
- [ ] **Sign in with Apple** — required if the app offers any third-party sign-in (Google, Facebook, etc.). Must be offered alongside other options.
- [ ] **Guest mode / delayed auth** — consider allowing users to explore core features before requiring sign-in. Apps that require sign-in immediately may be rejected if the sign-in isn't justified.

## In-App Purchases

- [ ] **StoreKit 2** — use for all in-app purchases and subscriptions. StoreKit 1 still works but StoreKit 2 is the recommended path.
- [ ] **Subscription management** — must link to subscription management (Settings → Subscriptions) or use `manageSubscriptions` API.
- [ ] **Restore purchases** — must provide a "Restore Purchases" button for non-consumable and subscription purchases.
- [ ] **Paywall legal links** — subscription paywalls must include links to: Terms of Service, Privacy Policy. Subscription pricing, duration, and auto-renewal terms must be clearly visible.
- [ ] **No external payment links** — cannot direct users to external websites for purchases that bypass the App Store (with limited exceptions for reader apps and link entitlement).
- [ ] **Free trial clarity** — if offering a free trial, clearly state when billing begins and how to cancel.

## Content & Safety

- [ ] **Age rating** — accurately reflects app content. If content changes over time (user-generated, dynamic), rate for the maximum potential content.
- [ ] **UGC moderation** — if users can create/share content visible to others, must include: content reporting mechanism, blocking capability, content moderation (human or automated), and clear content guidelines.
- [ ] **Content policy** — if accepting user-generated content, publish and enforce a content policy within the app.
- [ ] **Objectionable content filtering** — apps with user-generated content must filter objectionable material before it's visible to other users.

## Ads

- [ ] **Ad SDK compliance** — ad SDKs must comply with Apple's privacy requirements. Check that your ad SDK version supports SKAdNetwork and ATT.
- [ ] **Ad placement** — ads must not interfere with app functionality, navigation, or be deceptive. Interstitial ads must have a clear close button.
- [ ] **Children's category** — if targeting children (under 13), additional restrictions apply: no behavioral advertising, no third-party analytics, limited data collection.

## Technical Requirements

- [ ] **iOS version support** — target the current and previous major iOS version minimum (e.g., iOS 17+ when iOS 18 is current).
- [ ] **IPv6 compatibility** — app must work on IPv6-only networks.
- [ ] **64-bit support** — required (32-bit apps not accepted since 2017).
- [ ] **Launch time** — app must launch in a reasonable time. Apps that take too long to launch may be rejected.
- [ ] **Background modes** — only declare background modes you actually use. Unused background mode declarations trigger review.

## App Store Listing

- [ ] **Screenshots** — required for each supported device size. Must show actual app UI (not marketing renders with no app content).
- [ ] **App icon** — 1024×1024, no alpha channel, no rounded corners (system applies them).
- [ ] **Description** — accurate representation of app functionality. Don't include pricing, "free" claims, or time-sensitive information.
- [ ] **Keywords** — 100 character limit. No competitor names, no repeating words already in your title.
- [ ] **Support URL** — required. Must be a working link to a support page or form.
- [ ] **Copyright** — must be a valid entity name (person or company), not a website URL.

## Common Rejection Reasons

These are the most frequent rejection causes from Apple's transparency reports:

1. **Guideline 2.1 — Performance: App Completeness** — broken links, placeholder content, incomplete features. Submit only when the app is complete.
2. **Guideline 4.0 — Design** — apps that are essentially websites wrapped in a WebView, or that don't provide enough native functionality.
3. **Guideline 2.3 — Performance: Accurate Metadata** — screenshots don't match the app, description is misleading, wrong category.
4. **Guideline 5.1.1 — Privacy: Data Collection and Storage** — missing privacy policy, inaccurate privacy labels, missing ATT prompt.
5. **Guideline 3.1.1 — Business: In-App Purchase** — attempting to use external payment for digital goods/services that should use IAP.

---

## When to Use This Checklist

- **Phase 1 Step 7 (Expert Review)** — review against this checklist before finalizing the project plan. Add compliance tasks to sprint scope.
- **Phase 3 Step 5 (Security & Privacy Audit)** — verify all items are implemented correctly.
- **Phase 4 Step 3 (Compliance Final Check)** — final verification before submission.

Items discovered here should be added as explicit sprint tasks, not left as "we'll figure it out before submission."
