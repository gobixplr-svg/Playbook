# Step 2: Technical Spikes

## Purpose

Validate "Complex" or "Unknown" technical risks with time-boxed prototypes before committing to architecture. A spike answers a specific question — **can we actually do this, and how?** — with working code, not speculation.

## Why This Matters

The Technical Feasibility assessment (Phase 0, Step 4) identified risks and complexity ratings. Items rated "Complex" or "Unknown" are too risky to design around without proof. A spike is cheaper than building the wrong architecture and discovering mid-production that a core feature doesn't work as assumed.

Spikes are **experiments, not prototypes.** The code is disposable. The knowledge is permanent.

## Prerequisites

- Step 1 (Requirements) completed — user stories identify which features have technical unknowns
- Phase 0 feasibility assessment flagged "Complex" or "Unknown" items

## Process

### 2.1 Identify Spike Candidates

Review three sources for spike candidates:

1. **Phase 0 Technical Feasibility** — Any feature rated "Complex" or "Unknown" in the complexity assessment. Any item flagged as "Spike Needed."
2. **Phase 0 Risk Register** — Risks with "High" impact that depend on unproven technical assumptions.
3. **Step 1 Requirements** — Acceptance criteria that depend on APIs, integrations, or techniques you haven't used before.

**Not everything needs a spike.** Only spike items where:
- You genuinely don't know if it's possible
- The approach has multiple viable options and you need data to choose
- Failure would force a significant architectural change
- The team has no prior experience with the technology

**When to skip spikes entirely:**

Sometimes every flagged item turns out to be a known-good pattern by the time you reach Pre-Production. If that's the case, skip the step — don't run spikes just to check a box. Process should accelerate delivery, not slow it down.

Skip spikes when:
- All flagged items use well-documented, mature tooling with production track records
- The "unknowns" have resolved into clear choices since the feasibility assessment (ecosystems evolve)
- Fallback options exist and are trivial to swap if the primary choice doesn't work
- A spike would essentially be building Sprint 1 features early with no additional learning

If skipping, document the decision and rationale in a brief spike report. Note which approach you're choosing and why, so the reasoning is captured for Architecture Design (Step 3). Then move on.

**Prioritizing spike candidates:**
- **Spike what blocks architecture decisions first** — if a spike outcome changes your data model or service design, run it before Step 3
- **Spike integration risks over implementation risks** — "Can we connect to this API?" matters more than "Can we build this feature?" (features can always be simplified; incompatible APIs cannot)
- **Skip obvious spikes** — if the team has production experience with the technology, a spike wastes time. Spike the unknowns, not the familiar.
- **Limit to 2–4 spikes** — more than 4 means your architecture has too many unknowns. Simplify the tech stack or revisit requirements.

### 2.2 Define Spike Parameters

For each spike, define these before writing any code:

| Parameter | Description |
| ----------- | ------------- |
| **Question** | The specific question this spike answers. One question per spike. |
| **Time Box** | Maximum time to spend. Typically 2-4 hours for a focused spike, 1-2 days for a complex one. |
| **Success Criteria** | What does "validated" look like? Be concrete: "Data loads in under 2 seconds" not "It works." |
| **Failure Criteria** | What would make you abandon this approach? Define the kill condition upfront. |
| **What It Validates** | Which requirements, architecture decisions, or risks does this spike inform? |
| **Approach** | Brief description of what you'll build. Keep it minimal — just enough to answer the question. |

### 2.3 Execute Spikes

**Rules of engagement:**

1. **Respect the time box.** When time is up, stop and document what you learned — even if incomplete. An inconclusive spike is still valuable information.
2. **Build the minimum.** No polish, no error handling, no tests, no architecture. Hard-code values. Skip edge cases. The only goal is answering the question.
3. **Isolate the spike.** Use a separate directory, branch, or throwaway project. Spike code does not become production code.
4. **Document as you go.** Take screenshots, record timings, note surprises. You'll forget the details later.
5. **One spike at a time.** Complete and document one before starting the next. Context-switching between spikes wastes time.

**Spike execution pattern:**
```
1. Set up the minimal environment needed
2. Build the smallest thing that tests the hypothesis
3. Measure against success criteria
4. If blocked, note the blocker and try one alternative approach
5. Document findings regardless of outcome
```

### 2.4 Document Results

Complete a [Spike Report](../templates/spike-report.md) for each spike. The report captures:

- **Verdict:** Validated / Partially Validated / Failed / Inconclusive
- **What worked:** Findings that confirm the approach
- **What didn't:** Surprises, blockers, or limitations discovered
- **Performance data:** Timings, sizes, costs — concrete numbers
- **Architectural recommendations:** How this should be built in production based on what you learned
- **Code artifacts:** Links to spike code (even if throwaway) for future reference
- **Impact on requirements:** Do any user stories or acceptance criteria need updating?

### 2.5 Update Plans

After all spikes are complete:

1. **Update requirements** — Revise acceptance criteria if spike findings change what's feasible
2. **Note architectural constraints** — Findings that must be respected in Step 3 (Architecture)
3. **Update the risk register** — Risks that were validated, mitigated, or newly discovered
4. **Kill any dead features** — If a spike fails and no alternative exists, move the feature to Won't Have

## Deliverable

Complete a [Spike Report](../templates/spike-report.md) for each spike and save them to your project's `docs/pre-production/spike-reports/`.

## Definition of Done

- [ ] All "Complex" and "Unknown" items from Phase 0 have been spiked
- [ ] Each spike has a completed Spike Report with clear verdict
- [ ] Architectural recommendations are documented for each validated spike
- [ ] Requirements updated based on spike findings (if needed)
- [ ] Risk register updated with new information
- [ ] No unresolved blockers remain for architecture design (Step 3)

## Tips

- **Don't gold-plate spikes.** The moment you start adding error handling to spike code, you've crossed the line from experiment to prototype. Stop and document.
- **Failed spikes are successes.** Discovering that an approach doesn't work before you've built architecture around it is the entire point. Document why it failed and what the alternative is.
- **Spike in the real environment.** If the question is "can Mapbox render a 2,000-mile trail smoothly on iOS?", don't test in a web browser. Use the actual SDK on the actual platform.
- **Time-box aggressively.** A spike that takes a week has become a project. If you can't answer the question in 1-2 days, the question is too broad — break it into smaller questions.
- **Save the code.** Even throwaway spike code is useful reference material. Put it in a `spikes/` directory and link from the report.
