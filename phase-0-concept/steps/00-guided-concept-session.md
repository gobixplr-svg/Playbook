# Step 0: Guided Concept Session

## Purpose

Run a structured, conversational brainstorming session that walks through the entire Concept Phase interactively. Instead of filling out templates in isolation, this process uses iterative Q&A to surface details, challenge assumptions, and draft deliverables in real time.

This is the **entry point** for Phase 0. It drives Steps 1-5 through conversation rather than replacing them.

## When to Use

- Starting a new app concept from scratch
- Refining a rough idea into a structured concept
- Working with Claude Code or a collaborator to develop a spec
- Trigger phrase: "Let's brainstorm a concept" (or similar)

## How It Works

The session is a guided conversation organized into **sections** that map to Playbook Steps 1-4. Each section follows the same pattern:

```
Section Start
  → Ask one question at a time
  → Each question builds on previous answers
  → Branch into follow-ups when answers warrant depth
  → Summarize what's been captured
  → Draft the section's deliverable
  → Checkpoint: Confirm accuracy, surface anything missed
Section End → Transition to next section
```

### Key Rules

1. **One question at a time.** Never present a wall of questions. Let each answer breathe and inform the next question.
2. **Build on previous answers.** Don't ask generic questions when you already have context. Reference what was said.
3. **Branch, don't follow a script.** If an answer opens a new thread (e.g., "we want social features" → dig into what kind), follow it before moving on.
4. **Draft deliverables live.** After each section, draft the corresponding template. The user validates incrementally, not at the end.
5. **Checkpoint between sections.** Pause, summarize, confirm. Don't barrel through.
6. **Flag research items.** When a question can't be answered in conversation (e.g., "what APIs exist for trail data?"), flag it for Step 3 or Step 4 rather than guessing.

## Session Flow

### Section 1: Ideation & Vision
**Maps to:** [Step 1: Ideation & Vision](01-ideation-and-vision.md)
**Goal:** Capture the core idea, audience, and vision
**Typical questions:** 5-8
**Deliverable drafted:** Vision Statement

**Question Sequence (adaptive — not all questions are asked every time):**

1. **The spark.** "What's the core idea? Describe it however it comes to mind — rough is fine."
2. **The user.** "Who's the primary person using this? Paint a picture of them."
3. **The problem.** "What problem does this solve for them? Or what desire does it fulfill?"
4. **The current state.** "What do people do today instead? What's the workaround?"
5. **The differentiator.** "What makes this different from what exists? Why would someone switch?"
6. **The core loop.** "What does the user do most often in the app? What's the daily/weekly interaction?"
7. **The vision.** "Where does this go long-term? If it's wildly successful in 3 years, what does it look like?"
8. **The principles.** "When you have to make a tradeoff, what wins? (e.g., simplicity vs. features, fun vs. accuracy)"

**Checkpoint:** Draft the Vision Statement. Review with user. Refine.

---

### Section 2: Discovery & Scope
**Maps to:** [Step 2: Discovery Questions](02-discovery-questions.md)
**Goal:** Surface unknowns, scope decisions, and technical requirements
**Typical questions:** 15-25 (highly adaptive based on concept complexity)
**Deliverable drafted:** Discovery Questionnaire (completed)

**Question Categories (asked in this order, branching within each):**

1. **Platform & reach.** Which platforms, what order, native vs. cross-platform?
2. **Data & integrations.** What data does the app need? Where does it come from?
3. **Backend & infrastructure.** User accounts? Cloud sync? Real-time features?
4. **Social & community.** Solo vs. social? What social features? Privacy implications?
5. **Content & data pipeline.** What content ships at launch? How is it maintained?
6. **Gamification & rewards.** Achievement types? Unlock mechanics? Physical rewards?
7. **Monetization.** Revenue model direction? (Final answer comes from competitive analysis)

**Branching logic examples:**
- "Are you targeting multiple platforms?" → Yes → "Simultaneous launch or staggered?" → "Native per platform or cross-platform framework?"
- "Do you want social features?" → Yes → "Competitive, cooperative, or community-based?" → "Does this require content moderation?"
- "Physical rewards?" → Interested → "Fulfillment model? Partnerships? Cost implications?"

**Checkpoint:** Present completed Discovery Questionnaire. Categorize items as Decided / Needs Research / Deferred / Assumption. Confirm.

---

