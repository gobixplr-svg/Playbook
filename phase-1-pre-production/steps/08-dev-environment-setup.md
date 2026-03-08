# Step 8: Dev Environment Setup

## Purpose

Set up the development environment so Phase 2 starts with momentum, not configuration. By the end of this step, the project compiles, a test suite runs, external services respond, and an AI workspace loads your project context — so the first Sprint 1 task is building a feature, not debugging Xcode settings.

## Why This Matters

Setting up infrastructure last means your first Sprint 1 story carries hidden setup tax — you'll spend days on Xcode config, dependency resolution, CI debugging, and "why won't HealthKit authorize?" instead of building features. Every hour of setup friction in Sprint 1 is an hour stolen from the walking skeleton.

This step formalizes three things:
1. **Build environment** — the project compiles with the correct structure and dependencies
2. **Testing pipeline** — at least one test passes end-to-end (target → framework → assertion → green)
3. **AI workspace** — a new AI session can understand the project without manual briefing

## Prerequisites

- Step 1 (Requirements) completed — test plan scope and acceptance criteria count
- Step 3 (Architecture) completed — module structure, service protocols, DI pattern
- Step 5 (Design System) completed — design tokens for asset setup
- Step 6 (API & Data) completed — database schema, external service list
- Step 7 (Project Plan) completed — sprint plan, current sprint stories

## Process

### 8.1 Project Setup

Create the IDE project and configure it to match the architecture from Step 3.

**Important: Create the Xcode project manually, not via AI.** AI-generated Xcode project files frequently produce "grey folder" issues — folder references instead of groups — which prevents the AI from managing files correctly in later sprints. The correct workflow:

1. **You create the Xcode project** in Xcode with the correct template, bundle ID, deployment target, language version, and test targets
2. **Verify file-system-synchronized groups** — Xcode 15+ uses this by default for new projects. In the Project Navigator, right-click the top-level app folder: if you see "Convert to Group" it's already synced (correct). This means new folders and `.swift` files created on the filesystem automatically appear as blue groups in Xcode.
3. **AI builds out the folder structure and files** — with file-system-synchronized groups enabled, the AI can safely create directories and Swift files via the filesystem and they'll compile automatically

**Xcode project creation checklist:**

| Setting | Value |
|---------|-------|
| Template | App (iOS) |
| Product Name | [Your app name] |
| Team | Your Apple Developer account |
| Organization Identifier | [com.yourapp] |
| Interface | SwiftUI |
| Language | Swift |
| Storage | None (unless using SwiftData/Core Data) |
| Include Tests | Yes (both Unit and UI) |

**After project creation, configure:**
- Deployment target from architecture doc
- Swift language version from architecture doc
- Strict concurrency checking (if using Swift 6)
- Code signing

**Then the AI creates:**
- Folder hierarchy from the architecture document — every folder in the spec should exist, even if empty
- `.gitignore` for the platform
- Initial commit of the empty project structure

**Folder structure verification:**
Walk through the architecture document's module list and confirm each folder exists in the project as a blue group (not grey). Missing folders mean missing homes for Sprint 1 code — which leads to "where does this go?" decisions that slow development.

### 8.2 Dependency Management

Add all third-party dependencies identified across Steps 2–6.

