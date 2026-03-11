# Step 5: Security & Privacy Audit

## Purpose

Verify that the app handles data responsibly, meets Apple's privacy requirements, and has no exploitable security gaps. This step produces a formal audit report that confirms the app is safe to ship — or identifies exactly what needs fixing before it is.

## Why This Matters

App Store review rejections for privacy violations cost 1-2 weeks per cycle. A missing PrivacyInfo.xcprivacy entry, an incorrect App Privacy Details response, or a hardcoded API key discovered post-launch can mean emergency patches, user trust damage, or outright removal from the store. Security and privacy issues are also the most expensive category to fix after launch — the cost curve is exponential, not linear.

For solo developers using AI tools, this is especially critical. AI-generated code can introduce subtle security flaws that compile, pass tests, and look correct — but leak data, skip validation, or store credentials insecurely. This audit is the safety net.

## Prerequisites

- Steps 1-4 completed (functional, platform, and performance testing done)
- App builds and runs on real devices
- All third-party SDKs integrated in their final form
- Privacy policy drafted or existing
- Access to App Store Connect for App Privacy Details review

## Process

### 5.1 Data Inventory

Before auditing privacy compliance, you need a complete picture of what data the app touches. Walk through every feature and document the data lifecycle.

**For each data type the app handles, record:**

| Data Type | Collected | Stored | Transmitted To | Purpose | Linked to Identity | Tracking |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| [e.g., email] | [yes/no] | [where: Keychain, UserDefaults, database] | [your server, third-party SDK] | [app functionality, analytics] | [yes/no] | [yes/no] |

**Common data types to check:**

- User account info (email, name, profile photo)
- Location data (precise, coarse, always, while-in-use)
- Health and fitness data (HealthKit, activity, biometrics)
- Usage analytics (screen views, feature usage, crash logs)
- Device identifiers (IDFA, IDFV, device model)
- Financial data (purchases, subscription status)
- User-generated content (photos, text, routes, workouts)
- Contacts or social graph data
- Search history or browsing data

**How to find data you forgot about:**

1. Search the codebase for `UserDefaults`, `Keychain`, `CoreData`, `SwiftData`, `FileManager`, any database client (e.g., `supabase`, `firebase`)
2. Search for network calls — `URLSession`, `Alamofire`, any SDK `.send()` or `.request()` methods
3. Check third-party SDK documentation for what they collect automatically (many analytics SDKs collect device info, IP address, and usage patterns without explicit code)

### 5.2 PrivacyInfo.xcprivacy Manifest

Apple requires a privacy manifest that declares what APIs your app uses and why. This applies to your app and to any third-party SDK that accesses covered API categories.

**Reference:** The `app-store-expert` skill's `privacy-requirements.md` covers the full PrivacyInfo.xcprivacy specification, required API categories, and approved reason codes.

**Audit checklist:**

