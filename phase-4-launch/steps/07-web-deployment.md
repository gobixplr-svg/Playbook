# Step 7: Web Deployment (If Applicable)

## Purpose

Deploy a web presence that supports your app — a marketing/landing page, privacy policy hosting, and optionally deep linking or App Clips. By the end of this step, your app has a live website that drives App Store downloads, hosts required legal pages, and optionally integrates with iOS features like Universal Links and Smart App Banners.

**This step is optional.** If your app has no web component and you do not need a landing page, skip to the Phase 4 retrospective. However, even a simple one-page site significantly improves discoverability and provides a permanent home for your privacy policy and support information.

## Why This Matters

A web presence gives your app a home outside the App Store. It is where press links point, where search engines find you, and where users land when they Google your app name. Without it, your only presence is your App Store listing — which you do not fully control and which is not indexable by search engines in the same way.

It is also a practical requirement: Apple expects a working Support URL and Privacy Policy URL. Hosting these on a proper website is more professional than a Google Doc link.

## Prerequisites

- App is submitted or approved (Steps 1-4)
- App Store link is known (even if the app is not yet live, you can construct the URL)
- Privacy policy and terms of service are written
- Marketing assets from Step 2 are available (screenshots, icon, description)
- Reference the `web-dev-lead` skill for deployment details and technical implementation

## Process

### 7.1 Decide What You Need

Not every app needs the same web presence. Choose your level:

| Level | What it includes | When to use | Effort |
| ----- | ---------------- | ----------- | ------ |
| Minimal | Privacy policy + support page | Every app needs this at minimum | 1-2 hours |
| Landing page | Single marketing page + legal pages | Most apps benefit from this | 4-8 hours |
| Full site | Landing page + blog + docs + support | Apps with web components, APIs, or content marketing plans | 1-2 days |

For most solo dev launches, a landing page (Level 2) is the sweet spot.

### 7.2 Deploy the Marketing/Landing Page

A landing page has one job: convince visitors to download your app.

**Essential sections:**

1. **Hero:** App name, tagline, one screenshot or device mockup, prominent App Store download button
2. **Features:** 3-4 key benefits with icons or screenshots (reuse Step 2 assets)
3. **Social proof:** Testimonials from beta testers, press quotes, or download counts (add later as they accumulate)
4. **Call to action:** App Store badge with download link
5. **Footer:** Links to privacy policy, terms of service, support/contact, and social media

**Technical implementation:**

The `web-dev-lead` skill covers Cloudflare Pages deployment, React/Vite/Tailwind setup, and best practices in detail. Consult it for implementation specifics.

**Quick-start options by complexity:**

| Option | Stack | Best for | Deploy time |
| ------ | ----- | -------- | ----------- |
| Static HTML | Single HTML file + CSS | Minimal landing page | 30 minutes |
| Vite + React + Tailwind | React SPA | Polished landing page with interactivity | 2-4 hours |
| Next.js / Astro | Framework with SSG | Full site with blog, SEO, and multiple pages | 4-8 hours |

**Deployment with Cloudflare Pages:**

1. Push your site code to a GitHub repository
2. Connect the repository to Cloudflare Pages
3. Configure the build settings (build command, output directory)
4. Deploy — Cloudflare handles SSL, CDN, and global distribution
5. Add your custom domain

Refer to the `web-dev-lead` skill for step-by-step Cloudflare Pages deployment instructions.

### 7.3 App Store Smart Banner

Add the Smart App Banner meta tag to your website so iOS Safari visitors see a banner prompting them to download (or open) your app.

**Add this to your HTML `<head>`:**

```html
<meta name="apple-itunes-app" content="app-id=YOUR_APP_ID">
```

Replace `YOUR_APP_ID` with your numeric Apple ID (found in App Store Connect under App Information > General > Apple ID).

**Optional parameters:**

```html
<meta name="apple-itunes-app" content="app-id=YOUR_APP_ID, app-argument=your-deep-link-url">
```

The `app-argument` passes a URL to the app when the user taps "Open" — useful for deep linking to specific content.

**How it works:**

- iOS Safari automatically displays a banner at the top of the page
- If the user has the app installed, it shows "Open"
- If not installed, it shows "View" and links to the App Store
- The banner is native iOS UI — you do not need to style it
- It only appears on iOS Safari, not on other browsers or desktop

### 7.4 SEO Basics for App Landing Pages

Search engine traffic is free and compounds over time. Basic SEO takes minutes and pays dividends for months.

**Essential meta tags:**

```html
<title>App Name — Short Description | Your Brand</title>
<meta name="description" content="A 150-160 character description of your app that includes your primary keyword.">
<meta name="keywords" content="your, app, keywords, here">
```

**Open Graph tags (for social sharing):**

```html
<meta property="og:title" content="App Name — Short Description">
<meta property="og:description" content="Brief app description for social cards.">
<meta property="og:image" content="https://yoursite.com/og-image.png">
<meta property="og:url" content="https://yoursite.com">
<meta property="og:type" content="website">
```

**Twitter Card tags:**

```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="App Name — Short Description">
<meta name="twitter:description" content="Brief app description.">
<meta name="twitter:image" content="https://yoursite.com/twitter-card.png">
```

**Technical SEO checklist:**

- [ ] Page loads in under 3 seconds (Cloudflare Pages handles this well)
- [ ] Site is mobile-responsive (test on actual devices)
- [ ] HTTPS is enabled (Cloudflare Pages provides this automatically)
- [ ] `robots.txt` allows crawling
- [ ] `sitemap.xml` exists (especially for multi-page sites)
- [ ] Structured data (JSON-LD) for SoftwareApplication (helps Google show rich results)

**Structured data example:**

