# Estimation Tracker — [Project Name]

Empirical wall-clock vs estimate data for shipping slices. Used to
calibrate future scoping doc estimates and project-specific wall-clock
multipliers.

Start this tracker the first time the project ships a scoped slice. Update
at every slice close. Recompute aggregate ratios every ~5-10 data points.

---

## Methodology

- **At slice kickoff:** log estimate in the slice's scoping doc tracker (sessions + rough wall-clock hours).
- **At slice close:** log actual sessions + rough wall-clock hours alongside the estimate. Copy the row into the Data table below.
- **Variance:** actual / estimate. >1.0 = took longer than estimated. <1.0 = faster.
- **Calibration trigger:** after ~5-10 data points, revisit the wall-clock ratio claim in project memory or scoping-doc defaults, and update accordingly.
- **Patterns to watch for:**
  - Variance by work type (schema work / pure refactor / UI / cross-cutting / cleanup)
  - Variance by surface area (single-file vs cross-file)
  - Variance by review burden (reviewer rounds, redesign loops)

## Units note

"Wall-clock hours" is intentionally rough — measuring exact hours is
expensive (start/stop, idle time, tool-wait blocks). Session count is the
high-confidence number; hours is approximate. Both are useful: sessions
tell us "how chunked," hours tell us "how much time."

---

## Data

| Date closed | Slice | Type | Est sessions | Est hours | Actual sessions | Actual hours | Variance | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| _(populated as slices ship)_ | | | | | | | | |

---

## Aggregate ratios (recompute after each ~5 data points)

- **Data points collected:** 0
- **Mean variance:** —
- **Median variance:** —
- **Variance by type:**
  - Schema work: — (n=0)
  - UI work: — (n=0)
  - Refactor: — (n=0)
  - Cross-cutting: — (n=0)
- **Implied wall-clock multiplier vs conventional team:** —
- **Last recomputation:** never

## Calibration log

Each calibration entry: date + data points used + new ratio + decision applied.

_(empty)_
