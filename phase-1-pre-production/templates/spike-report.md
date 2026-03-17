# Spike Report: _[Spike Name]_

> Playbook Reference: [Step 2: Technical Spikes](../steps/02-technical-spikes.md)

## Project

**App Name:** _[Name]_
**Date:** _[YYYY-MM-DD]_
**Time Spent:** _[Hours]_ / _[Time Box]_

---

## Spike Definition

| Parameter | Value |
| ----------- | ------- |
| **Question** | _[The specific question this spike answers]_ |
| **Time Box** | _[Maximum time allocated]_ |
| **Success Criteria** | _[What "validated" looks like — concrete, measurable]_ |
| **Failure Criteria** | _[What would make you abandon this approach]_ |
| **What It Validates** | _[Requirements, architecture decisions, or risks this informs]_ |

### Approach

_[Brief description of what you built to answer the question.]_

---

## Verdict

**Result:** _[Validated / Partially Validated / Failed / Inconclusive]_

**One-sentence summary:** _[What you learned in one sentence.]_

---

## Findings

### What Worked

_[Concrete findings that confirm the approach. Include specifics — timings, data sizes, API behavior.]_

### What Didn't Work

_[Surprises, blockers, limitations. What was harder than expected? What assumptions were wrong?]_

### Performance Data

| Metric | Value | Acceptable? |
| -------- | ------- | ------------- |
| _[e.g., Data load time]_ | _[e.g., 1.2 seconds]_ | _[Yes / No — against success criteria]_ |
| | | |
| | | |

---

## Architectural Recommendations

_[Based on spike findings, how should this be built in production? What patterns, libraries, or approaches should be used? What should be avoided?]_

---

## Impact on Requirements

_[Do any user stories or acceptance criteria need updating based on these findings? List specific changes.]_

| Story | Change | Reason |
| ------- | -------- | -------- |
| _[Story ID]_ | _[What changes]_ | _[Why, based on spike findings]_ |
| | | |

---

## Impact on Risk Register

| Risk | Update |
| ------ | -------- |
| _[Risk from Phase 0]_ | _[Mitigated / Elevated / Unchanged / New risk discovered]_ |
| | |

---

## Code Artifacts

**Location:** _[Path to spike code, e.g., spikes/route-pipeline/]_

**Key files:**
- _[file]_ — _[what it demonstrates]_
- _[file]_ — _[what it demonstrates]_

**Reusable learnings:** _[Any code patterns or configurations worth preserving for production]_

---

## Screenshots / Evidence

_[Screenshots, terminal output, or other evidence supporting the findings.]_

---

## Status

- [ ] Spike executed within time box
- [ ] Findings documented with concrete data
- [ ] Architectural recommendations written
- [ ] Requirement impacts identified
- [ ] Risk register updates noted
- [ ] Code artifacts saved and linked
