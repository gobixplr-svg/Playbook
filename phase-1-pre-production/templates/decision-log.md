# Decision Log

> Playbook Reference: [Doc Surface Discipline → The Decision Log](../../docs/doc-surface-discipline.md)
> Copy to `docs/decisions.md`. A **queryable index of significant decisions —
> not a new archive.** Each row points to where the decision's reasoning lives
> (an ADR, a memory, a design doc). Purpose is *retrieval*: answer "what did we
> decide about X, and why?" with one scan instead of grepping the session log.
> Append-only — never rewrite a row; mark it superseded and add a new one.

**What belongs here:** a choice between real alternatives with lasting
consequences. Architectural → also a full **ADR** (`docs/adr/`, see `adr.md`).
Product / scope / strategy / ops → a memory + a row here.

**What does NOT belong:** status updates (roadmap), task tracking, per-session
narrative (session log), or pure style preferences.

---

## Architectural decisions (ADRs)

| ADR | Decision | Status |
| --- | --- | --- |
| [001](adr/001-_[slug]_.md) | _[one-line decision]_ | _[Proposed / Accepted / Superseded]_ |

## Product / scope / strategy / ops decisions (sub-ADR)

Didn't warrant a full ADR but durable. Canonical "why" lives in the linked
memory / doc.

| Date | Decision | Why / link |
| --- | --- | --- |
| _[YYYY-MM-DD]_ | _[one-line decision]_ | _[reason + link to memory/doc]_ |

---

## Maintenance

- **Add a row** when a decision is made — at decision time, not at wrapup.
- **Decision sweep on compaction:** before trimming the state file / roadmap /
  priming doc, scan the prose being removed for any *decision* (not just status)
  and confirm it is captured here (and in an ADR/memory if warranted). This is
  the step that prevents decisions getting buried in archived narrative.
