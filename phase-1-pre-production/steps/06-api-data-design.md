# Step 6: API & Data Design

## Purpose

Define the complete data layer: database schema, API contracts, data flows, and server-side logic. By the end of this step, the client app knows exactly what to send, what to expect back, and where every piece of data lives — so that Phase 2 development can build against a stable contract.

## Why This Matters

Without a data design spec, you'll discover schema problems mid-sprint when a query doesn't return what the UI needs, or when two tables need a join you didn't plan for. Fixing schema decisions after code is written is expensive — migrations, client refactors, and broken tests.

This step formalizes three things:
1. **Schema** — what tables exist, what columns they have, how they relate
2. **API contracts** — what the client calls and what comes back
3. **Data flows** — how data moves between the client, backend, and external services

## Prerequisites

- Step 3 (Architecture) completed — data models, service protocols, and system components defined
- Step 2 (Spikes) completed — any database-related spike findings incorporated
- Step 4 (UI/UX) completed — knowing what data each screen displays informs the API contracts

## Process

### 6.1 Design the Database Schema

Translate the data models from Step 3 into a concrete database schema for your chosen backend.

**For each table:**
- Column names and data types (match your backend's type system)
- Primary keys and unique constraints
- Foreign key relationships with cascading behavior
- Default values and computed columns
- Indexes for common query patterns
- Check constraints for data validation

**Schema design principles:**
- Start from the data models in the architecture document — they define the domain
- Add database-specific concerns: timestamps (`created_at`, `updated_at`), soft deletes, audit columns
- Use your backend's native features (PostGIS for geometry, JSONB for flexible data, etc.)
- Keep it normalized for v1 — denormalize only when you have a proven performance problem
- Name columns consistently: `snake_case`, past tense for booleans (`is_active`, not `active`)

### 6.2 Define Row-Level Security (RLS) Policies

If your backend supports row-level security (Supabase, Firebase), define access policies for every table:

**For each table, define who can:**
- **Read** — which rows can which users see?
- **Insert** — who can create new rows?
- **Update** — which rows can which users modify? Which columns?
- **Delete** — who can delete rows? Or use soft deletes?

**Common patterns:**
- Users can only read/write their own data: `auth.uid() = user_id`
- Public data is readable by all: route library, badge definitions
- Premium content gating: only return premium routes for subscribed users
- Admin-only data: route creation, badge definition management

### 6.3 Define API Contracts

For each service protocol from Step 3, define the concrete API operations:

**For each operation, document:**
1. **Method** — the function signature or HTTP method
2. **Input** — parameters with types and validation rules
3. **Output** — response shape (success and error)
4. **Auth** — authentication/authorization requirements
5. **Caching** — should the client cache this? For how long?
6. **Pagination** — does this need pagination? What strategy?

**API contract format:**

```
### fetchRoutes

Purpose: Get the route library for the current user
Auth: Required (determines premium access)
Method: Supabase query — routes table

Input:
- difficulty: String? (optional filter)

Output (success):
- [Route] — Array of Route objects
- Premium routes included only if user has active subscription

Output (error):
- .networkError — no connectivity
- .unauthorized — session expired

Caching: 24 hours, invalidate on pull-to-refresh
Pagination: None for v1 (< 20 routes)
```

### 6.4 Define Data Flows

Map how data moves through the system for each major operation:

**Types of data flows:**
1. **Client → Server:** Creating or updating data (start route, update progress)
2. **Server → Client:** Fetching data (load routes, fetch badges)
3. **External → Client → Server:** Health data ingestion (HealthKit → app → Supabase)
4. **Server → Client (realtime):** Push updates (social reactions, friend activity)
5. **Client → External:** Sharing (share card → iOS share sheet)

**For each flow, document:**
- Trigger (what starts the flow)
- Steps in sequence
- Where data transforms happen
- Error handling at each step
- Offline behavior

### 6.5 Define Server-Side Logic

Some logic must run on the server — not in the client:

**Common server-side operations:**
- Subscription validation (verify App Store receipts)
- Premium content gating (only return premium routes to subscribers)
- Rate limiting (friend requests, API calls)
- Data aggregation (leaderboards, global stats)
- Push notification triggers (milestone reached → send push)
- Scheduled jobs (streak calculation at midnight)

**For each server function:**
- Trigger (API call, schedule, webhook)
- Input
- Logic
- Output
- Error handling

### 6.6 Define Storage Buckets

If your backend includes file storage, define the bucket structure:

**For each bucket:**
- Name and purpose
- Access policy (public, authenticated, specific users)
- File naming convention
- Size limits
- Accepted file types

### 6.7 Verify Coverage

Check the API spec against requirements:

**Verification checklist:**
- [ ] Every data model from Step 3 has a corresponding database table
- [ ] Every service protocol method has an API contract
- [ ] Every screen in the screen inventory can be populated from the defined APIs
- [ ] RLS policies enforce premium gating server-side
- [ ] Offline-capable operations are identified
- [ ] Error states are defined for all API operations
- [ ] Data flows cover the full lifecycle (create → read → update → delete)

## Deliverable

Complete the [API Spec Template](../templates/api-spec.md) and save it to your project's `docs/pre-production/api-spec.md`.

## Definition of Done

- [ ] Database schema defined for all tables (columns, types, relationships, indexes)
- [ ] Row-level security policies defined for every table
- [ ] API contracts defined for every service protocol method
- [ ] Data flows mapped for all major operations
- [ ] Server-side logic documented (edge functions, validation, scheduled jobs)
- [ ] Storage buckets defined with access policies
- [ ] Schema supports all Must Have requirements
- [ ] Premium content gating enforced server-side (not client-only)
- [ ] Offline data strategy defined (what's cached, what requires network)
- [ ] Coverage verified against screen inventory and requirements

## Tips

- **Start from the spike.** If a spike validated a schema design, use it — don't redesign from scratch.
- **The client shouldn't trust itself.** Pricing, subscription status, and premium access must be validated server-side. The client can show a lock icon, but the server must actually withhold the data.
- **Design for the queries you'll run.** Look at each screen and ask: "What data does this screen need?" Then ensure the schema supports that query efficiently (indexes, joins, etc.).
- **Don't over-normalize.** If you always fetch a route with its milestones, consider whether a join is fine or if you'd rather embed milestones. For v1 with < 20 routes, normalized is fine.
- **RLS is your security layer.** With Supabase, RLS policies replace traditional API middleware. Every table needs explicit policies — tables without policies are inaccessible by default.
- **Version your API from day one.** Even if you're a solo dev, naming things `v1` makes future changes easier.