**For each dependency:**
- Package manager (SPM preferred for iOS — it's built into Xcode and requires no extra tooling)
- Exact or tightly-pinned version (not "latest" — that's a build break waiting to happen)
- Which spec identified this dependency (architecture, API, spike)

**Dependency inventory format:**

| Dependency | Manager | Version | Purpose | Source Spec |
|-----------|---------|---------|---------|-------------|
| [name] | SPM | ~x.y | [what it does] | Step 3 / Step 6 |

Build the project after adding all dependencies. A clean build with all dependencies resolved is the baseline.

### 8.3 Testing Infrastructure

Set up the testing pipeline so TDD begins immediately in Sprint 1.

**Create test targets:**
- Unit test target (Swift Testing framework preferred for new code)
- UI test target (XCUITest — Swift Testing doesn't support UI tests yet)

**Create mock protocols:**
For every service protocol defined in Step 3, create a corresponding mock class. These are the test doubles that make your architecture testable:

```swift
class MockRouteService: RouteServiceProtocol {
    var routesToReturn: [Route] = []
    var fetchRoutesCalled = false
    func fetchRoutes() async throws -> [Route] {
        fetchRoutesCalled = true
        return routesToReturn
    }
}
```

**Write a "hello world" test:**
The most boring test imaginable — assert that true equals true. The value is in the infrastructure, not the assertion:
- The test target compiles
- It can import the main module
- Assertions execute
- CI can report results

### 8.4 External Service Configuration

Set up development instances for all external services from the architecture and API specs.

**For each service, document:**

| Service | Dev Instance | Key Location | Verification |
|---------|-------------|-------------|-------------|
| [name] | [URL/project] | [Info.plist / Keychain / server-only] | [test request] |

**Key placement rules:**
- **Public keys** (read-only, bundle-ID-restricted): Info.plist or build config
- **User tokens** (access/refresh, OAuth): Keychain — never UserDefaults
- **Service role keys** (admin access): Server-side only (Edge Functions, CI secrets) — never in the client bundle

Verify connectivity by running a simple request against each service. A database query, an auth check, a map tile load — anything that proves the connection works.

#### Infrastructure Pre-flight Checklist

Before committing to any hosted backend or third-party service, verify the following. Discovering cost or account issues mid-sprint is expensive — 5 minutes of pre-flight prevents a session-derailing pivot.

| Check | What to Verify |
|-------|---------------|
| **Account limits** | Free tier project count, bandwidth caps, storage limits. Will you hit a wall during development? |
| **Hosting costs** | Pricing at all tiers (free, dev, production). Can you justify the cost at each phase? |
| **Self-hosting viability** | Is there a Docker/self-hosted option as fallback? What's the setup cost? |
| **Required secrets** | List every API key, private key, and credential the service needs. Where will each one live? |
| **Service coupling** | Which app services depend on this backend? (e.g., RLS-protected routes require real auth) |
| **Local development** | Can you run the service locally without internet? Is there a CLI or emulator? |

**Why this exists:** In RoamAbout, the team committed to Supabase Cloud without checking account limits — the free tier was already exhausted. Mid-integration pivot to Docker self-hosting cost a session. The service coupling between auth and routes (RLS requires authenticated role) wasn't flagged until testing, causing another pivot.

### 8.5 CI/CD Pipeline

Configure automated builds so broken code is caught before it merges.

**Minimum CI pipeline:**
- Trigger on push to main/develop and on PRs
- Build the project
- Run all tests (unit + UI)
- Report status (pass/fail)

**Code quality:**
- Configure a linter (SwiftLint for iOS, ESLint for web, etc.)
- Match linter rules to coding standards from the AI workspace
- Zero violations on the empty project

Start minimal. A CI pipeline that builds and tests is 90% of the value. Deployment, code coverage, and artifact archiving can be added incrementally.

### 8.6 AI Workspace Setup

Configure the AI coding assistant to understand the project without manual briefing every session.

**Project instructions (`.claude/CLAUDE.md` or equivalent):**
- What the app is (one paragraph)
- Current phase and status
- Key decisions (tech stack, architecture, patterns)
- Project structure reference
- Core values for tradeoff decisions
- Pointers to pre-production specs

**Coding rules (`.claude/rules/` or equivalent):**
Create focused rules files (30–80 lines each) for:
- **Language standards** — naming, architecture patterns, error handling, testing framework
- **UI framework rules** — design system enforcement, animation standards, prohibited patterns
- **Security rules** — input validation, credential storage, data privacy, rate limiting
- **Git workflow** — commit format, branching strategy, PR conventions

Each rules file should be imperative ("Do X", "Never Y") — concise rules are followed; verbose guidelines are skipped.

**Memory seeding (`.claude/memory/MEMORY.md` or equivalent):**
Summarize key decisions from Steps 1–7 that would be expensive to re-derive:
- Tech stack and versions
- Architecture pattern and DI approach
- Data model summary (names, key relationships)
- Service protocol inventory
- Design tokens (colors, typography, spacing)
- Database structure (table names, RLS patterns)
- Sprint plan summary (current sprint, total sprints)
- Testing approach

MEMORY.md is the cheat sheet an AI reads at session start. Summarize and link — don't duplicate full specs.

**Session hooks (optional but valuable):**
- Session start: restore previous context (branch, recent files, active task)
- Session end: persist current state
- Pre-compact: snapshot before context compression
- Decision logger: persist architectural decisions

**Verification:** Start a fresh AI session. Without any manual context, the AI should answer: What is this project? What tech stack does it use? What architecture pattern? What am I working on right now?

### 8.7 Security Configuration

Create security rules and configure platform-specific security settings.

**Security rules file:**
Define rules for each category:
- Input validation (string lengths, format validation, HTML stripping)
- Authentication session management (token refresh, re-auth for destructive actions, session cleanup on sign-out)
- Credential storage (what goes in Keychain vs. Info.plist vs. server-only)
- Network security (HTTPS enforcement, logging restrictions)
- Data privacy (what health/personal data is stored, aggregation policies)
- Premium content gating (server-side enforcement via RLS, not client-side booleans)
- Rate limiting (per-endpoint limits, 429 response handling)
- Error message sanitization (generic user messages, private detailed logs)

**Platform-specific security:**
- iOS: PrivacyInfo.xcprivacy manifest, App Transport Security, privacy usage descriptions
- Backend: RLS policies from API spec ready to deploy, Edge Functions for server-side validation
- Review OWASP Mobile Top 10 for relevance to your app

### 8.8 Validation

Final verification that the environment is ready for Phase 2:

- [ ] Project builds successfully (empty app, correct folder structure)
- [ ] At least one unit test passes
- [ ] CI pipeline triggers on push and reports green
- [ ] Each external service responds to a test request
- [ ] Linter runs with zero violations
- [ ] AI workspace loads correctly in a new session
- [ ] All pre-production specs are accessible from the project
- [ ] Security configuration documented and verified

## Deliverable

Complete the [Dev Environment Setup Template](../templates/dev-environment-setup.md) and save it to your project's `docs/pre-production/dev-environment-setup.md`.

## Definition of Done

- [ ] Project compiles with the folder structure from the architecture document
- [ ] All dependencies from the architecture and API specs resolve and build
- [ ] Test targets exist with at least one passing unit test
- [ ] External services configured with development credentials and verified
- [ ] CI/CD pipeline runs on push (build + test)
- [ ] AI workspace configured (project instructions, coding rules, memory, hooks)
- [ ] Security rules documented and platform security configured
- [ ] Validation checklist passes (Section 8.8)

## Tips

- **Build the folder structure from the architecture doc, not from a tutorial.** Step 3 already defined where everything goes — don't reinvent it.
- **Pin dependency versions.** "Latest" today is "broken build in 3 months" when a breaking change ships.
- **One passing test proves the pipeline works.** Write the most boring test imaginable. The value is in the infrastructure, not the assertion.
- **Don't skip the AI workspace for "later."** The first Sprint 1 session without it costs more time than setting it up now.
- **Security rules prevent vulnerabilities at generation time.** It's cheaper to never write `UserDefaults.set(token)` than to find and fix it in code review.
- **Separate dev from production early.** Use a separate backend instance for development. Accidentally deleting production data during Sprint 1 is a bad day.
- **The validation checklist is your exit gate.** If any item fails, the environment isn't ready. Fix it before starting Sprint 1.
