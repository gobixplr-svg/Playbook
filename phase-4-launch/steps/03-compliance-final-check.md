# Step 3: Compliance Final Check

## Purpose

Verify that your app meets every Apple compliance requirement before submission. By the end of this step, you have passed a systematic audit of privacy manifests, tracking transparency, content declarations, export compliance, and third-party SDK policies — so that your submission does not get rejected for preventable compliance issues.

## Why This Matters

Compliance rejections are the most common and most frustrating reason for launch delays. They are also the most preventable. Apple's requirements span privacy, content, legal, and technical domains — and they change regularly. A compliance gap discovered after submission means a rejection, a fix-and-resubmit cycle, and 1-7 additional days of review.

This step is a pre-flight check. You run it once, fix everything it catches, and submit with confidence.

## Prerequisites

- App Store Connect app record exists (Step 1)
- A build is uploaded and available in TestFlight (Step 1)
- Store and marketing assets are ready (Step 2)
- You have access to the `app-store-expert` skill for detailed guidance

## Process

### 3.1 Run the App Store Expert Pre-Submission Review

The `app-store-expert` skill contains comprehensive, up-to-date knowledge of Apple's review guidelines, privacy requirements, rejection patterns, and technical requirements. **Use it as your primary compliance reference rather than duplicating its content here.**

Start by running the `app-store-expert` skill's pre-submission review workflow. This covers:

- App Store Review Guidelines compliance (all sections)
- Common rejection patterns and how to avoid them
- Technical requirements verification
- Metadata best practices

Document every finding. Fix issues immediately or create a tracked list of items to resolve before submission.

### 3.2 PrivacyInfo.xcprivacy Verification

Apple requires a privacy manifest file (`PrivacyInfo.xcprivacy`) in your app bundle. This declares what data your app collects and why.

**Verification steps:**

1. **Confirm the file exists** in your Xcode project and is included in the app target's build phase (Copy Bundle Resources)
2. **Verify NSPrivacyTracking** is set correctly — `true` only if your app tracks users as defined by Apple (linking user/device data with third-party data for advertising or sharing with data brokers)
3. **Verify NSPrivacyTrackingDomains** lists all tracking domains (if NSPrivacyTracking is true)
4. **Verify NSPrivacyCollectedDataTypes** accurately lists every category of data your app collects, with correct purposes, linked/not-linked status, and tracking/not-tracking status
5. **Verify NSPrivacyAccessedAPITypes** lists all required reason APIs your app uses (UserDefaults, file timestamp, system boot time, disk space, active keyboards)

**Common mistakes:**

- Forgetting to include the privacy manifest from third-party SDKs (each SDK should ship its own)
- Declaring data as "not linked to identity" when you do have a user account system
- Missing required reason APIs that are called indirectly through dependencies

Refer to the `app-store-expert` skill for the current list of required reason APIs and data collection categories.

### 3.3 App Tracking Transparency (ATT) Check

If your app tracks users (as defined by Apple), you must implement the ATT framework.

**Do you need ATT?**

- If you use any advertising SDK that links device data with third-party data: **Yes**
- If you use analytics that are first-party only (e.g., your own analytics, no data sharing): **No**
- If you are unsure, consult the `app-store-expert` skill for Apple's current definition of "tracking"

**If ATT is required, verify:**

- [ ] `NSUserTrackingUsageDescription` is present in Info.plist with a clear, user-friendly explanation
- [ ] ATT prompt is shown before any tracking occurs
- [ ] App respects the user's choice — if they deny, no tracking data is collected
- [ ] Privacy manifest `NSPrivacyTracking` is set to `true`
- [ ] Tracking domains are listed in `NSPrivacyTrackingDomains`

**If ATT is not required:**

- [ ] Confirm `NSPrivacyTracking` is set to `false` in your privacy manifest
- [ ] Confirm you are not inadvertently linking user data with third-party data through any SDK

### 3.4 App Privacy Details Accuracy

