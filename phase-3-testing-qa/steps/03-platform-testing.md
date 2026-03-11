# Step 3: Platform Testing

## Purpose

Verify the app works correctly across the full range of devices, OS versions, and system configurations your users will have. Functional testing (Step 2) proved the app works on your primary test device. Platform testing proves it works everywhere you intend to support.

## Why This Matters

The iPhone SE has a 4.7-inch screen. The iPhone 16 Pro Max has a 6.9-inch screen. That is nearly 50% more vertical space. Layouts that look great on one can truncate, overflow, or waste space on the other. Similarly, iOS 17 and iOS 18 have different API behaviors, permission flows, and default settings.

Dark mode, Dynamic Type, and Reduce Motion are not edge cases — Apple reports that a significant percentage of users enable Dark mode, and accessibility settings are legally protected in many markets. If your app breaks under these configurations, it is broken for real users, not hypothetical ones.

Platform testing on real devices also catches performance issues the simulator hides. The simulator runs on your Mac's M-series chip with 16-32 GB of RAM. An iPhone SE has 4 GB. What feels instant on the simulator can stutter on real hardware.

## Prerequisites

- Functional testing (Step 2) complete — all primary test cases passed on the baseline device
- Test Plan (Step 1) with device matrix defined
- Access to physical test devices (at minimum: one small screen, one standard, one large)
- Bug list from Step 2 available for comparison (don't re-report known bugs)

## Process

### 3.1 Prepare Test Devices

Set up each physical device for testing.

**For each device:**

1. Update to the target iOS version (current or current-1, per your matrix)
2. Sign in with a test Apple ID (not your personal account)
3. Install the app via Xcode (Build to Device) or TestFlight
4. Reset any relevant settings to defaults before beginning

**Minimum device matrix:**

| Device | Screen | Chip | RAM | Role |
| ------ | ------ | ---- | --- | ---- |
| iPhone SE (3rd gen) | 4.7" | A15 | 4 GB | Smallest screen, lowest spec |
| iPhone 16 | 6.1" | A18 | 8 GB | Most common, baseline |
| iPhone 16 Pro Max | 6.9" | A18 Pro | 8 GB | Largest screen, highest spec |

**If you only have one physical device**, prioritize it for performance and permissions testing. Use the simulator for layout checks at different screen sizes — layout bugs are visible in the simulator even though performance bugs are not.

### 3.2 Test Screen Size Extremes

Run the critical user flows on the smallest and largest devices, focusing on layout issues.

**On iPhone SE (smallest screen), check:**

- [ ] Text is not truncated in headers, labels, or buttons
- [ ] Buttons and tap targets are not overlapping or too small (minimum 44x44 points)
- [ ] Scrollable content is actually scrollable (not clipped at the bottom)
- [ ] Modals and sheets are fully visible and dismissible
- [ ] Keyboard does not cover input fields (test every text input in the app)
- [ ] Tab bar and navigation bar items are not cramped or overlapping
- [ ] Images scale down without breaking layout

**On iPhone 16 Pro Max (largest screen), check:**

- [ ] Content does not float in the center with excessive margins
- [ ] Full-width elements actually reach the edges (respecting safe areas)
- [ ] List items and cards use the extra space well (not stretched thin)
- [ ] Bottom sheets and action sheets are positioned correctly
- [ ] Landscape layout (if supported) does not break

**Document layout issues** with screenshots showing the problem on both devices side by side.

### 3.3 Test iOS Version Compatibility

Test on both your current and current-1 iOS versions.

**API availability checks:**

- Features using iOS 17+ APIs work correctly on iOS 17
- Features using iOS 18+ APIs are either gated with `@available` checks or not present on iOS 17
- No runtime crashes from unavailable APIs

**Behavioral differences to watch for:**

| Area | iOS 17 vs. iOS 18 Differences |
| ---- | ----------------------------- |
| Navigation | `NavigationStack` behavior differences, sheet presentation |
| Permissions | Location, notification, and tracking prompt timing and wording |
| Background refresh | Different scheduling behavior and battery optimization |
| Widget rendering | Timeline and rendering lifecycle changes |
| Privacy | App Tracking Transparency enforcement differences |

**Test the full permission flow on each iOS version:**

1. Fresh install — verify permission prompts appear at the right time
2. Grant permission — verify feature works
3. Deny permission — verify graceful degradation
4. Change permission in Settings mid-session — verify app responds correctly

### 3.4 Test Dark Mode

Switch the device to Dark mode (Settings → Display & Brightness → Dark) and run through all primary flows.

**Check for:**

- [ ] All text is visible against dark backgrounds (no dark-on-dark text)
- [ ] Images with transparency render correctly (no white halos or invisible elements)
- [ ] Custom colors adapt (hardcoded light-mode colors break in dark mode)
- [ ] Status bar content is visible (light content on dark background)
- [ ] Separator lines and borders are visible but not harsh
- [ ] Maps, charts, and third-party views adapt to dark mode
- [ ] Launch screen / splash screen matches the dark mode appearance
- [ ] No flicker or flash of light mode during transitions

**Then switch back to Light mode** and verify nothing is broken by the toggle. Some apps cache dark mode assets and display them in light mode after switching.

### 3.5 Test Dynamic Type

Dynamic Type is Apple's system-wide text scaling. Test at three levels:

**Level 1 — Default size:**

- This is your baseline. Should already work from functional testing.

**Level 2 — Largest standard size (Settings → Display & Brightness → Text Size → maximum):**

- [ ] All text remains readable and fully visible
- [ ] Labels do not overlap each other or clip against container edges
- [ ] Buttons remain tappable with legible text
- [ ] Horizontal layouts switch to vertical stacking if needed
- [ ] Scroll views accommodate the expanded content height

**Level 3 — Largest accessibility size (Settings → Accessibility → Display & Text Size → Larger Text → maximum):**

- [ ] App remains usable (navigation, core flows, data entry)
- [ ] Text does not extend beyond the screen without scrolling
- [ ] Fixed-height containers either grow or become scrollable
- [ ] Images and icons maintain appropriate sizing relative to text
- [ ] Tab bar and navigation bar items remain accessible

**Common Dynamic Type failures:**

- Fixed-height rows in lists — text overflows or clips
- Hardcoded font sizes (e.g., `.font(.system(size: 14))`) instead of text styles (e.g., `.font(.body)`)
- Horizontal label-value pairs that need to stack vertically at large sizes
- Constraints that work at default size but conflict at larger sizes

### 3.6 Test Reduce Motion

Enable Reduce Motion (Settings → Accessibility → Motion → Reduce Motion) and verify:

- [ ] Hero animations are replaced with cross-fades or removed
- [ ] Parallax scrolling effects are disabled
- [ ] Auto-playing animations (loading spinners are fine, decorative animations are not) are stopped
- [ ] Page transitions use simple fades instead of slides/zooms
- [ ] No content is inaccessible due to removed animations (e.g., a tutorial that requires swiping through animated cards)

### 3.7 Test Network Conditions

Test the app's behavior under degraded and absent network conditions.

**Airplane mode (no network):**

1. Launch the app with airplane mode enabled
   - [ ] App does not crash
   - [ ] Cached/local data is displayed (if applicable)
   - [ ] Clear offline indicator or message is shown
   - [ ] Features that require network show appropriate messaging (not infinite spinners)
2. Start with network, then enable airplane mode mid-use
   - [ ] In-progress operations fail gracefully with retry option
   - [ ] No data loss from interrupted operations
   - [ ] App does not freeze or become unresponsive

**Slow network (Network Link Conditioner):**

1. Set Network Link Conditioner to "Very Bad Network" profile
   - [ ] Loading indicators appear (not just blank screens)
   - [ ] Timeouts trigger after a reasonable period (not 60+ seconds of waiting)
   - [ ] Images load progressively or show placeholders
   - [ ] The app remains interactive while content loads (UI is not blocked)

**Network recovery:**

1. Start in airplane mode, then restore network
   - [ ] App recovers without requiring a force-quit and relaunch
   - [ ] Pending operations retry or prompt the user
   - [ ] Data refreshes to current state

### 3.8 Compile Platform Compatibility Report

Summarize findings across all devices and configurations in a structured report.

```text
## Platform Compatibility Report

### Test Date: YYYY-MM-DD
### App Version: X.Y.Z (Build NNN)

### Device Results

| Device | iOS | Status | Issues Found |
| ------ | --- | ------ | ------------ |
| iPhone SE (3rd gen) | 18.2 | PASS with issues | BUG-015, BUG-016 |
| iPhone 16 | 18.2 | PASS | None |
| iPhone 16 Pro Max | 18.2 | PASS | None |
| iPhone 16 (sim) | 17.7 | PASS with issues | BUG-017 |

### Configuration Results

| Configuration | Status | Issues Found |
| ------------- | ------ | ------------ |
| Dark mode | PASS with issues | BUG-018 |
| Dynamic Type (large) | PASS | None |
| Dynamic Type (accessibility) | FAIL | BUG-019, BUG-020 |
| Reduce Motion | PASS | None |
| Airplane mode | PASS | None |
| Slow network | PASS with issues | BUG-021 |

### Summary
- Total issues found: 7
- Critical: 0 | High: 2 | Medium: 4 | Low: 1
- Blocking ship: BUG-019 (accessibility layout break), BUG-020 (accessibility layout break)
```

## Anti-Patterns

- **Simulator-only testing** — The simulator uses your Mac's CPU, GPU, and RAM. It does not replicate real device constraints. An animation that runs at 60fps on a MacBook Pro may drop frames on an iPhone SE. Always test performance-sensitive flows on real hardware.
- **Testing only on your personal device** — Your device is the mid-range or high-end device you bought for yourself. It is not representative of the cheapest, oldest device your users might have.
- **Skipping Dark mode** — A significant portion of users run Dark mode full-time. White text on white backgrounds, invisible icons, and flashing light-mode screens during transitions are all bugs they'll see immediately.
- **Testing Dynamic Type at one extra size** — Going from default to one size larger catches nothing. The real breaks happen at the largest standard size and the accessibility sizes. Test the extremes.
- **Fixing layout during platform testing** — Document the issues, finish the test pass, then fix in batch. Fixing mid-test changes behavior and can mask other issues.
- **Ignoring Reduce Motion** — Motion sensitivity affects real users. Animations that trigger vestibular discomfort are accessibility failures, not preferences.
- **Assuming Wi-Fi everywhere** — Many users are on cellular with variable speeds. Some users are in areas with poor connectivity. Test with Network Link Conditioner, not just airplane mode.

## Deliverable

**Platform Compatibility Report** saved to `docs/testing/platform-compatibility-report.md` containing:

- Pass/fail results per device, iOS version, and configuration
- Bug list with device/configuration-specific reproduction steps
- Ship-blocking issues identified

## Definition of Done

- [ ] Critical flows tested on smallest device (iPhone SE or equivalent)
- [ ] Critical flows tested on largest device (iPhone 16 Pro Max or equivalent)
- [ ] Critical flows tested on current iOS version
- [ ] Critical flows tested on current-1 iOS version (simulator acceptable if no physical device)
- [ ] Full app tested in Dark mode — no invisible text, broken images, or light-mode flashes
- [ ] Dynamic Type tested at largest standard size and largest accessibility size
- [ ] Reduce Motion tested — animations replaced or removed
- [ ] Airplane mode tested — app does not crash, shows appropriate offline state
- [ ] Slow network tested — loading indicators appear, timeouts are reasonable
- [ ] Network recovery tested — app recovers without force-quit
- [ ] All platform-specific bugs documented with device info and reproduction steps
- [ ] Platform compatibility report saved to `docs/testing/platform-compatibility-report.md`

## Tips

- **Borrow devices if you don't own them.** Ask friends, family, or colleagues to borrow an iPhone SE or older device for a day. One day of real-device testing catches more bugs than a week on the simulator.
- **Use Xcode's Accessibility Inspector.** It shows VoiceOver labels, Dynamic Type behavior, and contrast ratios without needing to toggle settings on a real device. Use it as a fast first pass before testing on-device.
- **Test Dark mode and Light mode back-to-back.** Switch between them rapidly. Some bugs only appear during the transition, not in steady state — cached colors, theme-unaware custom views, and flash-of-wrong-theme on launch.
- **Screenshot everything.** Take screenshots on each device at each configuration. Side-by-side comparisons reveal issues you wouldn't notice testing sequentially.
- **Network Link Conditioner profiles are on-device.** Go to Settings → Developer → Network Link Conditioner. If you don't see Developer settings, connect the device to Xcode and it will appear.
- **Prioritize real devices for performance, simulator for layout.** If you only have one physical device, use it for performance and permissions testing. Use the simulator's device options (iPhone SE, Pro Max) for layout testing — layout bugs are visible even though performance bugs are not.
