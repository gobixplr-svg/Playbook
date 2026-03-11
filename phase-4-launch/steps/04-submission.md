# Step 4: Submission

## Purpose

Archive, upload, and submit your app for App Store review. By the end of this step, your app is "Waiting for Review" in App Store Connect, your version metadata is complete, and you have a plan for handling the review outcome — whether it is approval or rejection.

## Why This Matters

Submission is the point of no return for your current build. A clean submission — with correct metadata, a stable build, and complete information — passes review faster and avoids back-and-forth with the review team. A sloppy submission triggers rejections for things that have nothing to do with your app's quality: a missing screenshot, a broken demo account, a vague privacy description.

Review times vary from 24 hours to 7 days. Every rejection adds another review cycle. Getting it right the first time is the fastest path to launch.

## Prerequisites

- App Store Connect fully configured (Step 1)
- Store and marketing assets ready (Step 2)
- Compliance final check passed (Step 3)
- App is stable — no known crashers or critical bugs
- All test suites pass

## Process

### 4.1 Prepare the Final Build

Before archiving, do a final verification pass on the build you intend to submit.

**Pre-archive checklist:**

- [ ] Scheme is set to **Release** (not Debug)
- [ ] Build number is incremented from the last uploaded build
- [ ] Version number is correct (e.g., 1.0.0 for first release)
- [ ] All debug flags, test endpoints, and developer menus are disabled
- [ ] Analytics and crash reporting point to production (not staging)
- [ ] API endpoints point to production servers
- [ ] No test data is bundled in the app
- [ ] App runs correctly on a physical device in Release configuration
- [ ] All test suites pass against the Release build

**Version and build numbering:**

- **Version** (CFBundleShortVersionString): The user-facing version, e.g., `1.0.0`. This is what appears on the App Store.
- **Build** (CFBundleVersion): An incrementing number for each upload, e.g., `1`, `2`, `3`. App Store Connect rejects duplicate build numbers for the same version.

### 4.2 Archive and Upload

**Archive:**

1. In Xcode, select **Product > Archive**
2. Xcode builds the Release configuration and creates an archive
3. When complete, the Organizer window opens showing your archive

If the archive fails, common causes are:

- Signing issues (check Signing & Capabilities in target settings)
- Missing provisioning profile (download from Developer portal or let Xcode manage automatically)
- Compiler errors that only appear in Release mode (optimizations catch issues Debug mode does not)

**Upload:**

1. In the Organizer, select your archive and click **Distribute App**
2. Choose **App Store Connect** as the distribution method
3. Choose **Upload** (not Export)
4. Select distribution options:
   - **Strip Swift symbols:** Yes (reduces app size)
   - **Upload symbols:** Yes (enables crash symbolication in App Store Connect)
   - **Manage version and build number:** Let Xcode manage unless you have a reason not to
5. Select the signing certificate and provisioning profile
6. Click **Upload**

The upload takes a few minutes. After it completes, the build appears in App Store Connect under TestFlight within 15-30 minutes (Apple processes the build server-side).

**Alternative upload method:**

If Xcode upload fails (network issues, timeout), you can use `xcrun altool` or Transporter (free app on the Mac App Store) to upload the `.ipa` exported from the Organizer.

### 4.3 Wait for Build Processing

After upload, Apple processes the build. This typically takes 15-30 minutes but can take up to a few hours.

**While waiting, check for:**

- **Email from Apple** — if the build has issues (missing privacy manifest, invalid binary), Apple sends an email. Check your App Store Connect email address.
- **Build status in App Store Connect** — under TestFlight, the build should move from "Processing" to "Ready to Test"
- **Compliance warnings** — resolve any warnings before proceeding to submission

Do not proceed to the next section until the build shows as available in App Store Connect.

### 4.4 Complete Version Metadata

In App Store Connect, navigate to your app and select the version you are submitting. Fill in all required metadata.

**Version Information:**

| Field | What to enter |
| ----- | ------------- |
| What's New | Describe what is in this version. For v1.0, this is your launch description — summarize the app's core value in 2-4 sentences. |
| Screenshots | Upload all screenshots from Step 2 for each required device size |
| App Preview | Upload preview video(s) from Step 2 (optional but recommended) |
| Description | App Store description — lead with benefits, include keywords naturally, max 4000 characters |
| Keywords | Comma-separated, max 100 characters total. Use the `app-builder` skill's ASO strategy for keyword research. |
| Support URL | Link to your support page, FAQ, or contact form |
| Marketing URL | Link to your app's website or landing page (optional) |
| Promotional Text | Up to 170 characters, can be changed without a new submission. Use for timely messages. |

**App Review Information:**

| Field | What to enter |
| ----- | ------------- |
| Contact Information | Name, phone, email for the review team to reach you |
| Demo Account | If your app requires login, provide a test account with username and password |
| Notes | Explain anything non-obvious to the reviewer. If features require specific setup, describe it. |
| Attachment | Optional — include a video walkthrough if your app's flow is complex |

**The demo account is critical.** If your app requires sign-in and you do not provide working credentials, the review will be rejected. Create a dedicated review account with:

- Pre-populated data so the reviewer sees a functional app (not empty states)
- Access to all features you want reviewed (including premium features if applicable)
- No two-factor authentication or additional verification steps that would block the reviewer