- [ ] PrivacyInfo.xcprivacy exists in the app target
- [ ] Every "required reason API" used by your code has a declared reason
- [ ] Every third-party SDK that accesses required reason APIs includes its own privacy manifest (or you've declared their usage in yours)
- [ ] API reason codes match your actual usage — don't copy-paste all reasons; only declare what you actually do

**Required reason API categories to check:**

- File timestamp APIs (`NSFileCreationDate`, `NSFileModificationDate`)
- System boot time APIs (`systemUptime`, `ProcessInfo`)
- Disk space APIs (`volumeAvailableCapacityKey`)
- User defaults APIs (`UserDefaults` — only when accessed from a third-party SDK context)
- Active keyboard APIs

**Verification:** Build the app and check the build log for privacy manifest warnings. Xcode 15+ flags missing declarations.

### 5.3 App Privacy Details (App Store Connect)

App Privacy Details is the user-facing privacy label in the App Store. It must accurately reflect your data inventory from Section 5.1.

**For each data type collected, verify:**

- [ ] Data type is listed in App Privacy Details
- [ ] "Linked to You" / "Not Linked to You" classification is correct
- [ ] "Used to Track You" classification is correct (tracking = combining your data with third-party data for advertising or sharing with data brokers)
- [ ] Purpose categories are accurate (App Functionality, Analytics, Advertising, etc.)

**Common mistakes:**

- Forgetting to list data collected by third-party SDKs (analytics, crash reporters, ad networks)
- Marking data as "Not Linked" when it is associated with a user account
- Not listing data that is collected but never displayed to the user (background analytics, diagnostics)
- Listing data types you planned to collect but removed during development

Cross-reference your data inventory table (Section 5.1) row by row against your App Privacy Details entries. Every row should have a matching entry, and no App Privacy Details entry should exist without a matching row.

### 5.4 App Tracking Transparency (ATT)

If the app tracks users across other companies' apps or websites (IDFA access, cross-app data sharing for advertising), ATT is required.

**Audit checklist:**

- [ ] Determine if ATT is required — if you use IDFA or share data with ad networks, it is
- [ ] If required: ATT prompt appears before any tracking occurs (not at app launch — after the user understands the value)
- [ ] If required: `NSUserTrackingUsageDescription` in Info.plist with a clear, app-specific explanation
- [ ] If required: App respects the user's choice — denial means no tracking, not degraded experience
- [ ] If not required: Verify no SDK is accessing IDFA behind the scenes (search for `ASIdentifierManager` and `ATTrackingManager`)

**Timing matters:** Don't show the ATT prompt on first launch before the user has any context. Show it at a natural point where the user understands why — e.g., before enabling personalized recommendations. Conversion rates for ATT prompts improve significantly when shown in context vs. at cold launch.

### 5.5 Third-Party SDK Privacy Audit

Each third-party SDK is a liability. It can collect data you didn't intend, access APIs that require privacy manifest declarations, and introduce security vulnerabilities.

**For each SDK:**

| SDK | Version | Privacy Manifest Included | Data Collected | APIs Used | Risk Level |
| ---- | ---- | ---- | ---- | ---- | ---- |
| [name] | [version] | [yes/no] | [what it sends home] | [required reason APIs] | [low/medium/high] |

**Verification steps:**

1. Check if the SDK ships its own PrivacyInfo.xcprivacy (required for SDKs on Apple's list of commonly used frameworks)
2. Review the SDK's privacy documentation — what data does it collect by default?
3. Check if the SDK offers privacy-respecting configuration (disable analytics, opt out of data collection)
4. Verify the SDK version is current — older versions may lack privacy manifests

**Red flags:**

- SDK has no privacy manifest and accesses covered APIs
- SDK documentation doesn't clearly state what data it collects
- SDK collects data that isn't reflected in your App Privacy Details
- SDK hasn't been updated in 12+ months

### 5.6 Privacy Policy Verification

Your privacy policy must match reality — what the app actually collects, not what a template says.

**Verify:**

- [ ] Privacy policy URL is accessible and loads correctly
- [ ] Every data type from Section 5.1 is mentioned in the policy
- [ ] Data retention periods are stated
- [ ] Third-party data sharing is disclosed (which SDKs, what data, why)
- [ ] User rights are documented (data deletion, data export, opt-out)
- [ ] Contact information for privacy inquiries is provided
- [ ] Policy is written in plain language (not just legal boilerplate)
- [ ] If the app targets children or uses health data, additional compliance sections are present (COPPA, HIPAA, GDPR as applicable)

### 5.7 Secrets and Credentials Audit

Search the entire codebase for hardcoded secrets. This is the single most common security mistake in AI-generated code.

**Search for:**

```text
# Search patterns — run each against the full codebase
"api_key"
"apiKey"
"API_KEY"
"secret"
"password"
"token"
"private_key"
"sk_live"
"sk_test"
"Bearer "
"Authorization"
```

**Audit checklist:**

- [ ] No API keys, tokens, or passwords hardcoded in Swift files
- [ ] No secrets in Info.plist that should be server-side only
- [ ] `.gitignore` excludes `.env`, `*.xcconfig` with secrets, and any credential files
- [ ] Git history doesn't contain previously committed secrets (if it does, rotate the keys — removing from HEAD isn't enough)
- [ ] Service role keys (admin/write access) are server-side only — never in the client bundle
- [ ] Public keys in the client are read-only and restricted by bundle ID

**Key placement rules:**

| Key Type | Acceptable Location | Never In |
| ---- | ---- | ---- |
| Public/anon keys (read-only, bundle-ID-restricted) | Info.plist, build config | Hardcoded in source |
| User tokens (access, refresh, OAuth) | Keychain | UserDefaults, source code |
| Service role keys (admin access) | Edge Functions, server env | Client bundle, Info.plist, source |

### 5.8 Database and API Security

Verify that the backend enforces security — don't rely on the client to be well-behaved.

**Row Level Security (RLS):**

- [ ] RLS is enabled on every table in the database
- [ ] Every table has at least one policy (a table with RLS enabled but no policies denies all access — verify this is intentional or add policies)
- [ ] SELECT policies prevent users from reading other users' data
- [ ] INSERT policies verify the user is inserting their own data (`auth.uid() = user_id`)
- [ ] UPDATE policies prevent users from modifying other users' records
- [ ] DELETE policies prevent users from deleting other users' data
- [ ] Service role bypass is limited to Edge Functions / server-side only

**Server-side validation:**

- [ ] All input validation happens on the server, not just the client
- [ ] Pricing, scoring, and calculations are server-side (client-side values can be tampered with)
- [ ] Premium content is withheld server-side via RLS or API logic — not hidden by client-side feature flags
- [ ] Rate limiting is configured on sensitive endpoints (auth, purchases, data mutations)
- [ ] Error responses don't leak database structure, stack traces, or internal identifiers

**Reference:** The `code-review` skill's security review dimensions cover these areas systematically.

### 5.9 OWASP Mobile Top 10 Quick Check

Walk through the OWASP Mobile Top 10 as a final sanity check. Not every item applies to every app — mark each as applicable or N/A with a brief justification.

| # | Risk | Status | Notes |
| ---- | ---- | ---- | ---- |
| M1 | Improper Platform Usage | [ ] | Misuse of platform features (Keychain, permissions, biometrics) |
| M2 | Insecure Data Storage | [ ] | Sensitive data in UserDefaults, unencrypted files, logs |
| M3 | Insecure Communication | [ ] | Missing HTTPS, certificate pinning, data in transit |
| M4 | Insecure Authentication | [ ] | Weak auth flows, session management, token handling |
| M5 | Insufficient Cryptography | [ ] | Weak algorithms, hardcoded keys, improper key storage |
| M6 | Insecure Authorization | [ ] | Client-side auth decisions, missing server checks |
| M7 | Client Code Quality | [ ] | Buffer overflows, format strings (less common in Swift) |
| M8 | Code Tampering | [ ] | Jailbreak detection, integrity checks (risk-dependent) |
| M9 | Reverse Engineering | [ ] | Sensitive logic in client, obfuscation needs |
| M10 | Extraneous Functionality | [ ] | Debug endpoints, test accounts, hidden admin features |

For a typical solo-developer iOS app, M1 (platform usage), M2 (data storage), M4 (authentication), and M6 (authorization) are the highest-priority items. M8 and M9 are generally lower priority unless the app handles financial data or DRM.

### 5.10 Compile the Audit Report

Consolidate all findings into a single security audit report. For each finding, assign a severity and determine if it blocks launch.

**Finding format:**

| ID | Category | Severity | Finding | Blocks Launch | Resolution |
| ---- | ---- | ---- | ---- | ---- | ---- |
| SEC-001 | [section] | Critical/High/Medium/Low | [what's wrong] | [yes/no] | [what to do] |

**Severity definitions:**

- **Critical** — Exploitable vulnerability, data breach risk, or guaranteed App Store rejection. Must fix before launch.
- **High** — Significant security gap or privacy compliance issue. Should fix before launch.
- **Medium** — Suboptimal practice that increases risk but isn't immediately exploitable. Fix in first post-launch update.
- **Low** — Minor improvement opportunity. Track for future work.

## Anti-Patterns

- **Audit-by-assumption** — "I think we handle data correctly" without actually tracing the data flow. The inventory (Section 5.1) exists because memory is unreliable.
- **Copy-paste privacy manifests** — Declaring all possible API reasons instead of only the ones you actually use. Apple reviews these and rejects apps with unjustified declarations.
- **Privacy policy templates** — Using a generic template without customizing it to your actual data practices. The policy must match the data inventory.
- **Security through obscurity** — Assuming attackers won't find hardcoded keys because the code is compiled. IPA files are trivially decompilable. Secrets in the binary are public.
- **Client-side-only security** — Validating input, checking permissions, or gating features only in the client. The server must enforce every rule independently.
- **Skipping the SDK audit** — Trusting that third-party SDKs handle privacy correctly. Each SDK is your responsibility in the App Store review.
- **One-time audit** — Running this once and never again. Re-audit after any significant feature addition or SDK update.

## Deliverable

Security audit report saved to your project's `docs/testing/security-audit-report.md`. The report should include:

- Data inventory table (Section 5.1)
- Privacy compliance status (Sections 5.2-5.6)
- Security findings table (Section 5.10)
- OWASP quick check results (Section 5.9)
- Launch readiness determination (all Critical/High findings resolved or documented with mitigation plan)

## Definition of Done

- [ ] Data inventory complete — every data type collected, stored, or transmitted is documented
- [ ] PrivacyInfo.xcprivacy covers all required reason APIs with correct reason codes
- [ ] App Privacy Details in App Store Connect match actual data collection
- [ ] ATT implementation verified (or confirmed not required)
- [ ] Third-party SDKs audited for privacy manifest compliance
- [ ] Privacy policy matches actual data practices
- [ ] No hardcoded secrets in codebase or git history
- [ ] RLS policies verified on all database tables
- [ ] Server-side validation confirmed on all endpoints
- [ ] OWASP Mobile Top 10 reviewed with findings documented
- [ ] All Critical and High findings have a resolution plan
- [ ] Audit report committed to version control

## Tips

- **Start with the data inventory.** Everything else flows from knowing what data you actually touch. Skip this and you'll miss things in every downstream section.
- **Use `git log -p --all -S 'api_key'` to search git history for secrets.** Removing a key from HEAD doesn't remove it from history. If you find one, rotate the key immediately — don't just delete the commit.
- **Test RLS policies with a real authenticated session, not the service role.** The service role bypasses RLS entirely. Your audit must use the same access level as your users.
- **Run the `code-review` skill on security-sensitive code paths.** Authentication flows, payment processing, data encryption, and permission checks are high-value targets for review.
- **Check third-party SDK changelogs before updating.** New SDK versions can add new data collection that changes your privacy obligations.
- **Privacy compliance is not a one-time gate.** Every feature that touches new data types, adds an SDK, or changes data flows should trigger a mini-audit of the affected sections.
- **When in doubt about App Privacy Details, over-disclose.** Failing to disclose data collection causes App Store rejection. Disclosing data you don't collect just makes your privacy label slightly longer.
