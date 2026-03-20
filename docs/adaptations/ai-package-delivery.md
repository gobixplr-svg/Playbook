# Adaptation: AI Development Package as a Consulting Deliverable

How to package Playbook-derived tools, standards, and process into a professional deliverable for client teams adopting AI-assisted development.

---

## When to Use This

When a consulting engagement includes "help us work better with AI" alongside the hands-on build work. The code you write is one project; the system you leave behind serves every future project.

## What Goes in the Package

### Core (always include)

**Global CLAUDE.md**
The team's coding standards, security rules, and conventions — read automatically by Claude Code on every project. This is the highest-leverage piece. Without it, the AI guesses. With it, the AI follows their standards.

Customize for:
- Their framework (Bootstrap, Tailwind, Foundation, etc.)
- Their CMS (WordPress, Shopify, static, etc.)
- Their JS approach (vanilla, jQuery, React, etc.)
- Their deployment pipeline
- Their security requirements

**Per-Project CLAUDE.md Template**
A fill-in-the-blanks template that captures client-specific details: brand colors, fonts, CSS prefix, architecture decisions. Created at the start of each engagement.

**Skills (Custom Commands)**
Pre-built slash commands for repeatable tasks. Prioritize:
1. `/new-project` — Scaffolding (they'll use this on every engagement)
2. `/component` — Section building (the biggest time saver)
3. `/preflight` — Pre-launch checklist (catches embarrassing bugs)
4. Any skill specific to their workflow (e.g., `/wp-template` for WordPress shops)

### Process (include when the team wants structure)

**Adapted Playbook**
The Development Playbook compressed for their project type. See `docs/adaptations/agency-consulting.md` for the agency-specific 4-phase variant.

**Playbook State Template**
Progress tracking file that lives in each project repo. Ensures continuity across sessions and developers.

### Guides (include when the team is new to AI tools)

**Getting Started**
Zero to productive. Installation, first commands, basic workflow.

**AI Workflow Best Practices**
How to prompt effectively, give feedback, and avoid common mistakes.

**Maintaining the System**
How to create new skills, update CLAUDE.md, and improve the process over time. This is the guide that makes the package self-sustaining.

### Optional (include based on team needs)

**Figma MCP Setup**
If the team is open to connecting their design tool to AI. Only include if there's a realistic path to adoption.

**Starter Boilerplate**
A project skeleton with their framework pre-wired. Include if they start projects from scratch regularly.

---

## Assessment Process

Before building the package, understand the team's current state:

### Technical Assessment
1. **What's their stack?** — Framework, CMS, JS approach, deployment
2. **What version?** — Bootstrap 4 vs 5, WordPress theme base, jQuery dependency
3. **What do they NOT use?** — Features in the framework they skip (e.g., "we use Bootstrap CSS but not Bootstrap JS")
4. **What tools do they want to keep vs. change?** — Don't force migrations; recommend and wait

### Process Assessment
1. **How do they start projects today?** — From scratch? Copy a previous project? Template?
2. **How do designs get to developers?** — Figma, XD, Sketch, PDF, napkin drawings?
3. **What's their QA process?** — Formal, informal, "ship and pray"?
4. **What causes rework?** — Client feedback cycles, missed specs, cross-browser issues?

### Team Assessment
1. **How experienced are they with AI tools?** — Brand new, dabbling, daily users?
2. **Who decides on tool changes?** — Can the dev lead adopt Figma, or does the design team need to buy in?
3. **How many people will use the package?** — Solo developer or a team that needs consistency?

---

## The "Recommend But Don't Force" Principle

When you identify a better tool or workflow (e.g., Figma over XD), frame it as:
- **Here's what I recommend and why**
- **Here's the setup guide, ready when you are**
- **The package works with your current tools today**

Don't make the package dependent on a change the team hasn't agreed to. Build for their current reality and include the upgrade path.

**Example (agency engagement, March 2026):** The dev lead confirmed Bootstrap 5 immediately. The Figma recommendation went to the design team as a separate conversation. The package shipped with XD workflow support and Figma MCP setup ready to activate.

---

## Delivery Format

### File Structure
```
client-ai-package/
├── OVERVIEW.md              # Non-technical summary (for stakeholders)
├── README.md                # Technical quick start (for developers)
├── CLAUDE.md                # Global coding standards
├── skills/                  # Custom Claude Code commands
├── playbook/                # Adapted project process
├── guides/                  # Learning materials
├── templates/               # Reusable project documents
└── starter/                 # Framework boilerplate
```

### Two Entry Points
- **OVERVIEW.md** for non-technical stakeholders — what the package is, why it matters, business impact
- **README.md** for developers — installation, quick start, file reference

### Reading Order Table
Include a table showing who reads what first:
| Role | Start Here |
|---|---|
| Non-technical | OVERVIEW.md |
| Developer | README.md → guides/getting-started.md |
| Full team | guides/ai-workflow.md → guides/maintaining-the-system.md |

---

## Pricing the Package

The package has value independent of the build work. Consider:

**Bundled:** Include as part of a larger engagement (build + package). The build demonstrates the tools; the package is the long-term value.

**Standalone:** Deliver as a separate consulting product. Assessment + customized package + walkthrough session.

**Ongoing:** Package + monthly check-in to review what the team has changed, answer questions, and update the global CLAUDE.md based on their learnings.

---

## Success Metrics

The package is working when:
- The team uses `/new-project` on their next engagement without being told to
- Someone creates a new skill without your involvement
- The CLAUDE.md gets updated after a project decision
- The playbook state file shows progression (not abandoned after Phase 0)
- Revision rounds with clients decrease over time
