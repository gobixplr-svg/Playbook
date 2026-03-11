# Step 1: Store Preparation

## Purpose

Set up your App Store Connect presence so the store is ready to receive a build. By the end of this step, your app record exists with correct metadata, pricing is configured, territories are selected, in-app purchases are defined (if applicable), and TestFlight is ready for external testing.

## Why This Matters

App Store Connect configuration is prerequisite work that blocks everything downstream. You cannot upload a build, submit for review, or configure in-app purchases until the app record exists with a valid bundle ID. Getting this done early — before the final compliance pass and submission — means you are not scrambling with store admin on launch week.

Mistakes here cause silent delays. A wrong bundle ID means re-signing and re-uploading. A misconfigured pricing tier means your free app accidentally charges users. A missing territory means users in key markets cannot find you.

## Prerequisites

- Apple Developer Program membership is active ($99/year)
- Bundle ID is finalized in your Xcode project (matches what you will register)
- Pricing model is decided (free, freemium with IAP, paid upfront)
- App name is finalized (from Phase 0 Step 6)
- App icon is ready at 1024x1024 (from Phase 1 Step 5 or Phase 4 Step 2)
- Phase 3 testing is complete or near-complete

## Process

### 1.1 Create the App Record

Log in to [App Store Connect](https://appstoreconnect.apple.com) and create a new app.

**Required fields:**

| Field | What to enter | Notes |
| ----- | ------------- | ----- |
| Platform | iOS | Select additional platforms if applicable |
| Name | Your app's display name | Max 30 characters. Must be unique on the App Store. |
| Primary Language | Your default language | Usually English (U.S.) |
| Bundle ID | com.yourcompany.appname | Must match Xcode project exactly |
| SKU | A unique identifier you choose | Not visible to users. Use something like `appname-ios-v1`. |

**Tips for the app name:**

- Check availability before committing — App Store Connect will reject duplicates
- The name here is what appears on the store listing, not the home screen (that is the Display Name in Xcode)
- You can change it later, but name changes during review may trigger additional review time

### 1.2 Configure App Information

After creating the app record, fill in the App Information section:

**Category selection:**

- **Primary category:** Choose the category that best describes your app's core function. This is the category where your app appears in browse results.
- **Secondary category (optional):** Choose a second category if your app spans two areas. This gives you visibility in two browse sections.

Browse the [Apple category list](https://developer.apple.com/app-store/categories/) and look at where competitor apps are listed. Choose the category where your target users actually browse.

**Additional fields:**

- **Content Rights:** Declare whether your app contains third-party content
- **Age Rating:** You will complete the questionnaire in Step 3 (Compliance), but be aware it lives here
- **License Agreement:** Use Apple's standard EULA unless you have a custom one

### 1.3 Set Up Pricing and Availability

**Pricing:**

| Model | Configuration | Notes |
| ----- | ------------- | ----- |
| Free | Price tier: Free | No payment setup needed |
| Paid | Select a price tier | Apple sets regional prices automatically based on your base tier |
| Freemium | Price tier: Free + configure IAPs separately | See Section 1.5 |

**Availability:**

- **All territories** is the default and usually correct for v1 launches
- Deselect specific territories only if you have a legal or content reason (e.g., region-specific regulations, content licensing)
- **Pre-order:** Available if you want to collect interest before the app is live. Users can pre-order up to 180 days before release. Consider this only if you have a marketing reason — most solo dev launches do not need it.

**Release options:**

- **Manually release this version:** You control when the app goes live after approval. Best for coordinated launch days.
- **Automatically release this version:** The app goes live as soon as it passes review. Best if you just want it out.
- **Automatically release on a specific date:** Schedule a release date. Useful for coordinated launches, but risky if review takes longer than expected.

For most solo dev launches, choose "Manually release" so you control the exact go-live moment.

### 1.4 Register the Bundle ID

If you have not already registered your bundle ID in the Apple Developer portal, do it now.

1. Go to [Certificates, Identifiers & Profiles](https://developer.apple.com/account/resources/identifiers/list)
2. Click the **+** button to register a new identifier
3. Select **App IDs** then **App**
4. Enter a description and your bundle ID (Explicit)
5. Enable any capabilities your app uses (Push Notifications, HealthKit, Sign in with Apple, etc.)
6. Click **Register**

**Verify the bundle ID matches your Xcode project** — open Xcode, select your target, and confirm the Bundle Identifier under Signing & Capabilities matches exactly.

### 1.5 Configure In-App Purchases (If Applicable)

Skip this section if your app is free with no premium features or if it is a one-time paid download.

**IAP types:**

| Type | Use case | Example |
| ---- | -------- | ------- |
| Consumable | One-time use, can purchase again | Extra credits, tokens |
| Non-Consumable | Buy once, own forever | Remove ads, unlock feature |
| Auto-Renewable Subscription | Recurring billing | Monthly/yearly premium access |
| Non-Renewing Subscription | Fixed-duration access, no auto-renew | Season pass |

**For each IAP, configure:**

1. **Reference Name** — internal label (not shown to users)
2. **Product ID** — unique string (e.g., `com.yourcompany.appname.premium_monthly`). Cannot be changed or reused after creation.
3. **Pricing** — select a price tier for one-time purchases, or set up subscription pricing/tiers
4. **Display Name and Description** — user-facing text shown on the purchase sheet
5. **Review Screenshot** — a screenshot showing the IAP in context (for App Review)
6. **Review Notes** — explain to the reviewer how to test the IAP

**For subscriptions, also configure:**

- Subscription group (group related tiers so users can upgrade/downgrade)
- Introductory offers (free trial, pay-as-you-go, pay-up-front)
- Promotional offers (for win-back campaigns — configure later)

**StoreKit configuration:**

Ensure your Xcode project has a StoreKit configuration file for local testing. If you set this up in Phase 2/3, verify the product IDs match what you just created in App Store Connect.

### 1.6 Set Up TestFlight

If you already configured TestFlight during Phase 3, verify the setup is current. If not, set it up now.

**Internal testing:**

- Internal testers are members of your App Store Connect team (up to 100)
- Builds are available immediately after upload — no review required
- Use this for your own testing and trusted collaborators

**External testing:**

- External testers can be anyone with an email address (up to 10,000)
- First build in a new group requires Beta App Review (usually 24-48 hours)
- Subsequent builds to the same group are usually available immediately
- Requires beta app description, feedback email, and privacy policy URL

**TestFlight checklist:**

- [ ] At least one internal test group exists
- [ ] Test accounts are added to the internal group
- [ ] Beta App Description is filled in (for external testing)
- [ ] Beta feedback email is set
- [ ] Privacy policy URL is provided (can be a placeholder page during beta)
- [ ] At least one successful build has been uploaded and distributed

### 1.7 Verify Everything Connects

Before moving on, confirm the full chain works:

1. **Bundle ID** in Apple Developer portal matches Xcode project
2. **App record** in App Store Connect references the correct bundle ID
3. **Signing** in Xcode uses the correct team and provisioning profile
4. **A test build uploads successfully** via Xcode (Product > Archive > Distribute App > TestFlight)
5. **The build appears** in App Store Connect under TestFlight
6. **IAPs (if any)** appear in the app record and match the product IDs in your code

If the test build upload fails, the most common causes are:

- Bundle ID mismatch between Xcode and App Store Connect
- Provisioning profile does not include required capabilities
- Missing or invalid app icon
- Info.plist issues (missing required keys)

Fix any issues now. A build that cannot upload blocks every remaining step.

## Deliverable

App Store Connect fully configured: app record created, pricing and availability set, in-app purchases defined (if applicable), TestFlight operational, and a test build successfully uploaded.

## Definition of Done

- [ ] App record created in App Store Connect with correct name, bundle ID, and SKU
- [ ] Primary and secondary categories selected
- [ ] Pricing tier configured (free, paid, or freemium)
- [ ] Territory availability set
- [ ] Release option chosen (manual, automatic, or scheduled)
- [ ] Bundle ID registered in Apple Developer portal with required capabilities
- [ ] In-app purchases configured in App Store Connect (if applicable)
- [ ] IAP product IDs match StoreKit configuration in Xcode (if applicable)
- [ ] TestFlight set up with at least one test group
- [ ] Test build successfully uploaded and distributed via TestFlight
- [ ] Bundle ID, signing, and provisioning verified end-to-end

## Tips

- **Do this before your compliance pass.** Step 3 (Compliance Final Check) requires an existing app record for several verification steps. Having a build already uploaded also lets you verify privacy manifests are bundled correctly.
- **Product IDs are permanent.** Once you create an IAP product ID, it cannot be reused even if you delete the IAP. Choose a clear naming convention and stick with it.
- **Manual release gives you control.** Unless you have a reason to auto-release, choose manual. It lets you coordinate launch day with marketing, press outreach, and social posts.
- **Upload a build early.** Even if it is not the final build, uploading early validates the entire signing and distribution pipeline. Discovering a provisioning issue on launch day is avoidable pain.
- **TestFlight external review is separate from App Review.** Your beta build goes through a lighter review process, but it still gets reviewed. Submit your first external TestFlight build a few days before you need it.
