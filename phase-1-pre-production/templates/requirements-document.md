# Requirements Document

> Playbook Reference: [Step 1: Requirements Specification](../steps/01-requirements-specification.md)

## Project

**App Name:** _[Name]_
**Date:** _[YYYY-MM-DD]_
**Author:** _[Name]_
**Phase 0 Reference:** _[Link to Concept Document]_

---

## Phase 0 Review Notes

_Document any changes, new insights, or resolved research items since the Concept Document was written. If nothing has changed, write "No changes since Phase 0."_

---

## Epics Overview

| # | Epic | Description | Stories | Must Have | Should Have |
| --- | ------ | ------------- | --------- | ----------- | ------------- |
| E1 | _[Name]_ | _[What this epic delivers]_ | _[count]_ | _[count]_ | _[count]_ |
| E2 | | | | | |
| E3 | | | | | |
| E4 | | | | | |
| E5 | | | | | |
| E6 | | | | | |
| E7 | | | | | |

---

## Epic 1: _[Name]_

_[One-sentence description of what this epic delivers to the user.]_

### Story E1-S1: _[Title]_

**Priority:** Must Have / Should Have
**Concept Reference:** _[Feature from Concept Document MoSCoW list]_

> **As a** _[user type]_, **I want** _[capability]_ **so that** _[benefit]_.

**Acceptance Criteria:**

| # | Given | When | Then | Test Type |
| --- | ------- | ------ | ------ | ----------- |
| AC1 | _[precondition]_ | _[action]_ | _[outcome]_ | Unit / Integration / UI / Snapshot |
| AC2 | | | | |
| AC3 | | | | |

**Edge Cases:**
- _[Edge case and expected behavior]_

**Notes:** _[Implementation hints, constraints, or open questions — optional]_

---

### Story E1-S2: _[Title]_

**Priority:** Must Have / Should Have
**Concept Reference:** _[Feature from Concept Document MoSCoW list]_

> **As a** _[user type]_, **I want** _[capability]_ **so that** _[benefit]_.

**Acceptance Criteria:**

| # | Given | When | Then | Test Type |
| --- | ------- | ------ | ------ | ----------- |
| AC1 | _[precondition]_ | _[action]_ | _[outcome]_ | Unit / Integration / UI / Snapshot |
| AC2 | | | | |
| AC3 | | | | |

**Edge Cases:**
- _[Edge case and expected behavior]_

**Notes:** _[optional]_

---

_[Repeat Story blocks as needed for this epic]_

---

## Epic 2: _[Name]_

_[Repeat Epic + Story structure for each epic]_

---

## Edge Cases & Constraints

### Platform Constraints

| Constraint | Source | Impact | Mitigation |
| ------------ | -------- | -------- | ------------ |
| _[e.g., HealthKit background refresh limits]_ | _[Platform/API]_ | _[What it affects]_ | _[How to handle]_ |
| | | | |
| | | | |

### Business Constraints

| Constraint | Description | Stories Affected |
| ------------ | ------------- | ----------------- |
| _[e.g., Free vs. premium feature boundary]_ | _[Details]_ | _[Story IDs]_ |
| | | |
| | | |

### Privacy & Compliance

| Requirement | Description | Stories Affected |
| ------------- | ------------- | ----------------- |
| _[e.g., HealthKit usage description required]_ | _[Details]_ | _[Story IDs]_ |
| | | |

---

## Dependencies & Sequencing

### Dependency Map

_List stories that block other stories:_

| Story | Blocks | Reason |
| ------- | -------- | -------- |
| _[Story ID]_ | _[Story IDs]_ | _[Why it must come first]_ |
| | | |
| | | |

### Walking Skeleton

_The minimal set of stories that produce a functioning (if bare-bones) app:_

1. _[Story ID]_ — _[Title]_
2. _[Story ID]_ — _[Title]_
3. _[Story ID]_ — _[Title]_
4. _[Story ID]_ — _[Title]_

### Suggested Build Order

_Recommended sequence for development (feeds into Step 7: Project Plan):_

| Phase | Stories | Rationale |
| ------- | --------- | ----------- |
| Sprint 1 | _[Story IDs]_ | _[Walking skeleton / foundation]_ |
| Sprint 2 | _[Story IDs]_ | _[Core features]_ |
| Sprint 3 | _[Story IDs]_ | _[Remaining Must Haves]_ |
| Sprint 4 | _[Story IDs]_ | _[Should Haves if time allows]_ |

---

## Test Plan Summary

| Test Type | Story Count | Estimated Test Count | Infrastructure Needed |
| ----------- | ------------- | --------------------- | ---------------------- |
| Unit | _[count]_ | _[count]_ | XCTest |
| Integration | _[count]_ | _[count]_ | XCTest + mock services |
| UI | _[count]_ | _[count]_ | XCUITest |
| Snapshot | _[count]_ | _[count]_ | swift-snapshot-testing |
| **Total** | | | |

---

## Status

- [ ] Phase 0 outputs reviewed
- [ ] Epics defined
- [ ] All Must Have stories written with acceptance criteria
- [ ] All Should Have stories written with acceptance criteria
- [ ] Test types assigned to all acceptance criteria
- [ ] Edge cases documented
- [ ] Platform and business constraints documented
- [ ] Dependency map created
- [ ] Walking skeleton identified
- [ ] Suggested build order drafted
