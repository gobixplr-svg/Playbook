# Dev Environment Setup — [App Name]

> Playbook Reference: [Step 8: Dev Environment Setup](../steps/08-dev-environment-setup.md)

## Project

**App Name:** [Name]
**Date:** [YYYY-MM-DD]
**Author:** [Name]
**Deployment Target:** [e.g., iOS 17+]
**Language Version:** [e.g., Swift 6]
**Package Manager:** [e.g., SPM]

---

## 1. Project Setup

- [ ] Project created with correct bundle ID, deployment target, and language version
- [ ] Folder structure matches architecture document
- [ ] Build settings configured (signing, Swift version, deployment target)
- [ ] `.gitignore` created for the platform
- [ ] Initial commit of empty project structure
- [ ] Project builds successfully (empty app)

**Folder structure:**

```
[AppName]/
├── Models/
├── ViewModels/
├── Views/
├── Services/
└── Resources/
```

_Update to match your architecture document's module list._

---

## 2. Dependencies

| Dependency | Manager | Version | Purpose | Source Spec |
|-----------|---------|---------|---------|-------------|
| | | | | |

- [ ] All dependencies added to project
- [ ] Project builds with all dependencies resolved
- [ ] No version conflicts or warnings

---

## 3. Testing Infrastructure

### Test Targets

| Target | Framework | Purpose |
|--------|-----------|---------|
| [App]Tests | [Swift Testing / XCTest] | Unit tests, service tests |
| [App]UITests | XCUITest | UI flow tests |

### Mock Protocols

_One mock class per service protocol from Step 3:_

| Mock Class | Protocol | Ready |
|-----------|----------|-------|
| | | [ ] |

### Hello World Test

- [ ] Unit test target compiles
- [ ] Can import main module (`@testable import [App]`)
- [ ] At least one assertion executes and passes
- [ ] Test runner reports green

---

## 4. External Services

| Service | Dev Instance | Key Location | Verified |
|---------|-------------|-------------|----------|
| | | Info.plist / Keychain / Server-only | [ ] |

**Key placement rules:**
- Public keys (read-only, bundle-ID-restricted) → Info.plist or build config
- User tokens (access/refresh, OAuth) → Keychain only
- Service role keys (admin access) → Server-side only — never in client bundle

- [ ] Each service responds to a test request
- [ ] No keys committed to git that shouldn't be

---

## 5. CI/CD Pipeline

**Platform:** [e.g., GitHub Actions, Xcode Cloud]

**Pipeline triggers:**
- [ ] Triggers on push to main/develop
- [ ] Triggers on pull requests
- [ ] Builds the project
- [ ] Runs all tests (unit + UI)
- [ ] Reports pass/fail status

**Code quality:**
- [ ] Linter configured ([e.g., SwiftLint])
- [ ] Linter rules match coding standards
- [ ] Zero violations on empty project

---

## 6. AI Workspace

### Project Instructions

| File | Purpose | Created |
|------|---------|---------|
| `.claude/CLAUDE.md` | Project overview, key decisions, structure, core values | [ ] |

**CLAUDE.md should include:**
- [ ] What the app is (one paragraph)
- [ ] Current phase and status
- [ ] Key decisions (tech stack, architecture, patterns)
- [ ] Project structure reference
- [ ] Core values for tradeoff decisions
- [ ] Pointers to pre-production specs

### Coding Rules

| Rules File | Scope | Lines | Created |
|-----------|-------|-------|---------|
| `swift-coding.md` | Language standards, architecture, testing | ~50 | [ ] |
| `swiftui-design.md` | UI framework, design system enforcement | ~60 | [ ] |
| `security.md` | Input validation, credentials, privacy, OWASP | ~65 | [ ] |
| `git-workflow.md` | Commit format, branches, PR conventions | ~75 | [ ] |

_Adjust file names and scopes for your platform and tools. Each file should be 30–80 lines of imperative rules._

### Memory Seed

| File | Purpose | Created |
|------|---------|---------|
| `.claude/memory/MEMORY.md` | Key decisions from Steps 1–7 | [ ] |

**MEMORY.md should summarize:**
- [ ] Tech stack and versions
- [ ] Architecture pattern and DI approach
- [ ] Data models (names and key relationships)
- [ ] Service protocols (names and responsibilities)
- [ ] Design tokens (colors, typography, spacing)
- [ ] Database structure (tables, RLS patterns)
- [ ] Sprint plan (current sprint, total sprints)
- [ ] Testing approach

### Session Hooks (Optional)

| Hook | Trigger | Purpose | Created |
|------|---------|---------|---------|
| Session start | New session opens | Restore context (branch, active task) | [ ] |
| Session end | Session closes | Persist current state | [ ] |
| Pre-compact | Context compression | Snapshot before truncation | [ ] |
| Decision logger | Key decisions made | Persist to memory | [ ] |

### AI Workspace Verification

- [ ] Start a fresh AI session (no manual context)
- [ ] AI can answer: What is this project?
- [ ] AI can answer: What tech stack does it use?
- [ ] AI can answer: What architecture pattern?
- [ ] AI can answer: What am I working on right now?

---

## 7. Security Configuration

### Security Rules Coverage

- [ ] Input validation (string lengths, format, HTML stripping)
- [ ] Authentication session management (token refresh, re-auth, cleanup)
- [ ] Credential storage (Keychain vs. Info.plist vs. server-only)
- [ ] Network security (HTTPS enforcement, logging restrictions)
- [ ] Data privacy (health/personal data policies, aggregation)
- [ ] Premium content gating (server-side enforcement, not client booleans)
- [ ] Rate limiting (per-endpoint limits, 429 handling)
- [ ] Error message sanitization (generic user messages, private logs)

### Platform Security

- [ ] Privacy manifest created (PrivacyInfo.xcprivacy or equivalent)
- [ ] Privacy usage descriptions added (camera, location, health, etc.)
- [ ] App Transport Security enabled (no HTTP exceptions)
- [ ] RLS policies from API spec ready to deploy
- [ ] OWASP Mobile Top 10 reviewed for relevance

---

## 8. Validation Checklist

_All items must pass before starting Phase 2:_

- [ ] Project builds successfully (empty app, correct folder structure)
- [ ] At least one unit test passes
- [ ] CI pipeline triggers on push and reports green
- [ ] Each external service responds to a test request
- [ ] Linter runs with zero violations
- [ ] AI workspace loads correctly in a new session
- [ ] All pre-production specs are accessible from the project
- [ ] Security configuration documented and verified

---

## Status

- [ ] Project setup complete (8.1)
- [ ] Dependencies resolved (8.2)
- [ ] Testing infrastructure ready (8.3)
- [ ] External services verified (8.4)
- [ ] CI/CD pipeline running (8.5)
- [ ] AI workspace configured (8.6)
- [ ] Security rules established (8.7)
- [ ] Validation checklist passed (8.8)
