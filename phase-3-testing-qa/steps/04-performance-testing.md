# Step 4: Performance Testing

## Purpose

Profile the app with Xcode Instruments to verify it meets performance benchmarks for launch time, memory usage, scroll frame rate, battery impact, and bundle size. By the end of this step, you have measured data — not impressions — proving the app performs well enough to ship.

## Why This Matters

Performance bugs are invisible during development. The app feels fast on your M-series Mac running the simulator with 16 GB of RAM and a gigabit connection. On a real device with 4 GB of RAM, cellular data, and 20 other apps in memory, the experience is different.

Users do not report performance bugs — they just stop using the app. A 4-second launch time, stuttering scroll, or battery drain that kills 10% per hour results in deletion, not a bug report. Performance testing catches these issues before users encounter them.

Performance testing also catches memory leaks, which are particularly insidious because the app works fine during short test sessions but crashes after extended real-world use as leaked allocations accumulate.

## Prerequisites

- Functional testing (Step 2) and platform testing (Step 3) complete
- A physical test device — performance profiling on the simulator is meaningless because it uses Mac hardware
- Xcode Instruments installed (included with Xcode)
- The oldest/lowest-spec device you support available for testing (iPhone SE 3rd gen or equivalent)

## Process

### 4.1 Define Performance Benchmarks

Before profiling, define the specific numbers you're targeting. Vague goals like "the app should be fast" are not measurable. Set concrete thresholds.

**Recommended benchmarks for iOS apps:**

| Metric | Target | Unacceptable | How to Measure |
| ------ | ------ | ------------ | -------------- |
| Cold launch to interactive | < 2 seconds | > 4 seconds | Time Profiler, manual stopwatch |
| Warm launch to interactive | < 1 second | > 2 seconds | Time Profiler |
| Memory (steady state) | < 100 MB | > 200 MB | Allocations instrument |
| Memory (peak) | < 150 MB | > 300 MB | Allocations instrument |
| Scroll frame rate | 60 fps sustained | < 45 fps or visible stutter | Core Animation instrument |
| Memory leaks | 0 leaks | Any leak in navigation flows | Leaks instrument |
| Battery (active use) | < 10% per hour | > 20% per hour | Energy Log instrument |
| App bundle size | < 30 MB | > 100 MB | Xcode build report, App Store Connect |

**Adjust these based on your app's complexity.** A map-heavy app will use more memory than a list-based app. An app with offline media will have a larger bundle. The point is to set the numbers before measuring so you know whether you've passed or failed.

### 4.2 Profile App Launch (Time Profiler)

App launch time is the first impression. Users expect apps to be interactive within 2 seconds.

**Measurement process:**

1. Connect the oldest supported device via USB
2. In Xcode, choose Product → Profile (or Cmd+I)
3. Select the **Time Profiler** instrument
4. Force-quit the app on device to ensure a cold launch
5. Press Record in Instruments, then launch the app on device
6. Stop recording once the app is fully interactive (first meaningful content visible)

**What to look for:**

- **Total launch time** — from process start to first frame rendered
- **Main thread blocking** — any work on the main thread that delays UI rendering
- **Heavy initializers** — classes or services that do expensive work in `init()` or `viewDidLoad()`
- **Synchronous network calls** — any network request that blocks launch
- **Database migrations** — schema migrations that run on first launch after an update

**Common launch time fixes:**

| Problem | Fix |
| ------- | --- |
| Loading all data at launch | Lazy-load data only when the screen that needs it appears |
| Synchronous network calls | Move to async, show cached/placeholder data immediately |
| Heavy `AppDelegate` / `@main` setup | Defer non-critical initialization to after first frame |
| Large asset loading | Load images asynchronously, use thumbnails for lists |
| Database migration on main thread | Run migrations on a background thread with a loading indicator |

### 4.3 Profile Memory Usage (Allocations)

Memory issues cause crashes on low-memory devices and degrade system-wide performance.

**Measurement process:**

1. In Instruments, select the **Allocations** instrument
2. Launch the app and perform a typical user session (5-10 minutes of normal use)
3. Monitor the **All Heap Allocations** graph for the steady-state memory level
4. Navigate through all major features — watch for spikes
5. Return to the home screen — memory should return close to the initial level

**What to look for:**

- **Steady-state memory** — the baseline memory usage when the app is idle on the home screen
- **Peak memory** — the highest memory usage during any operation
- **Growth over time** — if memory increases with each navigation cycle and never decreases, you have a leak or retain cycle
- **Image memory** — large images loaded at full resolution instead of display size

**Memory budget guidance:**

| Device RAM | Your App Budget (Approximate) |
| ---------- | ----------------------------- |
| 4 GB (iPhone SE 3rd gen) | 100-120 MB |
| 6 GB (iPhone 15) | 150-180 MB |
| 8 GB (iPhone 16 Pro) | 200-250 MB |

iOS will terminate your app if it exceeds the system's memory limit, which varies by device and system load. Stay well under the limit — your app is not the only thing running.

### 4.4 Check for Memory Leaks (Leaks Instrument)

Memory leaks cause gradual memory growth that eventually leads to crashes during extended use.

