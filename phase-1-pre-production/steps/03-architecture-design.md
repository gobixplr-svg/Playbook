# Step 3: Architecture Design

## Purpose

Define the system architecture, app structure, data models, and testability patterns that will support all v1 requirements. By the end of this step, any developer should be able to understand how the app is organized, how data flows, and how to add a new feature without guessing.

## Why This Matters

Architecture is the difference between a codebase that scales gracefully and one that becomes unmaintainable by Sprint 3. This step converts the "what" (requirements) and "how it could work" (spikes) into "how it will work" — with explicit decisions about patterns, boundaries, and testability.

Good architecture for a solo dev app is not the same as good architecture for a team project. Optimize for clarity, speed of iteration, and testability — not abstraction layers you'll never need.

## Prerequisites

- Step 1 (Requirements) completed — epics, user stories, and acceptance criteria defined
- Step 2 (Spikes) completed — technical unknowns resolved, architectural recommendations documented

## Process

### 3.1 System Architecture Overview

Draw the high-level picture: what are the major components and how do they connect?

**Define:**
- **Client app** — what framework, what architecture pattern (MVVM, MVC, etc.)
- **Backend services** — what provides data, auth, storage, push
- **External integrations** — health data sources, map SDKs, analytics
- **Data flow direction** — what talks to what, sync vs. async, real-time vs. batch

Keep it to a single diagram. If you need more than one diagram, you're overcomplicating it.

### 3.2 App Architecture Pattern

Choose and document the app's internal architecture:

**Decide:**
- **Pattern:** MVVM, MVC, TCA, Clean Architecture, etc.
- **Why this pattern:** Justify the choice for your team size and app complexity
- **Module/feature boundaries:** How is the codebase organized? By feature? By layer?
- **Navigation:** How does the app navigate between screens? (Router, NavigationStack, TabView)

**For each major feature/epic, document:**
- Which views, view models, and services are involved
- How data flows from the backend/health source → model → view model → view
- Where business logic lives (view model vs. service vs. server-side)

### 3.3 Data Models

Define the Swift/Kotlin models that represent your domain:

**For each model:**
- Properties with types
- Relationships to other models
- Where it's stored (local only, server only, both)
- Codable conformance if synced with a backend

**Consider:**
- Keep models simple — plain structs, not classes, unless you need reference semantics
- Separate API response models from domain models if the shapes differ
- Use value types by default

### 3.4 Service Layer

Define the services that provide data and capabilities to the app:

**For each service:**
- What it does (single responsibility)
- Its public interface (protocol)
- Dependencies it needs
- Whether it's a singleton, injected, or created per-use
- Error handling strategy

**Common services for a mobile app:**
- Auth service
- Data sync service
- Health data service
- Map/location service
- Notification service
- Analytics service (if applicable)

### 3.5 State Management

How does app-wide state flow?

**Define:**
- What state is global vs. local to a view
- How user session state is managed (logged in, subscription status)
- How data refreshes propagate to views
- Caching strategy (what's cached locally, for how long, when invalidated)

### 3.6 Testability Architecture

TDD requires testable architecture. Design for it explicitly:

**Protocol-oriented design:**
- Every service has a protocol defining its interface
- Production implementations conform to the protocol
- Test mocks conform to the same protocol
- Views depend on protocols, not concrete types

**Dependency injection:**
- How are dependencies provided? (initializer injection, environment, container)
- Where is the dependency graph configured? (app entry point, factory)

**Test boundaries:**
- What's testable in isolation? (business logic, view models, services)
- What requires integration tests? (API calls, database queries, HealthKit)
- What requires UI tests? (navigation, user flows, visual correctness)

### 3.7 Offline & Sync Strategy

How does the app work without a network connection?

**Define:**
- What data is available offline (cached routes, local progress)
- What requires network (social features, photo loading, subscription validation)
- How local changes sync when connectivity returns
- Conflict resolution strategy (last-write-wins, merge, etc.)

### 3.8 Security Architecture

How are sensitive operations and data protected?

**Define:**
- Authentication flow (sign-in, token refresh, session management)
- API key management (where keys are stored, how they're accessed)
- Health data privacy (what leaves the device, what stays local)
- Premium feature gating (server-side validation, not client-side hiding)

## Deliverable

Complete the [Architecture Document Template](../templates/architecture-document.md) and save it to your project's `docs/pre-production/architecture-document.md`.

## Definition of Done

- [ ] System architecture overview with component diagram
- [ ] App architecture pattern chosen and justified
- [ ] Module/feature structure defined
- [ ] Navigation architecture documented
- [ ] Data models defined for all domain entities
- [ ] Service layer designed with protocols
- [ ] State management strategy documented
- [ ] Testability architecture designed (DI, protocols, mock strategy)
- [ ] Offline/sync strategy defined
- [ ] Security architecture documented
- [ ] Architecture supports all Must Have requirements without modification
- [ ] Spike recommendations incorporated

## Tips

- **Don't over-architect for one.** If you're a solo dev, you don't need a dependency injection framework. Protocol conformance + initializer injection is enough.
- **Design for testability, not for abstraction.** A protocol for every service enables mocking. A protocol for every model is overkill.
- **Let the requirements drive the architecture.** Every architectural decision should trace back to a user story or acceptance criterion. If you can't explain why a component exists in terms of user value, you probably don't need it.
- **Spike recommendations are constraints.** If the spike said "use `lineTrimOffset` with `lineMetrics: true`," that's an architectural constraint, not a suggestion.
- **Name things by what they do, not what they are.** `RouteProgressTracker` is better than `RouteProgressManager`. `HealthDataSyncService` is better than `HealthService`.
