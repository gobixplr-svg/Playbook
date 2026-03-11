# Step 2: Store & Marketing Assets

## Purpose

Create all visual assets needed for App Store submission and launch marketing. By the end of this step, you have polished screenshots, preview videos, promotional artwork, and marketing materials — everything needed to present your app professionally to the world.

## Why This Matters

Your store listing is your app's storefront. Most users decide whether to download within 3-5 seconds of viewing the listing. Screenshots and preview videos do the heavy lifting — they communicate value faster than any description text.

Weak store assets undermine great apps. A polished app with generic screenshots loses to an average app with compelling visuals.

## Prerequisites

- App UI is feature-complete and visually polished (Phase 3 QA passed)
- Design tokens and brand assets from Phase 1 are finalized
- Asset inventory from Phase 1 Step 5.8 identifies what's needed
- Raw screenshots and screen recordings captured during Phase 2 sprints (see [Asset Pipeline Reference](../../phase-1-pre-production/references/asset-pipeline.md))

## Process

### 2.1 App Store Screenshots

Screenshots are the most impactful element of your store listing. Apple allows up to 10 per localization; most successful apps use 4-8.

**Required dimensions:**

| Device | Size (pixels) | Required |
| ------ | ------------- | -------- |
| iPhone 6.9" (16 Pro Max) | 1320 x 2868 | Yes — primary showcase device |
| iPhone 6.7" (15 Plus/Pro Max) | 1290 x 2796 | Yes — required for older device support |
| iPhone 6.5" (11 Pro Max/XS Max) | 1242 x 2688 | Yes — still widely required |
| iPhone 5.5" (8 Plus) | 1242 x 2208 | Conditional — only if supporting older devices |
| iPad 12.9" (6th gen) | 2048 x 2732 | Only if iPad app |
| iPad 12.9" (2nd gen) | 2048 x 2732 | Only if iPad app (older layout) |

**Tip:** Design for the largest size first, then scale down. Or use a tool that generates all sizes from one template.

**Screenshot strategy:**

1. **Lead with the core value.** Screenshot 1 should show the single most compelling screen with a headline that answers "why should I download this?" Not "Welcome to AppName" — that wastes the most important slot.
2. **Show the journey.** Screenshots 2-5 should walk through the core user flow in order — browse → engage → achieve → share.
3. **End with social proof or premium.** The last screenshot can show ratings, awards, or premium features.
4. **Every screenshot needs a text overlay.** A headline (5-8 words max) above or below the device frame that tells the user what they're seeing and why it matters.

**Screenshot creation workflow:**