**Measurement process:**

1. In Instruments, select the **Leaks** instrument (can run alongside Allocations)
2. Perform repetitive navigation cycles:
   - Push to a detail view → pop back (repeat 10 times)
   - Present a modal → dismiss it (repeat 10 times)
   - Open a feature → close it → open again (repeat 10 times)
3. Each cycle should return memory to approximately the same level
4. The Leaks instrument will flag any leaked allocations with a red checkmark

**Common leak sources in SwiftUI / UIKit:**

| Leak Pattern | Cause | Fix |
| ------------ | ----- | --- |
| Strong reference cycle in closures | Closure captures `self` strongly | Use `[weak self]` in closures that outlive the current scope |
| Delegate retain cycles | Delegate property is `strong` instead of `weak` | Declare delegate properties as `weak var delegate` |
| Timer not invalidated | `Timer.scheduledTimer` holds a strong reference | Invalidate timers in `deinit` or `onDisappear` |
| NotificationCenter observer not removed | Observer keeps the object alive | Use the block-based API that returns a token, remove in `deinit` |
| Closure-based Combine subscriptions | `sink` closure captures self | Store `AnyCancellable` and use `[weak self]` |

**Test the most common leak scenario:** Navigate to a screen, navigate back, check if the screen's view controller / view model is deallocated. If not, you have a retain cycle.

### 4.5 Profile Scroll Performance (Core Animation)

Stuttering scrolls are immediately noticeable and feel broken. Target 60 fps consistently.

**Measurement process:**

1. In Instruments, select the **Core Animation** instrument (or **Animation Hitches** in newer Xcode versions)
2. Navigate to a list or scrollable view
3. Scroll slowly, then quickly, then fling and let it decelerate
4. Watch the frame rate graph — sustained 60 fps with no drops below 45 fps

**Common scroll performance issues:**

| Problem | Symptom | Fix |
| ------- | ------- | --- |
| Synchronous image loading | Stutter when new cells appear | Use `AsyncImage` or a caching library, load in background |
| Complex cell layouts | Consistent frame drops during scroll | Simplify cell hierarchy, pre-compute layouts |
| Transparency and blending | Frame drops with overlapping translucent views | Minimize transparent layers, use opaque backgrounds |
| Shadow rendering | Frame drops on cells with shadows | Use `shadowPath` for pre-computed shadow shapes |
| Large images in cells | Memory spike and stutter | Resize images to cell display size before rendering |

### 4.6 Measure Battery Impact (Energy Log)

Battery drain makes users uninstall your app. Even if functionality is perfect, killing the battery is a dealbreaker.

**Measurement process:**

1. In Instruments, select the **Energy Log** instrument
2. Use the app normally for 15-30 minutes of active use
3. Monitor the energy impact level (Low / Medium / High / Very High)
4. Check for background activity after leaving the app

**What to look for:**

- **CPU wake-ups** — the app should not continuously poll in the background
- **Location services** — using `kCLLocationAccuracyBest` when `kCLLocationAccuracyHundredMeters` would suffice
- **Network activity** — unnecessary polling, downloading data that is never displayed
- **Background refresh** — tasks running more frequently than needed
- **Animation** — animations running when the app is not visible

**Common battery fixes:**

| Problem | Fix |
| ------- | --- |
| Continuous location updates | Use significant change monitoring or reduce accuracy |
| Polling for updates | Use push notifications or server-sent events |
| Animations running in background | Stop animations in `sceneDidEnterBackground` |
| Unnecessary background refresh | Reduce fetch interval or eliminate if not needed |

### 4.7 Check Bundle Size

App bundle size affects download time, storage usage, and whether your app can be downloaded over cellular (the App Store limits cellular downloads to 200 MB).

**Measurement process:**

1. In Xcode, build for a generic iOS device (Product → Build For → Any iOS Device)
2. Check the `.app` bundle size in the build folder
3. In App Store Connect, check the estimated download size after uploading (this applies App Thinning)

**Size reduction checklist:**

- [ ] Images use asset catalogs with appropriate scale factors (not 3x images used at 1x)
- [ ] No unused assets in the bundle (images, fonts, data files referenced by nothing)
- [ ] Large assets (maps, datasets) are downloaded on demand, not bundled
- [ ] Third-party frameworks are necessary — remove any unused dependencies
- [ ] Debug symbols and dSYMs are not included in the release build
- [ ] Enable Bitcode and App Thinning in build settings

### 4.8 Test Network Request Efficiency

Inefficient network usage wastes battery, data, and time.

**What to check:**

1. **Request count** — Use Charles Proxy or Instruments Network instrument to count requests during a typical session. Look for redundant or duplicate requests.
2. **Payload size** — Are you downloading full objects when you only need a few fields? Are images full-resolution when thumbnails would suffice?
3. **Caching** — Are responses cached appropriately? Tapping back and forward should not re-fetch data that hasn't changed.
4. **Pagination** — Lists with many items should paginate, not load everything at once.
5. **Retry logic** — Failed requests should retry with exponential backoff, not hammer the server.

### 4.9 Compile Performance Benchmarks Report

