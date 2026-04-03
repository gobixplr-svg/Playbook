# Phase 3: Testing & QA

## Purpose

Prove the app works correctly, performs well, and meets the acceptance criteria defined in Pre-Production. This phase answers: **Is it ready to ship?**

## When to Enter

- Phase 2 Production is complete
- All "Must Have" features are implemented
- App builds and runs on all target platforms

## Steps

| Step | Name | Purpose | Key Output |
| ---- | ---- | ------- | ---------- |
| 1 | [Test Plan](steps/01-test-plan.md) | Define what to test, how, and acceptance criteria | Test Plan Document |
| 2 | [Functional Testing](steps/02-functional-testing.md) | Verify all features work as specified | Test results + bug list |
| 3 | [Platform Testing](steps/03-platform-testing.md) | Test on real devices across platforms and OS versions | Platform compatibility report |
| 4 | [Performance Testing](steps/04-performance-testing.md) | Profile and optimize for speed, memory, battery | Performance benchmarks |
| 5 | [Security & Privacy Audit](steps/05-security-privacy-audit.md) | Verify data handling, auth, and compliance | Security audit report |
| 6 | [Beta Testing](steps/06-beta-testing.md) | External testers via TestFlight / internal tracks | User feedback + bug reports |
| 7 | [Bug Fix Sprint](steps/07-bug-fix-sprint.md) | Address all critical and high-priority bugs | Clean bug tracker |

## Exit Criteria

Phase 3 is complete when:

- [ ] All acceptance criteria from requirements are verified
- [ ] No critical or high-priority bugs remain open
- [ ] App performs within defined benchmarks (launch time, memory, battery)
- [ ] Privacy and security audit passes
- [ ] Beta testers have validated core flows
- [ ] App Store / Play Store compliance is verified

## Key Principles

- **Test on real devices.** Simulators miss real-world issues (performance, permissions, network conditions).
- **Beta test with real users.** Your assumptions about UX will be wrong. Find out where before launch.
- **Fix the cause, not the symptom.** When a bug is found, understand why before patching.
- **Compliance is not optional.** App Store rejections cost weeks. Audit before submitting.

## Token Optimization

See the full [AI Token Optimization Guide](../docs/ai-token-optimization.md) for universal techniques.

**Phase 3 profile:** Lots of test execution and log reading. Repetitive patterns.

- **Use hooks to filter test output** — show only failures, not full suite output (the #1 token waste in QA)
- **Delegate test runs to subagents** — keep verbose output out of your main context
- **Batch related bugs by module** — one session per module, not one session per bug
- **Summarize profiler output before pasting** — raw Instruments/profiler data is extremely token-expensive

## Estimated Time

- **Solo dev, simple app:** 1-2 weeks
- **Solo dev, complex app:** 2-4 weeks
- **Team, complex app:** 3-6 weeks