### 4.5 Submit for Review

Once all metadata is complete and the build is selected:

1. Click **Add for Review**
2. Review the submission summary — verify screenshots, build, and metadata are correct
3. Click **Submit to App Review**

Your app status changes to **Waiting for Review**.

**Review timeline expectations:**

- Most apps are reviewed within 24-48 hours
- During busy periods (holidays, WWDC), reviews can take up to 7 days
- You can check current average review times at [Apple's App Review page](https://developer.apple.com/app-store/review/)
- You will receive an email when the review starts and when it completes

### 4.6 Common Rejection Reasons

The `app-store-expert` skill covers rejection patterns in detail. Here are the most frequent causes for solo developer apps:

**Metadata issues:**

- Screenshots do not match the actual app UI
- Description contains pricing information that is inaccurate
- App name or subtitle contains generic keywords (e.g., "Best Fitness App")
- Support URL leads to a broken or placeholder page

**Functionality issues:**

- App crashes during review (test on the same iOS version the reviewer uses — typically the latest release)
- App requires external hardware or conditions the reviewer cannot replicate (provide video)
- Login required but no demo account provided
- Features described in metadata are not present in the build
- In-app purchase does not complete or deliver content

**Privacy issues:**

- Privacy manifest missing or inaccurate
- Usage description strings are generic ("This app needs access to your camera" — explain why)
- Data collection not disclosed in App Privacy Details
- ATT prompt not shown before tracking occurs

**Design issues:**

- App is a thin wrapper around a website (use Safari instead)
- App does not provide enough functionality to justify being on the App Store
- UI is broken or unusable on certain device sizes

Consult the `app-store-expert` skill for the complete rejection pattern database and specific guidance for your app's category.

### 4.7 What to Do While Waiting

Review time is dead time only if you let it be. Use it productively:

- **Prepare launch marketing materials** (Step 5) if not already done
- **Write your launch day monitoring plan** (Step 6)
- **Set up your web presence** (Step 7) if applicable
- **Draft your Product Hunt listing** if you plan to launch there
- **Prepare social media posts** for launch day
- **Write release notes** for your blog or newsletter
- **Set up analytics dashboards** so they are ready when the app goes live
- **Brief your beta testers** — ask them to leave reviews on launch day

### 4.8 Handling Rejection

If your app is rejected, you will receive an email with the specific guideline(s) violated and the reviewer's notes.

**Fix vs. Appeal decision tree:**

1. **Read the rejection reason carefully.** Is it factually accurate? Does the reviewer correctly describe the issue?
2. **If the rejection is valid** — the reviewer identified a real issue:
   - Fix the issue
   - Upload a new build (increment the build number)
   - Reply to the rejection in the Resolution Center explaining what you fixed
   - Resubmit
3. **If the rejection seems incorrect** — the reviewer misunderstood your app or applied the wrong guideline:
   - Reply in the Resolution Center with a clear, respectful explanation
   - Include screenshots or a video showing the feature works correctly
   - Reference the specific guideline and explain why your app complies
   - Do not be confrontational — reviewers are human and respond better to clear, factual explanations
4. **If the reply does not resolve it** — you can request an appeal through the App Review Board:
   - Use the "Appeal" option in the Resolution Center
   - This escalates to a senior reviewer
   - Only use this for genuine disagreements about guideline interpretation, not for issues you should fix

**Resubmission after rejection:**

- Rejected builds go through a full review cycle again (not an expedited queue)
- The same reviewer may or may not review the resubmission
- Address every point in the rejection — do not fix one issue and leave another unresolved
- Use the Resolution Center notes to explain all changes clearly

## Deliverable

App submitted and in review: build uploaded, all metadata complete, app status is "Waiting for Review" (or "In Review" or "Ready for Sale" if review has completed).

## Definition of Done

- [ ] Final build archived in Release configuration
- [ ] Build uploaded to App Store Connect and processed successfully
- [ ] No compliance warnings or emails from Apple about the build
- [ ] All screenshots and preview videos uploaded for required device sizes
- [ ] App description, keywords, and metadata completed
- [ ] Demo account provided (if app requires login)
- [ ] App Review notes explain any non-obvious features or setup
- [ ] Support URL is live and accessible
- [ ] App submitted and status is "Waiting for Review" or beyond
- [ ] Rejection handling plan is understood (fix vs. appeal decision tree)

## Tips

- **Submit before you think you are ready.** Review time is the longest wait in the launch process. Submit as soon as the build is stable and compliant — you can update screenshots and metadata while the app is in review.
- **The demo account is the #1 preventable rejection.** If your app has any form of sign-in, provide a working demo account with pre-populated data. Test the credentials yourself before submitting.
- **Reply in the Resolution Center, not by email.** All reviewer communication happens through App Store Connect's Resolution Center. Email replies to rejection notifications do not reach the review team.
- **Keep rejection responses factual and short.** "We fixed the crash in XYZ by updating the null check in ABC. Build 42 addresses this." is better than a three-paragraph explanation.
- **Do not submit on Friday.** If you are rejected over the weekend, you lose two days. Submit early in the week so you have time to respond within the same review cycle.
- **Expedited reviews exist but are rare.** Apple offers expedited review requests for critical bug fixes or time-sensitive events. Do not abuse this for your initial launch — save it for genuine emergencies post-launch.