App Privacy Details are the privacy "nutrition labels" displayed on your App Store listing. They are configured in App Store Connect, not in code.

**Verification steps:**

1. Open App Store Connect > Your App > App Privacy
2. Walk through every data type category and verify your declarations match what the app actually does
3. Cross-reference with your `PrivacyInfo.xcprivacy` — the App Store Connect declarations and the privacy manifest should be consistent

**Categories to review:**

- Contact Info (name, email, phone)
- Health & Fitness (if using HealthKit)
- Financial Info (if processing payments beyond Apple IAP)
- Location (precise or coarse)
- User Content (photos, videos, audio)
- Browsing/Search History
- Identifiers (user ID, device ID)
- Usage Data (product interaction, advertising data)
- Diagnostics (crash data, performance data)

For each category where you collect data, declare:

- **Purpose:** Why you collect it (app functionality, analytics, advertising, etc.)
- **Linked to identity:** Can this data be associated with a user account?
- **Used for tracking:** Is this data used for tracking as defined by Apple?

### 3.5 Age Rating Questionnaire

Complete the age rating questionnaire in App Store Connect. This determines your app's age rating (4+, 9+, 12+, 17+) based on content.

**Answer honestly for:**

- Cartoon or fantasy violence
- Realistic violence
- Sexual content or nudity
- Profanity or crude humor
- Drug, alcohol, or tobacco references
- Simulated gambling
- Horror or fear themes
- Medical/treatment information
- Mature/suggestive themes
- Contests or gambling

**User-generated content (UGC):** If your app displays any content created by users (comments, profiles, posts), you must answer "Yes" to the UGC question. This typically pushes the rating to 17+ unless you have moderation in place. If your app has no UGC, make sure you are not accidentally exposing third-party content that counts as UGC.

### 3.6 Content Declarations

**EULA (End User License Agreement):**

- Apple provides a standard EULA that applies by default
- You only need a custom EULA if your app has terms that differ from Apple's standard agreement (e.g., subscription terms, data usage policies beyond standard)
- If using a custom EULA, add it in App Store Connect > Your App > App Information > License Agreement

**Copyright:**

- Enter the copyright holder and year in App Store Connect (e.g., "2026 Your Name")
- Ensure you have rights to all content in the app (images, fonts, sounds, data)
- If using open-source libraries, verify their licenses allow App Store distribution (most permissive licenses like MIT, Apache 2.0 do; GPL may not)

### 3.7 Export Compliance (Encryption)

Apple requires you to declare whether your app uses encryption.

**Quick decision tree:**

1. Does your app use encryption beyond what Apple provides through standard system frameworks (HTTPS/TLS via URLSession, ATS)? If **No** — select "No" for export compliance and you are done.
2. If **Yes** — does your encryption qualify for an exemption? Most apps that only use HTTPS/TLS for API calls and standard cryptographic functions qualify for the exemption.
3. If your app uses custom encryption algorithms or encrypts data at rest with non-system libraries, you may need to submit an Encryption Registration (ERN) or provide documentation.

**For most apps:**

- Using HTTPS for API calls = exempt (no action needed beyond the declaration)
- Using Apple's CryptoKit or Security framework = exempt
- Using a third-party encryption library for custom encryption = may require ERN

Set the `ITSAppUsesNonExemptEncryption` key in your Info.plist:

- `false` — if you only use exempt encryption (HTTPS, standard Apple frameworks)
- `true` — if you use non-exempt encryption (you will need to provide compliance documentation)

Setting this key in Info.plist avoids being asked the question every time you upload a build.

### 3.8 Third-Party SDK Compliance Audit

Apple holds your app responsible for everything third-party SDKs do. Audit each dependency.

**For each SDK in your project:**

