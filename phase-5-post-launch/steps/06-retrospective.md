# Step 6: Retrospective

## Purpose

Conduct a full project retrospective — not a sprint-level or phase-level retro, but a look back across the entire journey from concept to live product. This captures the high-level lessons that only become visible after you've shipped and observed real users.

## Process

### 6.1 Set the Scope

This retrospective covers the full project lifecycle: Phase 0 through Phase 5. It's bigger than a sprint retro, so budget 1-2 hours and give it the attention it deserves.

**When to run:**

- After the first 30 days post-launch (enough time to observe real user behavior)
- After a major milestone (e.g., reaching a revenue or user target)
- Before starting a new project (carry lessons forward)

### 6.2 Review Each Phase

Walk through each phase and capture what went well and what didn't. Be specific — "Phase 1 was hard" isn't useful. "Phase 1 Step 3 produced an architecture document that didn't match reality by Sprint 3" is.

**Phase 0 — Concept:**

- Did the concept hold up? Was the core value proposition validated by real users?
- Were there surprises in what users actually valued vs what you predicted?
- Was the Go/No-Go decision correct?

**Phase 1 — Pre-Production:**

- Which specs were most referenced during production? Which were never opened again?
- Did the architecture survive contact with real development, or did it need significant changes?
- Was the project plan realistic? How far off were the estimates?

**Phase 2 — Production:**

- Did the sprint process work? Were sprints the right length?
- Where did the most rework happen? What caused it?
- Was the 40/40/20 split (bugs/features/debt) maintained, or did one category dominate?

**Phase 3 — Testing & QA:**

- Were bugs found in testing or by users? (Found in testing = process working. Found by users = testing gaps.)
- Was beta testing valuable? Did it surface issues that internal testing missed?
- How much rework came from QA? Was it expected or surprising?

**Phase 4 — Launch:**

- Did the submission process go smoothly? Any rejections?
- Were store assets effective? What was the conversion rate?
- Was the launch timeline realistic?

**Phase 5 — Post-Launch:**

- Did monitoring catch issues before users reported them?
- Were the right metrics being tracked? Were any important signals missing?
- How well did the feedback loop work?

### 6.3 Identify Cross-Phase Patterns

Some lessons only emerge when looking across the full project. Look for:

- **Decisions that compounded.** A shortcut in Phase 1 that created ongoing pain in Phase 2-3.
- **Specs that drifted.** Documents that were accurate when written but became stale during production.
- **Processes that earned their keep.** Steps that felt bureaucratic but prevented real problems.
- **Processes that didn't.** Steps that consumed time without producing useful output.

### 6.4 Feed Improvements Back to the Playbook

The most valuable output of this retrospective is Playbook improvements. Every project that uses the Playbook should make it better for the next one.

**For each improvement, specify:**

- Which Playbook file to change (step guide, template, or README)
- What to add, modify, or remove
- Why — what happened that this change would have prevented or improved

```markdown
### Playbook Improvements
- [File path] — [What to change] — [Why]
- [File path] — [What to change] — [Why]
```

Don't just list problems. Write the solution. "Phase 1 Step 3 should include X" is actionable. "Phase 1 Step 3 was incomplete" is not.

### 6.5 Document Lessons for Future Reference

Write down the lessons that are project-specific (not Playbook changes) but worth remembering.

**Format:** What happened → What we learned → What to do next time.

These lessons live in the project's retrospective document and serve as a reference if you build something similar in the future.

## Deliverable

Complete the [Project Retrospective Template](../templates/project-retrospective.md) and save it to your project's `docs/post-launch/project-retrospective.md`.

## Definition of Done

- [ ] Each phase (0-5) reviewed with specific observations
- [ ] Cross-phase patterns identified
- [ ] Playbook improvements documented with specific file paths and changes
- [ ] Project-specific lessons recorded
- [ ] Retrospective document committed to version control

## Tips

- **Do this while memories are fresh.** The best time is 30 days post-launch, not 6 months later when the details have faded.
- **Honesty is the point.** A retrospective that says "everything went great" is either a perfect project (unlikely) or a missed opportunity to learn.
- **Focus on systemic issues, not one-off mistakes.** A typo that caused a bug isn't a process failure. An architecture pattern that caused repeated bugs across multiple sprints is.
- **Actually update the Playbook.** The retrospective is only as valuable as the changes it produces. If you identify an improvement, make the edit before closing the retro.
- **Keep it to 1-2 hours.** A retrospective that takes a full day is too detailed. Capture the big lessons, not every minor observation.
