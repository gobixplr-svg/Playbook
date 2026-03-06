# Architecture Document

> Playbook Reference: [Step 3: Architecture Design](../steps/03-architecture-design.md)

## Project

**App Name:** _[Name]_
**Date:** _[YYYY-MM-DD]_
**Author:** _[Name]_
**Architecture Pattern:** _[MVVM / MVC / TCA / etc.]_
**Target Platform:** _[iOS / Android / Web / etc.]_

---

## System Architecture

_High-level diagram showing how major components connect. Include: client app, backend, external integrations, and data flow direction._

### System Architecture Diagram

```
┌─────────────────────────────────────────────┐
│  Client App                                 │
│  ┌──────────┐  ┌──────────┐  ┌───────────┐ │
│  │  Views    │→ │ ViewModels│→ │ Services  │ │
│  └──────────┘  └──────────┘  └─────┬─────┘ │
└────────────────────────────────────┼────────┘
                                     │ Network
─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┼ ─ ─ ─ ─
                                     │
┌────────────────────────────────────┼────────┐
│  Backend                           │        │
│  ┌──────────┐  ┌──────────┐  ┌────┴──────┐ │
│  │ Database  │  │ Auth     │  │ Functions │ │
│  └──────────┘  └──────────┘  └───────────┘ │
│  ┌──────────┐  ┌──────────┐               │
│  │ Storage   │  │ Realtime │               │
│  └──────────┘  └──────────┘               │
└─────────────────────────────────────────────┘

External Services: [List — e.g., HealthKit, Mapbox, FCM, StoreKit]
```

_Replace this example with your project's actual architecture. Show major components, data flow directions, and the network boundary between client and server._

### Components

| Component | Technology | Purpose |
|-----------|------------|---------|
| | | |
| | | |
| | | |

---

## App Architecture

### Pattern: _[Pattern Name]_

_Why this pattern was chosen and how it applies to this project._

### Module / Feature Structure

```
[Project structure showing how code is organized]
```

### Navigation Architecture

_How the app navigates between screens. Include: root navigation, tab structure, modal flows._

---

## Data Models

### _[Model Name]_

```swift
// [Model definition with properties and types]
```

**Storage:** _[Local only / Server only / Both (synced)]_
**Relationships:** _[Related models]_

_[Repeat for each domain model]_

---

## Service Layer

### _[Service Name]_

**Responsibility:** _[Single sentence]_
**Protocol:**
```swift
// [Protocol definition]
```
**Dependencies:** _[What it needs]_
**Lifecycle:** _[Singleton / Injected / Per-use]_

_[Repeat for each service]_

---

## State Management

### Global State
_[What state is app-wide and how it's managed]_

### Local State
_[What state is view-specific]_

### Data Refresh Strategy
_[How views update when data changes]_

### Caching
_[What's cached, where, how long, when invalidated]_

---

## Testability

### Dependency Injection Strategy
_[How dependencies are provided — initializer injection, environment, container]_

### Mock Strategy
_[How services are mocked for testing]_

### Test Boundaries

| Layer | Test Type | What's Verified |
|-------|-----------|----------------|
| | | |
| | | |
| | | |

---

## Offline & Sync

### Offline Capabilities
_[What works without network]_

### Sync Strategy
_[How local changes sync]_

### Conflict Resolution
_[How conflicts are handled]_

---

## Security

### Authentication Flow
_[Sign-in flow, token management, session handling]_

### API Key Management
_[Where keys live, how they're accessed]_

### Data Privacy
_[What data leaves the device, what stays local]_

### Premium Feature Gating
_[How premium features are enforced — server-side vs. client-side]_

---

## Status

- [ ] System architecture overview complete
- [ ] App architecture pattern chosen and justified
- [ ] Module structure defined
- [ ] Navigation architecture documented
- [ ] Data models defined
- [ ] Service layer designed with protocols
- [ ] State management documented
- [ ] Testability architecture designed
- [ ] Offline/sync strategy defined
- [ ] Security architecture documented
