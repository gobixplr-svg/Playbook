# Step 4: Technical Feasibility

## Purpose

Validate that the app concept is technically buildable within your constraints (time, budget, skill, platform capabilities). This step identifies **blockers, risks, and key technical decisions** before you commit to building.

## Why This Matters

Technical feasibility isn't about designing the architecture (that's Pre-Production). It's about answering: **Can we actually build this?** You're looking for:
- Hard blockers (platform API doesn't support this)
- Skill gaps (we'd need to learn X)
- Cost surprises (this API costs $Y/month at scale)
- Complexity signals (this "simple" feature is actually a 3-month effort)

## Process

### 4.1 Platform API Capabilities

For each target platform, verify that the required APIs and capabilities exist.

**Investigate:**
- Does the platform provide the data/features you need?
- What are the API limitations (rate limits, data granularity, permissions)?
- What OS version is required for the APIs you need?
- Are there platform-specific review/compliance requirements?

**Document for each platform:**
| Capability Needed | API/Framework | Availability | Limitations | Notes |
|---|---|---|---|---|
| e.g., Step counting | HealthKit | iOS 8+ | Requires permission, background limits | |

### 4.2 Third-Party Integration Assessment

For each external service or API identified in Steps 2-3:

**Evaluate:**
- API documentation quality and stability
- Authentication requirements (OAuth, API keys)
- Rate limits and quotas
- Pricing (free tier, paid tiers, per-request costs)
- Data format and reliability
- Terms of service restrictions
- Community/SDK support for your platforms

**Categorize:**
- **Ready:** Well-documented, affordable, reliable — can proceed
- **Risky:** Unstable, expensive, or poorly documented — needs a fallback plan
- **Blocked:** TOS prohibits your use case, or API doesn't exist — need an alternative

### 4.3 Data & Content Feasibility

If the app depends on a dataset or content library:

**Assess:**
- Is the data publicly available or does it require licensing?
- What format is it in? How much cleanup/transformation is needed?
- How large is the dataset? Storage and delivery implications?
- How often does it need updating? Manual vs. automated pipeline?
- Are there legal/copyright considerations?

### 4.4 Infrastructure Cost Estimation

Rough-order cost estimation (not precise — that's Pre-Production):

**Consider:**
- Backend hosting (serverless vs. dedicated)
- Database (managed service pricing tiers)
- Storage (file/media storage and bandwidth)
- Third-party API costs at projected usage
- Authentication service costs
- CDN / content delivery
- Monitoring and error tracking

**Model at three scales:**
- **Prototype:** 10-100 users (should be near-free)
- **Launch:** 1,000-10,000 users
- **Growth:** 100,000+ users

### 4.5 Tech Stack Options

Don't finalize the stack — document the realistic options with tradeoffs.

For each viable approach:
| Factor | Option A | Option B | Option C |
|--------|----------|----------|----------|
| Platforms covered | | | |
| Code sharing | | | |
| Native API access | | | |
| Team familiarity | | | |
| Community/ecosystem | | | |
| Performance | | | |
| Time to MVP | | | |

### 4.6 Risk Register

Compile all identified risks:

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| e.g., HealthKit permission denied | Medium | High — core feature broken | Graceful fallback to manual entry |

### 4.7 Complexity Assessment

For each major feature, give a rough complexity signal:

- **Simple:** Straightforward implementation, well-documented patterns exist
- **Moderate:** Requires research or learning, but achievable
- **Complex:** Significant engineering challenge, no clear path yet
- **Unknown:** Can't assess without a prototype or spike

Flag anything rated "Complex" or "Unknown" — these need spikes in Pre-Production before committing to them.

## Deliverable

Complete the [Technical Feasibility Template](../templates/technical-feasibility.md) and save it to your project's `docs/concept/technical-feasibility.md`.

## Definition of Done

- [ ] All required platform APIs are verified as available
- [ ] Third-party integrations are evaluated (ready/risky/blocked)
- [ ] Data and content sources are assessed for availability and licensing
- [ ] Infrastructure costs are estimated at three scales
- [ ] Tech stack options are documented with tradeoffs (not finalized)
- [ ] Risk register is compiled
- [ ] Feature complexity is assessed with no unacknowledged "unknowns"
- [ ] No hard blockers remain, or blockers have documented workarounds
- [ ] "Needs Research" items from Step 2 are resolved or updated

## Tips

- **Prototype the risky parts.** If a critical feature depends on an API you haven't used, build a 30-minute proof of concept. Don't commit to the full project based on assumption.
- **Talk to the platform.** Apple and Google publish detailed API documentation. Read it, don't guess.
- **Cost surprises kill projects.** A "free" API at prototype scale can cost thousands at growth scale. Always check pricing tiers.
- **Your skillset is a real constraint.** Be honest about learning curves. "I've never used WebSockets" is a valid complexity factor.