| Check | What to verify |
| ----- | -------------- |
| Privacy manifest | SDK includes its own `PrivacyInfo.xcprivacy` (required for Apple's listed SDKs) |
| Data collection | SDK's data practices are disclosed in your App Privacy Details |
| Required reason APIs | SDK's use of required reason APIs is declared in its privacy manifest |
| Minimum deployment target | SDK supports your app's minimum iOS version |
| App Store distribution license | SDK's license allows App Store distribution |

**Where to find this information:**

- Check the SDK's documentation or GitHub repository for privacy manifests
- Apple maintains a list of commonly used SDKs that must include privacy manifests — consult the `app-store-expert` skill for the current list
- Run `grep -r "NSPrivacyAccessedAPITypes" Pods/` or `grep -r "NSPrivacyAccessedAPITypes" .build/` to find privacy manifests in your dependencies

**Common SDKs to audit:**

- Analytics (Firebase, Mixpanel, Amplitude)
- Crash reporting (Firebase Crashlytics, Sentry)
- Networking (Alamofire — typically no privacy concerns)
- Backend (Supabase, Firebase)
- Ads (AdMob, AppLovin — these require ATT)
- Maps (Mapbox, Google Maps)

### 3.9 Final Compliance Checklist

Run through this checklist as the final gate before proceeding to Step 4 (Submission).

**Privacy:**

- [ ] `PrivacyInfo.xcprivacy` exists, is in the app bundle, and is accurate
- [ ] App Privacy Details in App Store Connect match actual data practices
- [ ] ATT implemented correctly (if applicable) or confirmed not needed
- [ ] All third-party SDKs have privacy manifests (where required)

**Content:**

- [ ] Age rating questionnaire completed honestly
- [ ] EULA is appropriate (standard or custom)
- [ ] Copyright information is entered
- [ ] All content is properly licensed

**Technical:**

- [ ] Export compliance declared (`ITSAppUsesNonExemptEncryption` in Info.plist)
- [ ] Bundle ID matches across Xcode, Developer portal, and App Store Connect
- [ ] All required capabilities are enabled in the Developer portal
- [ ] Info.plist contains all required usage description strings (camera, location, health, etc.)

**Review readiness:**

- [ ] `app-store-expert` pre-submission review completed with no unresolved findings
- [ ] All findings from the pre-submission review have been fixed or documented with justification

## Deliverable

Compliance checklist passed: every item in Section 3.9 is checked, all findings from the `app-store-expert` pre-submission review are resolved, and the app is ready for submission.

## Definition of Done

- [ ] `app-store-expert` pre-submission review workflow completed
- [ ] PrivacyInfo.xcprivacy verified and accurate
- [ ] ATT implementation verified (or confirmed not needed)
- [ ] App Privacy Details in App Store Connect are accurate and consistent with privacy manifest
- [ ] Age rating questionnaire completed
- [ ] Content declarations completed (EULA, copyright)
- [ ] Export compliance declared in Info.plist
- [ ] Third-party SDK compliance audit completed
- [ ] Final compliance checklist (Section 3.9) passed with all items checked

## Tips

- **Use the `app-store-expert` skill as your primary reference.** It has detailed, current guidance on review guidelines, privacy requirements, and rejection patterns. This step guide tells you *what* to check — the skill tells you *how* and provides the latest specifics.
- **The privacy manifest is the most common compliance gap.** Apple started enforcing privacy manifest requirements in 2024, and many apps still get it wrong. Double-check every data type and required reason API.
- **Set `ITSAppUsesNonExemptEncryption` in Info.plist.** This saves you from answering the encryption question on every build upload. For most apps that only use HTTPS, the answer is `false`.
- **App Privacy Details and PrivacyInfo.xcprivacy must agree.** They are configured in different places (App Store Connect vs. Xcode), but Apple cross-references them. Mismatches trigger review questions or rejections.
- **Third-party SDKs are your responsibility.** If a dependency collects data you did not disclose, your app gets rejected — not the SDK. Audit before you submit.
- **Complete this step even if you are confident.** Compliance requirements change. Running the checklist takes 30-60 minutes and can save a week of rejection-resubmit cycles.
