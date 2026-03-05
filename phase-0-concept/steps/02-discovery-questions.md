# Step 2: Discovery Questions

## Purpose

Systematically surface the unknowns, assumptions, and scope decisions that will shape the project. This is not a brainstorm — it's a structured interrogation of the idea to identify what you know, what you don't, and what you need to decide.

## Why This Matters

Every project has implicit assumptions baked into the initial idea. This step forces those assumptions into the open where they can be validated, challenged, or deferred intentionally. Skipping this leads to mid-development surprises that are expensive to fix.

## Process

### 2.1 Run the Discovery Questionnaire

Work through each category in the [Discovery Questionnaire Template](../templates/discovery-questionnaire.md). For every question:

- **Answer it** if you know
- **Flag it** if you need research (these feed into Step 3: Competitive Analysis and Step 4: Technical Feasibility)
- **Defer it** with a rationale if it's not relevant yet (e.g., "monetization TBD until after competitive analysis")

### 2.2 Questionnaire Categories

The questionnaire covers these domains. Each is explained below with the *types* of questions to ask — the template has the specific questions.

#### Platform & Tech Stack
What platforms are you targeting and how will you build for them?

Key tensions:
- Native (best UX, most effort) vs. Cross-platform (shared code, some compromise)
- Platform-specific features (HealthKit, widgets) vs. feature parity
- Team skillset vs. ideal technology

*Don't finalize the tech stack here.* Document the options and tradeoffs. The decision comes in Pre-Production.

#### Data Sources & Integrations
What data does the app need, and where does it come from?

Consider:
- First-party data (user input, device sensors)
- Platform health APIs (HealthKit, Health Connect)
- Third-party APIs (Strava, Garmin, Fitbit)
- Public datasets (geographic data, trail databases)
- Content you need to create or license

#### Backend & Infrastructure
What server-side capabilities does the app need?

Consider:
- User accounts and authentication
- Cloud data sync across devices
- API layer between client and database
- Real-time features (leaderboards, social feeds)
- File/media storage
- Cost at scale

#### Social & Community
What role do other people play in the experience?

Consider:
- Solo experience vs. social features
- Privacy implications of social features
- Moderation requirements for user-generated content
- Community building vs. competitive features
- Sharing and virality mechanics

#### Content & Data Pipeline
What content does the app need, and how does it get created and maintained?

Consider:
- Static content (pre-loaded at build time)
- Dynamic content (fetched from server)
- User-generated content
- Content update frequency
- Content creation tooling needs

#### Gamification & Rewards
How does the app motivate continued engagement?

Consider:
- Achievement systems (badges, milestones)
- Progress visualization
- Unlockable content
- Physical rewards (cost, fulfillment, partnerships)
- Retention vs. manipulation — where's your ethical line?

#### Monetization
How does the app make money (or not)?

Consider:
- Free / freemium / premium / subscription
- What's free vs. paid? Where's the paywall?
- Ad-supported considerations
- In-app purchases
- Price sensitivity of target audience
- Revenue needed to sustain development

### 2.3 Categorize Outcomes

After completing the questionnaire, sort every answer into:

| Category | Meaning | Next Action |
|----------|---------|-------------|
| **Decided** | Clear answer, ready to act on | Document in Concept Doc |
| **Needs Research** | Don't know yet, need data | Feed into Step 3 (Competitive) or Step 4 (Technical) |
| **Deferred** | Not relevant for v1, revisit later | Document rationale for deferral |
| **Assumption** | Treating as true, but unvalidated | Flag for validation during Pre-Production |

## Deliverable

Complete the [Discovery Questionnaire Template](../templates/discovery-questionnaire.md) and save it to your project's `docs/concept/discovery-questionnaire.md`.

## Definition of Done

- [ ] All questionnaire categories are addressed
- [ ] Every question has an answer, a research flag, or a deferral with rationale
- [ ] Research items are identified for Step 3 (Competitive Analysis) and Step 4 (Technical Feasibility)
- [ ] Assumptions are explicitly listed
- [ ] No critical unknowns remain unacknowledged

## Tips

- **Don't rush past "I don't know."** The whole point is to find the gaps. An honest "needs research" is more valuable than a guessed answer.
- **Include stakeholders.** If you have a co-founder, designer, or advisor — run the questionnaire with them. Different perspectives surface different unknowns.
- **Revisit after later steps.** Competitive analysis and technical feasibility will answer many of the "needs research" items. Circle back and update.
