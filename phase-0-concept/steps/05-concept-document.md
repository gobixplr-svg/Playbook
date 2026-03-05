# Step 5: Concept Document

## Purpose

Synthesize everything from Steps 1-4 into a single, authoritative document that captures the full concept and supports a Go/No-Go decision. This is the "pitch deck as a document" — it should be compelling, honest, and complete.

## Why This Matters

The Concept Document is the contract between the idea phase and the building phase. It forces you to confront whether the idea holds up when all the pieces are laid out together. It's also the reference document that keeps the project aligned as you move into Pre-Production and beyond.

## Process

### 5.1 Compile the Concept Document

Use the [Concept Document Template](../templates/concept-document.md). The document should flow as a narrative, not just a collection of copy-pasted sections.

#### Document Sections

**Executive Summary**
- 2-3 paragraph overview that someone could read in 2 minutes and understand the entire concept
- Include: what it is, who it's for, why now, how it makes money

**Vision & Problem**
- Pull from Step 1: Vision Statement
- Refined problem statement
- Target audience with specifics

**Market Landscape**
- Pull from Step 3: Competitive Analysis
- Key competitors and their positioning
- Your differentiation and positioning statement
- Market size signals (if available)

**Product Overview**
- Core feature set for v1 (MoSCoW prioritized)
- User journey / key flows (narrative, not wireframes)
- Platform strategy (which platforms, in what order)
- Gamification and reward structure
- Social features scope

**Technical Summary**
- Pull from Step 4: Technical Feasibility
- Recommended tech stack direction (with rationale)
- Key integrations and data sources
- Infrastructure approach
- Known risks and mitigations

**Monetization Strategy**
- Revenue model
- Pricing rationale (grounded in competitive analysis)
- Free vs. paid feature split
- Revenue projections (rough — order of magnitude, not precise)

**Feature Prioritization (MoSCoW)**
- **Must Have:** Core features required for v1 launch
- **Should Have:** Important but not launch-blocking
- **Could Have:** Nice additions if time/resources allow
- **Won't Have (Yet):** Explicitly out of scope for v1

**Risk Register**
- Technical risks (from Step 4)
- Market risks (from Step 3)
- Resource/timeline risks
- Each with likelihood, impact, and mitigation

**Go/No-Go Criteria**
- List the specific criteria that determine whether to proceed
- Be honest — if there's a dealbreaker, name it

### 5.2 Review & Validate

Before marking Phase 0 as complete:

**Self-review:**
- Read the document as if you're seeing the idea for the first time. Is it convincing?
- Are there any "trust me, this will work" moments that lack evidence?
- Does the monetization model make sense given the competitive landscape?
- Are you excited about building this? (Motivation matters for solo devs.)

**External review (if possible):**
- Share with a trusted peer, mentor, or potential user
- Ask: "Would you use this? Would you pay for this?"
- Listen for confusion — if they don't get it, the concept needs refinement

### 5.3 Make the Go/No-Go Decision

Based on the completed document:

**Go** — The concept is clear, feasible, differentiated, and worth investing in. Proceed to Phase 1: Pre-Production.

**Conditional Go** — The concept is promising but has specific conditions that must be met (e.g., "go if the trail data API proves viable during a Pre-Production spike"). Document the conditions.

**No-Go** — The concept has a fatal flaw (no market, technically infeasible, unsustainable economics). Document why and shelve the idea. This is a success — you saved months of building the wrong thing.

**Pivot** — The analysis revealed a better version of the idea. Return to Step 1 with the new direction.

## Deliverable

Complete the [Concept Document Template](../templates/concept-document.md) and save it to your project's `docs/concept/concept-document.md`.

## Definition of Done

- [ ] Concept Document is complete with all sections
- [ ] Vision, competitive analysis, feasibility, and monetization are synthesized (not just appended)
- [ ] Feature prioritization uses MoSCoW framework
- [ ] Risk register is complete with mitigations
- [ ] Go/No-Go criteria are defined
- [ ] Go/No-Go decision is made and documented
- [ ] If "Go" — the document is ready to serve as the reference for Pre-Production

## Tips

- **Write it to convince a skeptic.** Even if you're the only reader, write as if you're presenting to an investor or co-founder. This forces rigor.
- **Kill your darlings.** If a feature doesn't survive scrutiny, move it to "Won't Have (Yet)" without guilt.
- **The document is alive.** Update it during Pre-Production as you learn more. It's a reference, not a contract carved in stone.
- **A "No-Go" is a win.** Seriously. You just saved yourself months of effort on something that wouldn't have worked. Celebrate clarity.
