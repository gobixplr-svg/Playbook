# Step 3: Iteration Planning

## Purpose

Decide what to build next based on data and feedback, not guesses. This step bridges monitoring (Step 1) and feedback (Step 2) into a concrete plan that feeds back into the sprint execution process from Phase 2.

## Process

### 3.1 Review Inputs

Before planning the next iteration, gather all the signals from the past cycle.

**Input sources:**

| Source | What to Look For |
| ------ | ---------------- |
| Analytics (Step 1) | Metrics trending down, drop-off points in funnels, underused features |
| Feedback log (Step 2) | Top-priority feature requests, recurring bugs, UX confusion patterns |
| Crash reports | Crash clusters affecting many users or specific OS versions |
| App Store reviews | Sentiment trends, new complaint themes since last update |
| Competitor activity | New features from competitors that affect your positioning |

Spend 30-60 minutes reviewing these inputs. Don't plan from memory — open the dashboards and read the feedback log.

### 3.2 Classify Work

All post-launch work falls into three categories. A healthy iteration balances all three.

| Category | What It Covers | Recommended Share |
| -------- | -------------- | ----------------- |
| Bug Fixes | Crashes, broken features, regressions | ~40% |
| Features | New capabilities, feature requests, UX improvements | ~40% |
| Tech Debt | Refactoring, dependency updates, performance, test coverage | ~20% |

**The 40/40/20 split is a starting point, not a rule.** Adjust based on reality:

- Post-launch week 1-2: Shift to 60/20/20 (bugs dominate early)
- Stable app with growth goals: Shift to 20/60/20 (features dominate)
- Before a major feature: Shift to 20/40/40 (pay down debt first)

The anti-pattern is 0% tech debt for months, then a "refactoring sprint" that breaks everything. Steady 20% prevents this.

### 3.3 Update the Roadmap

Update `docs/ROADMAP.md` with the new priorities. Follow the roadmap format from CLAUDE.md:

1. Move completed items to the Done section with completion dates
2. Reprioritize the Backlog based on the review in 3.1
3. Move the next batch of work to Active
4. Add new items surfaced from feedback and analytics

**When reprioritizing, consider:**

- RICE scores from the feedback log
- Severity of open bugs
- Dependencies (does Feature B require Feature A first?)
- User-facing impact vs internal improvement

### 3.4 Plan the Next Sprint

Use the Phase 2 sprint execution process to plan the next sprint. The process is the same whether you're in Phase 2 or Phase 5 — the only difference is where the work comes from.

**Sprint planning steps:**

1. **Pick a sprint theme** — What's the headline for this iteration? (e.g., "Stability + CSV Export")
2. **Select stories from the roadmap** — Pull from Active section, respecting the 40/40/20 balance
3. **Break stories into tasks** — Same task breakdown process as Phase 2
4. **Estimate effort** — Use the same estimation approach you've been using
5. **Identify risks** — New API dependencies, OS version constraints, breaking changes

**Sprint scope for post-launch:**

Post-launch sprints are typically shorter than Phase 2 sprints. Aim for 1-2 week sprints with a clear, shippable outcome at the end of each one.

### 3.5 Communicate the Plan

If you have users, beta testers, or stakeholders who are waiting on specific features or fixes:

- Update any public roadmap or changelog
- Respond to App Store reviews or support emails that asked for items you're now planning to build
- Don't share timelines unless you're confident — share intent ("we're working on this") rather than dates

## Deliverable

Updated `docs/ROADMAP.md` and a sprint plan for the next iteration. The sprint plan can live in your sprint tracker (Phase 2 template) or directly in the roadmap.

## Definition of Done

- [ ] Analytics and feedback inputs reviewed (not from memory — from the actual data)
- [ ] Work classified into bug fixes, features, and tech debt with approximate balance
- [ ] ROADMAP.md updated with new priorities and completed items
- [ ] Next sprint planned with theme, stories, and task breakdowns
- [ ] Risks and dependencies identified

## Tips

- **Plan from data, not feelings.** "I think users want X" is weaker than "12 users requested X and the feature has a RICE score of 450." Open the dashboards before you plan.
- **The 40/40/20 split prevents debt spirals.** It feels wasteful to spend 20% on tech debt when there are features to ship. But skipping it for three months creates a codebase that's painful to change — and then features take 3x longer.
- **Short sprints keep you honest.** Two-week sprints force you to ship something. Month-long sprints invite scope creep and deferred decisions.
- **Not everything needs a sprint.** A critical crash fix doesn't wait for sprint planning. Hotfix it, then account for the time in the current sprint.
- **Review the plan with fresh eyes.** Read the sprint plan the next morning. If the scope feels ambitious after a night's sleep, it is.