### Section 3: Market & Competition
**Maps to:** [Step 3: Competitive Analysis](03-competitive-analysis.md)
**Goal:** Understand the competitive landscape and position the app
**Typical questions:** 5-10 (supplemented by research)
**Deliverable drafted:** Competitor Matrix (initial)

**Process:**

1. **Ask what the user already knows.** "Are you aware of any apps that do something similar? What do you think of them?"
2. **Research.** Search app stores, web, and product directories for competitors. This is a research-heavy step — questions and research alternate.
3. **Review findings together.** "Here's what I found. Do any of these surprise you? Any I missed?"
4. **Pricing and monetization.** "Based on how competitors price, what feels right for your app?"
5. **Positioning.** "Given these competitors, what's your one-sentence differentiator?"
6. **Data sources.** "These are the data sources and APIs competitors use. Are there others we should investigate?"

**Checkpoint:** Draft the Competitor Matrix. Review positioning statement. Confirm.

---

### Section 4: Technical Feasibility
**Maps to:** [Step 4: Technical Feasibility](04-technical-feasibility.md)
**Goal:** Validate buildability, identify risks, estimate costs
**Typical questions:** 5-10 (supplemented by research)
**Deliverable drafted:** Technical Feasibility Assessment

**Process:**

1. **Resolve research items.** Address "Needs Research" flags from Section 2 that are technical in nature.
2. **API verification.** Research platform APIs and third-party services. Confirm availability and limitations.
3. **Ask about constraints.** "What's your team's tech stack experience? Any strong preferences? Budget constraints?"
4. **Cost modeling.** Estimate infrastructure costs at prototype / launch / growth scale.
5. **Risk identification.** "Based on everything we've discussed, here are the technical risks I see. Anything else concerns you?"
6. **Complexity assessment.** Flag features that need prototyping spikes before committing.

**Checkpoint:** Draft the Feasibility Assessment. Review risks and verdict. Confirm.

---

### Section 5: Synthesis
**Maps to:** [Step 5: Concept Document](05-concept-document.md)
**Goal:** Compile everything into the Concept Document and make a Go/No-Go decision
**Typical questions:** 2-5

**Process:**

1. **Draft the Concept Document** by synthesizing the four deliverables from Sections 1-4.
2. **Feature prioritization.** "Let's sort features into Must Have / Should Have / Could Have / Won't Have Yet."
3. **Review the risk register.** "These are all the risks we've identified. Any we should elevate?"
4. **Go/No-Go.** "Based on everything — does this hold up? Are we building this?"
5. **Next steps.** If Go → define what happens first in Pre-Production.

**Checkpoint:** Final Concept Document. Go/No-Go decision documented.

---

## Session Tips

### For the facilitator (Claude / collaborator)
- **Listen more than you talk.** The user's instincts about their idea are valuable. Draw them out, don't overwrite them.
- **Challenge gently.** If something seems risky or unclear, ask "how would that work?" rather than "that won't work."
- **Don't force completeness.** Some questions genuinely can't be answered yet. "Deferred" is a valid answer. The goal is conscious decisions, not complete answers.
- **Summarize often.** After every 3-5 questions, play back what you've heard. Misunderstandings compound.

### For the idea holder
- **Think out loud.** Half-formed thoughts are fine. The process will refine them.
- **Say "I don't know."** That's the most useful answer for questions you haven't considered. It flags what needs research.
- **Push back on questions.** If a question doesn't feel relevant, say so. Not every concept needs every question.
- **Stay in concept mode.** Resist the urge to jump into "how do we build it" during Sections 1-2. That comes in Sections 3-4.

## Integration with Playbook

This session produces draft versions of all Phase 0 deliverables:
- Vision Statement → `docs/concept/vision-statement.md`
- Discovery Questionnaire → `docs/concept/discovery-questionnaire.md`
- Competitor Matrix → `docs/concept/competitor-matrix.md`
- Technical Feasibility → `docs/concept/technical-feasibility.md`
- Concept Document → `docs/concept/concept-document.md`

After the session, each deliverable may need refinement — especially the Competitor Matrix and Technical Feasibility, which benefit from deeper research than a single conversation can provide. The session gets you 70-80% there; the individual step guides cover the remaining polish.

## Adapting This Process

**Quick concept (1-2 hours):** Run Sections 1-2 only. Produce Vision + Discovery. Defer competitive and technical research.

**Full concept (multi-session):** Run all 5 sections across 2-3 sessions. Allow time between sessions for research.

**Team workshop:** Assign one facilitator. Have the team answer collectively. Use checkpoints for alignment votes.
