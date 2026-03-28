# Content Authoring Brief — [App Name]

## Purpose

Plan and track content creation for apps where user-facing content is a production dependency. Without upfront content planning, content authoring becomes the bottleneck that blocks feature sprints.

## When to Use

Use this template when your app depends on authored content that must exist before features can be tested:

- **Question banks** — trivia, quizzes, educational content
- **Level designs** — puzzle levels, game worlds, progression stages
- **Route/location data** — curated places, trails, itineraries
- **Prompt libraries** — conversation starters, journal prompts, daily challenges
- **Seed content** — sample data that demonstrates the app's value on first launch

If your app generates content dynamically (user-generated, API-sourced, or procedural), this template is not needed.

---

## Content Inventory

| Content Type | Quantity Needed | Sprint Dependency | Priority | Status |
| ---- | ---- | ---- | ---- | ---- |
| | | Sprint N needs X items | Must Have / Should Have | Not Started / In Progress / Done |

---

## Content Structure

### [Content Type 1]

**Format:**

```
[Define the exact structure of each content item — fields, types, constraints]
```

**Example:**

```
[One complete example showing what a finished content item looks like]
```

**Quality Criteria:**

- [What makes a good item vs. a weak one]
- [Length, tone, difficulty, or other quality dimensions]
- [Accessibility or inclusivity requirements]

**Validation Rules:**

- [Format requirements — max length, required fields, etc.]
- [Content rules — no duplicates, difficulty distribution, topic coverage]

---

<!-- Copy the content type block above for each content type -->

## Production Timeline

| Milestone | Content Needed | Deadline | Notes |
| ---- | ---- | ---- | ---- |
| Sprint N kickoff | X items of [type] | YYYY-MM-DD | Minimum for feature testing |
| Beta release | X items of [type] | YYYY-MM-DD | Full content set for beta testers |
| Launch | X items of [type] | YYYY-MM-DD | Complete content library |

## Authoring Approach

**Who creates the content:**

- [ ] Manual authoring (developer/team)
- [ ] AI-assisted generation (generate → review → edit)
- [ ] Community/crowdsourced
- [ ] Licensed/purchased dataset
- [ ] Combination: ___

**AI-assisted workflow (if applicable):**

1. Generate batch of N items using [tool/prompt]
2. Review each item against quality criteria
3. Edit for tone, accuracy, and variety
4. Validate against format and content rules
5. Import into app data format

**Storage format:**

- [ ] JSON file(s) in app bundle
- [ ] Database seed data (SQL/migration)
- [ ] Remote content (API/CMS)
- [ ] Other: ___

---

## Content Tracking

### Batch 1 — [Date]

- **Items created:** N
- **Items reviewed:** N
- **Items approved:** N
- **Notes:**

---

<!-- Copy the batch block for each authoring session -->

## Risk: Content as Bottleneck

Content authoring is creative work that's harder to parallelize than code. Plan for these risks:

- **Start early** — begin authoring during Sprint 1 even if the feature that uses it is in Sprint 2+
- **Minimum viable content** — identify the smallest content set that makes the feature testable (often 5-10 items, not the full 200)
- **Placeholder content** — use placeholder/test content for development, swap real content before beta
- **Batch authoring** — content creation is faster in focused sessions than scattered across sprints