Summarize all measurements in a structured report.

```text
## Performance Benchmarks Report

### Test Date: YYYY-MM-DD
### App Version: X.Y.Z (Build NNN)
### Test Device: iPhone SE (3rd gen), iOS 18.2

### Results

| Metric | Target | Measured | Status |
| ------ | ------ | -------- | ------ |
| Cold launch | < 2s | 1.8s | PASS |
| Warm launch | < 1s | 0.6s | PASS |
| Memory (steady state) | < 100 MB | 85 MB | PASS |
| Memory (peak) | < 150 MB | 142 MB | PASS (marginal) |
| Scroll frame rate | 60 fps | 58-60 fps | PASS |
| Memory leaks | 0 | 0 | PASS |
| Battery (active use) | < 10%/hr | ~7%/hr | PASS |
| Bundle size | < 30 MB | 22 MB | PASS |

### Issues Found
- BUG-025: Memory peak hits 142 MB when loading route map with full-resolution tiles.
  Recommendation: Use lower-resolution tiles at default zoom.
- Network: 3 redundant API calls on home screen load. Consolidate into single query.

### Tested On Oldest Supported Device: Yes (iPhone SE 3rd gen)
```

## Anti-Patterns

- **Profiling on the simulator** — The simulator uses Mac hardware. A test that passes on the simulator tells you nothing about real-device performance. Always profile on physical hardware, preferably the oldest device you support.
- **Profiling on the newest device** — Your iPhone 16 Pro Max has an A18 Pro chip and 8 GB of RAM. Your users might have an iPhone SE with an A15 and 4 GB. Always test on the lowest-spec device in your support matrix.
- **Optimizing without measuring** — "I think this screen is slow" leads to premature optimization of the wrong thing. Measure first with Instruments, identify the actual bottleneck, then fix it. Data beats intuition.
- **Ignoring memory leaks because the app doesn't crash** — Leaks accumulate over time. A 0.5 MB leak per navigation cycle means 50 MB lost after 100 navigations. Your 30-minute test session might not trigger a crash, but a user's day-long session will.
- **Testing only short sessions** — Performance bugs often only appear after extended use: memory growth, leaked resources, accumulated cached data. Run the app for 30+ minutes with realistic usage patterns.
- **Skipping battery profiling** — "My app isn't doing anything in the background" is an assumption. A forgotten timer, a continuous location subscription, or a Combine publisher that never cancels can drain battery silently.
- **Accepting marginal results** — If your benchmark target is 100 MB and you're at 98 MB, you're one feature away from failure. Marginal passes should be flagged for optimization even if they technically pass.

## Deliverable

**Performance Benchmarks Report** saved to `docs/testing/performance-benchmarks-report.md` containing:

- Measured values for each benchmark metric
- Pass/fail status against defined thresholds
- Issues found with specific Instruments data
- Test device specifications
- Recommendations for any marginal or failing metrics

## Definition of Done

- [ ] Performance benchmarks defined with specific thresholds before testing
- [ ] App launch time measured on oldest supported device (cold and warm)
- [ ] Memory usage profiled with Allocations instrument (steady state and peak)
- [ ] Memory leak check completed with Leaks instrument on repeated navigation cycles
- [ ] Scroll performance measured with Core Animation / Animation Hitches instrument
- [ ] Battery impact measured with Energy Log instrument (minimum 15-minute session)
- [ ] Bundle size checked against target
- [ ] Network request efficiency reviewed (request count, payload size, caching)
- [ ] All profiling done on a physical device (not simulator)
- [ ] All profiling done on oldest supported device (not newest)
- [ ] Performance benchmarks report saved to `docs/testing/performance-benchmarks-report.md`
- [ ] Any failing or marginal benchmarks logged as bugs with fix recommendations

## Tips

- **Profile on the oldest device you support, not the newest.** If the app performs well on an iPhone SE, it will perform well everywhere. The reverse is not true.
- **Use Instruments templates, not custom setups.** Xcode ships with pre-configured Instruments templates (Time Profiler, Allocations, Leaks, Energy Log). Use them as-is — they are already configured for the most common analysis tasks.
- **Profile release builds, not debug builds.** Debug builds have optimizations disabled, assertions enabled, and additional logging. Profile with the Release configuration to get accurate measurements. In Xcode: Product → Scheme → Edit Scheme → Run → Build Configuration → Release.
- **Memory leaks compound.** A single 1 KB leak is harmless. That same leak triggered on every navigation cycle becomes 1 MB after 1000 navigations. Test by repeating the same flow many times and watching the allocation graph.
- **Launch time includes your code AND framework initialization.** Third-party SDKs that initialize eagerly (analytics, crash reporting, feature flags) add to launch time. Defer their initialization until after the first frame renders.
- **Battery profiling requires patience.** You need at least 15 minutes of continuous use to get meaningful battery data. Shorter sessions produce noisy results. Set up the test, use the app naturally, and let Instruments collect data.
- **AI tools can help analyze Instruments traces.** Export the Instruments trace, describe the flame graph or allocation chart to an AI assistant, and ask for optimization suggestions. This is faster than manually analyzing complex traces.