1. **Capture raw screenshots:** Run the app in Simulator at the required device sizes. Use `Cmd+S` in Simulator to save screenshots. Populate with realistic (not lorem ipsum) data.
2. **Design screenshot frames:** Use [AppShot](https://outofofficeoutdoors.com/products/appshot) (free, browser-based) or tools like Figma, ScreenshotBot, Rotato, or AppMockUp:
   - Device frame (realistic or minimal)
   - Background color or gradient (use your brand palette)
   - Text overlay area (headline + optional subheadline)
   - Consistent layout across all screenshots
3. **Write headlines:** Each screenshot gets a benefit-driven headline. Not "Home Screen" — instead "Track your mood in one tap." Focus on what the user gets, not what the screen shows.
4. **Export at required sizes:** Export each screenshot at every required dimension. Name files clearly: `01-track-mood-6.9.png`, `02-view-history-6.9.png`, etc.

**What makes great screenshots:**

- Real app UI (not mockups or wireframes)
- Realistic data that tells a story
- Headlines that communicate benefits, not features
- Consistent visual treatment across the set
- The first screenshot is the strongest — it shows in search results

**What to avoid:**

- Empty states or sparse data
- Settings screens or login screens
- Too much text (headlines should be 5-8 words)
- Inconsistent backgrounds or frame styles across the set
- Status bar showing test data (use clean status bars)

### 2.2 App Preview Video

App previews auto-play in the App Store and dramatically increase conversion. Apple allows up to 3 preview videos per localization.

**Specifications:**

| Device | Resolution | FPS | Duration | Format |
| ------ | ---------- | --- | -------- | ------ |
| iPhone 6.9" | 1320 x 2868 | 30 | 15-30 sec | H.264, .mov or .mp4 |
| iPhone 6.7" | 1290 x 2796 | 30 | 15-30 sec | H.264, .mov or .mp4 |
| iPhone 6.5" | 1242 x 2688 | 30 | 15-30 sec | H.264, .mov or .mp4 |

**Preview video strategy:**

- **One video is usually enough.** Focus on the core loop: the action users repeat most often.
- **Show, don't tell.** Demonstrate the app in use — taps, scrolls, transitions. Minimize text overlays in the video itself.
- **Hook in the first 3 seconds.** The poster frame (first frame) is what users see before playing. Make it compelling.
- **Keep it smooth.** Practice the flow before recording. No hesitation, no wrong taps, no loading spinners.

**Creation workflow:**

1. **Script the flow:** Write the exact sequence of taps/scrolls (e.g., "Open app → tap mood → select happy → see animation → view history"). Time it to 20-25 seconds.
2. **Record in Simulator or on device:** Use QuickTime (File > New Screen Recording) for Simulator, or screen record on device (Settings > Control Center > Screen Recording).
3. **Edit:** Trim dead time, speed up transitions if needed, add text overlays for context. Tools: iMovie (free), Screen Studio ($30), Rotato ($50).
4. **Add device frame (optional):** Wrap the recording in a device frame for a polished look. Rotato does this automatically.
5. **Export:** H.264 codec, .mov or .mp4, matching the required dimensions.

### 2.3 App Icon Finalization

If you deferred final icon creation from Phase 1, finalize it now.

Use [BatchSnap](https://outofofficeoutdoors.com/products/batchsnap) (free, browser-based) to generate all required icon sizes from your 1024x1024 source file. It exports the full iOS icon set with no uploads — everything runs in your browser.

**Checklist:**

- [ ] 1024x1024px PNG, sRGB, no transparency, no rounded corners (iOS masks it)
- [ ] All required sizes generated (use BatchSnap or Xcode's automatic generation)
- [ ] Tested at all rendering sizes (29pt through 1024pt)
- [ ] Dark mode variant provided (iOS 18+ tinted icon support)
- [ ] Icon looks distinct when placed next to competitors on a home screen
- [ ] Icon communicates the app's purpose without text

### 2.4 Promotional Artwork

Apple may feature your app if you provide promotional artwork. Even if not featured, these assets are useful for your own marketing.

**Apple promotional artwork (optional but recommended):**

| Asset | Size | Purpose |
| ----- | ---- | ------- |
| Short card | 1040 x 800 | Today tab feature cards |
| Long card | 1040 x 1600 | Today tab expanded |
| Hero image | 2880 x 1136 | Top banner feature |

**Guidelines:**

- Use your app's visual language (colors, typography) but show the app in context, not just a device screenshot
- Include the app icon somewhere in the composition
- No pricing or promotional text (Apple prohibits this)
- Show the app solving a real user problem

### 2.5 Marketing Materials

These extend beyond the App Store to support launch marketing and press outreach.

**Press kit (recommended):**

Create a folder with:

- App icon in multiple sizes (PNG, 512px and 1024px)
- 3-5 high-resolution screenshots without device frames (for press to use in articles)
- App description (short: 1 sentence, medium: 1 paragraph, long: 3 paragraphs)
- Developer/team bio and photo
- Contact email for press inquiries
- Link to the app on the App Store

**Social media assets:**

| Platform | Dimensions | Content |
| -------- | ---------- | ------- |
| Twitter/X | 1200 x 675 | Launch announcement card |
| Instagram | 1080 x 1080 | Feature highlight carousel (3-5 slides) |
| LinkedIn | 1200 x 627 | Professional launch announcement |
| Product Hunt | 240 x 240 (icon) + 1270 x 760 (gallery) | Product Hunt listing |

**Creation approach:**

- Build templates in Figma or Canva using your brand colors and typography
- Use [BatchSnap](https://outofofficeoutdoors.com/products/batchsnap) to batch-resize assets to each platform's dimensions
- Repurpose screenshot content — the same app screens work across formats with different framing
- Write platform-specific copy (casual on Twitter, professional on LinkedIn, feature-focused on Product Hunt)

### 2.6 Review and Optimize

Before uploading, review all assets:

**Quality checklist:**

- [ ] All screenshots are pixel-perfect at required dimensions
- [ ] No test data, debug UI, or simulator artifacts visible
- [ ] Text overlays are legible and free of typos
- [ ] Color consistency across all assets (same brand palette)
- [ ] Preview video runs smoothly with no lag or hesitation
- [ ] Device frames are current (not outdated phone models)
- [ ] All required sizes are generated for each device class

**A/B testing prep:**

- Create 2 variants of Screenshot 1 (different headline or different screen)
- App Store Connect supports product page optimization — test which screenshot set converts better
- Track which variant to test first in your launch plan

## Deliverable

Complete the [Store Asset Checklist Template](../templates/store-asset-checklist.md) and save it to your project's `docs/launch/store-asset-checklist.md`.

## Definition of Done

- [ ] App Store screenshots created for all required device sizes
- [ ] Each screenshot has a benefit-driven text overlay
- [ ] App preview video recorded, edited, and exported at required specs
- [ ] App icon finalized and tested at all rendering sizes
- [ ] Promotional artwork created (if seeking Apple feature)
- [ ] Press kit assembled
- [ ] Social media assets created for launch platforms
- [ ] All assets reviewed for quality and consistency
- [ ] Assets uploaded to App Store Connect (or ready to upload)

## Tips

- **Capture during production.** The best time to grab raw screenshots and recordings is while features are fresh. Don't wait until launch week to stage everything.
- **Realistic data wins.** Populate the app with data that tells a story — not "Test User" and "Lorem ipsum." Use names, stats, and content that feel real.
- **Lead with benefits, not features.** "Track your mood in one tap" beats "Mood Tracking Screen." Every headline should answer "why should I care?"
- **Competitors are your benchmark.** Before creating assets, browse the top 5 apps in your App Store category. Note what their screenshots look like. Then make yours better.
- **Screenshots are your highest-ROI marketing asset.** Spend more time here than on any other marketing material. They determine whether users tap "Get."
