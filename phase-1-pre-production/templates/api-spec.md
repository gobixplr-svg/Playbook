# API & Data Spec — [App Name]

> Playbook Reference: [Step 6: API & Data Design](../steps/06-api-data-design.md)

## Project

**App Name:** [Name]
**Date:** [YYYY-MM-DD]
**Author:** [Name]
**Backend:** [Technology — e.g., Supabase, Firebase, custom]

---

## Database Schema

### [Table Name]

```sql
CREATE TABLE table_name (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  -- columns here
  created_at TIMESTAMPTZ DEFAULT now()
);
```

**Indexes:**
```sql
CREATE INDEX idx_name ON table_name(column);
```

**Relationships:** [Belongs to X, Has many Y]

---

_Repeat for each table..._

---

## Row-Level Security

### [Table Name] — RLS Policies

| Operation | Policy Name | Rule | Description |
| ----------- | ------------ | ------ | ------------- |
| SELECT | | | |
| INSERT | | | |
| UPDATE | | | |
| DELETE | | | |

---

_Repeat for each table..._

---

## API Contracts

### [Service Name]

#### [Operation Name]

**Purpose:** [What this operation does]
**Auth:** [Required / Public / Admin]

**Input:**
| Parameter | Type | Required | Validation |
| ----------- | ------ | ---------- | ------------ |
| | | | |

**Output (success):**
```
[Describe the response shape]
```

**Output (error):**
| Error | Cause | Client Handling |
| ------- | ------- | ----------------- |
| | | |

**Caching:** [Duration, invalidation strategy]
**Pagination:** [Strategy if applicable]

---

_Repeat for each operation..._

---

## Data Flows

### Flow 1: [Flow Name]

**Trigger:** [What starts this flow]

```
[Step 1] → [Step 2] → [Step 3]
```

**Steps:**
1. [Description]
2. [Description]
3. [Description]

**Error handling:** [What happens if a step fails]
**Offline behavior:** [What happens without network]

---

_Repeat for each flow..._

---

## Server-Side Logic

### [Function Name]

**Trigger:** [API call / schedule / webhook]
**Runtime:** [Edge Function / Database trigger / Cron]

**Input:**
- [Parameters]

**Logic:**
1. [Step]
2. [Step]

**Output:**
- [Response]

**Error handling:**
- [Strategy]

---

_Repeat for each function..._

---

## Storage

### Buckets

| Bucket | Purpose | Access | File Types | Size Limit |
| -------- | --------- | -------- | ------------ | ------------ |
| | | | | |

### File Naming Convention

```
[bucket]/[path]/[filename]
```

---

## Coverage Verification

- [ ] Every data model has a database table
- [ ] Every service method has an API contract
- [ ] Every screen can be populated from defined APIs
- [ ] RLS policies enforce access control
- [ ] Premium gating is server-side
- [ ] Offline operations identified
- [ ] Error states defined for all operations
- [ ] Storage buckets defined with access policies

---

## Status

- [ ] Database schema defined
- [ ] RLS policies defined
- [ ] API contracts defined
- [ ] Data flows mapped
- [ ] Server-side logic documented
- [ ] Storage defined
- [ ] Coverage verified