```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Your App Name",
  "operatingSystem": "iOS",
  "applicationCategory": "LifestyleApplication",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD"
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.8",
    "ratingCount": "125"
  }
}
```

Refer to the `web-dev-lead` skill for comprehensive SEO guidance.

### 7.5 Universal Links (Optional)

Universal Links allow your website URLs to open directly in your app instead of Safari. This creates a seamless experience when users click links to your content.

**When to implement:**

- Your app has content that maps to web URLs (articles, profiles, items)
- You want shared links to open in the app for users who have it installed
- You are building a web + app experience (not just a marketing page)

**If your app does not have shareable content or deep-linkable screens, skip this section.**

**Setup overview:**

1. **Create an Apple App Site Association (AASA) file** and host it at `https://yourdomain.com/.well-known/apple-app-site-association`
2. **Configure Associated Domains** in your Xcode project (Signing & Capabilities > Associated Domains > add `applinks:yourdomain.com`)
3. **Handle incoming URLs** in your app's SceneDelegate or SwiftUI App lifecycle

**AASA file format:**

```json
{
  "applinks": {
    "apps": [],
    "details": [
      {
        "appID": "TEAMID.com.yourcompany.appname",
        "paths": ["/item/*", "/profile/*"]
      }
    ]
  }
}
```

**Important:** The AASA file must be served with `Content-Type: application/json` and must be accessible without redirects. Cloudflare Pages handles this correctly by default for files in the `.well-known` directory.

### 7.6 App Clips (Optional)

App Clips are lightweight versions of your app that users can access without downloading the full app. They are triggered by QR codes, NFC tags, Smart App Banners, or links.

**When to consider:**

- Your app has a single-task flow that makes sense without the full app (e.g., ordering food, renting a scooter, checking in)
- You want to lower the barrier to first interaction

**For most solo dev launches, App Clips are not needed.** They add significant development and configuration overhead. Consider them for v2 if your app has a clear single-task entry point.

If you do implement an App Clip, the key configuration steps are:

1. Create an App Clip target in Xcode
2. Configure the App Clip experience in App Store Connect (URL, card image, subtitle)
3. Host the AASA file with App Clip configuration
4. Test with the App Clip Diagnostics tool

### 7.7 Privacy Policy and Terms of Service Hosting

Apple requires a working Privacy Policy URL and Support URL. Host these on your website.

**Privacy Policy:**

- Must be publicly accessible (no login required)
- Must accurately describe what data you collect, how you use it, and how users can request deletion
- Must include contact information
- Host at `https://yoursite.com/privacy` or `https://yoursite.com/privacy-policy`

**Terms of Service:**

- Required if your app has user accounts, subscriptions, or user-generated content
- Host at `https://yoursite.com/terms` or `https://yoursite.com/terms-of-service`

**Support page:**

- At minimum, provide an email address or contact form
- Ideally, include an FAQ section with answers to common questions
- Host at `https://yoursite.com/support`

**Update App Store Connect:** Once these pages are live, update the Support URL and Privacy Policy URL in App Store Connect to point to them.

### 7.8 Verify Everything Works

After deployment, verify:

- [ ] Landing page loads correctly on desktop and mobile
- [ ] App Store download button links to the correct App Store listing
- [ ] Smart App Banner appears on iOS Safari
- [ ] Privacy policy page is accessible from the URL entered in App Store Connect
- [ ] Support page is accessible from the URL entered in App Store Connect
- [ ] Open Graph tags render correctly when sharing the URL on social media (use [https://cards-dev.twitter.com/validator](https://cards-dev.twitter.com/validator) or similar)
- [ ] SSL certificate is valid (HTTPS works without warnings)
- [ ] Universal Links work if implemented (test by tapping a link on an iOS device)
- [ ] Page speed is acceptable (test with [PageSpeed Insights](https://pagespeed.web.dev/))

## Deliverable

Live web presence: marketing/landing page deployed, privacy policy and terms of service hosted, App Store Smart Banner configured, and basic SEO implemented. All URLs updated in App Store Connect.

## Definition of Done

- [ ] Website deployed and accessible at your domain
- [ ] Landing page communicates app value and links to App Store
- [ ] Privacy policy hosted and publicly accessible
- [ ] Terms of service hosted (if applicable)
- [ ] Support page or contact information hosted
- [ ] App Store Connect URLs updated (Support URL, Privacy Policy URL, Marketing URL)
- [ ] Smart App Banner meta tag added
- [ ] Basic SEO implemented (meta tags, Open Graph, structured data)
- [ ] Universal Links configured (if applicable)
- [ ] Site verified on mobile and desktop
- [ ] SSL enabled and working

## Tips

- **A one-page site is enough.** Do not let the website become a project that delays your launch. A single, well-designed page with a download button, privacy policy link, and support email covers all requirements.
- **Use the `web-dev-lead` skill for implementation details.** This step guide covers *what* to deploy. The skill covers *how* — including Cloudflare Pages setup, React/Vite/Tailwind patterns, and SEO specifics.
- **The Smart App Banner is the highest-ROI web feature.** One meta tag in your HTML gives every iOS Safari visitor a native prompt to download your app. There is no easier conversion mechanism.
- **Host your privacy policy on a page you control.** Google Docs or Notion links look unprofessional and can break if you change the sharing settings. A static page on your domain is permanent and trustworthy.
- **Do not skip Open Graph tags.** When someone shares your landing page link on Twitter or LinkedIn, the preview card is generated from OG tags. Without them, the link preview is a blank box with a URL — which nobody clicks.
- **Ship the website before the app if possible.** A live landing page with "Coming Soon" lets you collect emails, test your messaging, and have a URL ready for press outreach before launch day.
