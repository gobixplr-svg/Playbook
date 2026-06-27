# Doc Surface Discipline

## Overview

Across a long project, documentation accretes. The same "what happened this session" narrative gets pasted into the state file, the priming doc, the roadmap, and the commit message — four copies of the same prose, growing every wrapup. Six months in, every cold start reads thousands of lines of historical paragraphs before the AI is useful. This guide names the failure pattern and gives you the discipline + tooling to prevent it.

**Core insight:** every documentation surface has exactly one role. When narrative lives in multiple places, drift, bloat, and stale-context costs compound. Pick one home for session narrative and edit everything else in place.

## The Failure Pattern

Without discipline, every wrapup cycle does this:

1. Prepend a session paragraph to the state file's "Current Step" field
2. Prepend the same paragraph to the priming doc's "Current State" section
3. Add a "Session NN:" block at the top of the roadmap
4. Add a row to the deliverables log with a wrapup paragraph stuffed in the description cell

After N sessions, the same narrative lives in 4 files at N copies each. Every cold start reads all of it. Token cost grows linearly with session count.

**Real numbers from a project that hit this wall at 44 sessions:**

| Surface | Size Before | Size After | What was happening |
| --- | --- | --- | --- |
| `docs/playbook-state.md` | 402 KB / 281 lines | 11 KB / 126 lines | "Current Step" field had accreted to a 32K-token single line; Deliverables Log had paragraph-sized cells |
| Priming doc "Current State" | 36 KB | 1.9 KB | 8 stacked session paragraphs in the section that loads on every cold start |
| Roadmap top | 5 wrapup blocks | 0 blocks | Status updates pasted as time-stamped blocks instead of editing theme rows in place |

The discipline below would have prevented all of it. The mechanical enforcement at the bottom is the safety net.

## Surface Roles

Five documentation surfaces, five distinct roles. Don't duplicate narrative across them.

| Surface | Role | Edit Pattern | Size Budget |
| --- | --- | --- | --- |
| Session log (`docs/playbook-session-log.md`) | Per-session narrative — append-only journal | NEW block at top of "Sessions" section, newest first | Unbounded (history) |
| State file (`docs/playbook-state.md`) | Phase tracking — current phase/step, checklist, recent deliverables | EDIT-IN-PLACE one-sentence "Current Step"; optional 1-row Deliverables Log add | < 15 KB (hard < 20 KB) |
| Roadmap (`docs/ROADMAP.md`) | Forward planning — themes, milestones, blockers, critical path | EDIT-IN-PLACE status lines + blocker bullets; NO session-NN blocks at top | < 50 KB (soft) |
| Priming doc (`CLAUDE.md`) | Agent priming — tech stack, conventions, file pointers, version | Edited only on convention/structure/version change; "Current State" sub-section is a one-sentence pointer | "Project Context" section < 4 KB (hard < 8 KB) |
| Commit messages | Per-commit context — what changed + why | One per commit; conventional commit prefix | n/a (git) |
| Decision log (`docs/decisions.md`) | Decision retrieval — a queryable index of significant decisions | APPEND a row at decision time; point to the ADR/memory holding the "why"; never rewrite a row | < 30 KB (soft) |

The state file and priming doc are the surfaces that get read most often by AI agents on cold start. The session log is the durable history. The roadmap is the forward-looking plan. Commit messages are the immutable record. The decision log is the retrieval index — see "The Decision Log" below for why narrative and ADRs alone don't cover it.

## The Five Discipline Rules

These ride on the surface roles above. Internalize them; apply on every wrapup.

### Rule A — Narrative Goes to ONE Place

The multi-paragraph "what we did this session" block goes only into `docs/playbook-session-log.md` as a new `### Session NN` block. Newest first. Every other surface gets edit-in-place deltas, not new prose.

If `docs/playbook-session-log.md` does not exist, create it at the start of Phase 2 (Production) with this skeleton:

```markdown
# Playbook Session Log

Append-only per-session narrative. Newest at top. Detailed prose lives here
so docs/playbook-state.md stays a small, scannable state file.

## Sessions

(New session blocks go here: ### Session NN (weekday YYYY-MM-DD time) — title)
```

### Rule B — Deltas, Not Additions, Everywhere Else

- **State file "Current Step":** EDIT the existing one-sentence line. Don't prepend a new paragraph.
- **Roadmap theme rows + blocker bullets:** EDIT in place. Don't add a "Session NN:" block at the top.
- **Priming doc "Current State":** EDIT only if version/structure actually changed. If the session was doc-only or didn't change conventions, leave it alone.

### Rule C — Deliverables Log Is a Pointer Table

Optional 1-row addition only when the session shipped a concrete, named, durable deliverable (a feature slice, a runbook, a migration). Cell content rules:

- **Description cell:** ≤ 200 characters. If you need more space, the entry belongs in the session log narrative, not the table.
- **Path cell:** file paths, ≤ 120 characters. Use brace expansion if listing multiple files.
- **Date:** ISO `YYYY-MM-DD`.

Doc-only commits typically don't warrant a Deliverables Log entry — their record lives in commit messages + session log narrative.

### Rule D — Size Budgets Are Real

After your wrapup edits, before committing:

```bash
wc -c docs/playbook-state.md
awk '/^## Project Context/{flag=1; next} /^## /{flag=0} flag' CLAUDE.md | wc -c
```

If either is over budget, you violated Rule A or Rule B. Fix before committing.

### Rule E — Foundation Docs Refresh on Triggers, Not on Wrapups

Architecture, data model, dev environment, deployment runbook, platform dependencies — these refresh on theme-close, ADR, new platform dependency, or scheduled drift audit. NOT every wrapup. See the doc-refresh-cadence pattern in `phase-2-production/`.

## The Decision Log

The surfaces above carry *narrative* (session log), *state* (state file), and
*plans* (roadmap). None is built for **retrieving a decision** — "what did we
decide about X, and why?" The session log holds the answer but it is an
append-only journal that grows unbounded (one real project hit 660 KB); grepping
it for a decision among thousands of narrative paragraphs does not scale. Two
existing patterns help but leave a gap:

- **ADRs** (`docs/adr/`) capture *architectural* decisions in full — context,
  decision, consequences. Use the `adr.md` template (`phase-1-pre-production/templates/`).
  But not every durable decision is architectural, and a full ADR is too heavy
  for a product/scope/ops call.
- **Memories** (the agent's persistent notes) capture the "why" of many
  product/ops decisions — but they're per-agent, not a project artifact, and not
  enumerable by a teammate reading the repo.

`docs/decisions.md` closes the gap: a **queryable index**, not a new archive. It
is a pointer-table — each row names the decision and links to where the reasoning
lives (an ADR, a memory, a design doc). Seed it from your existing ADRs; add a
row whenever a decision is made (at decision time, not at wrapup). Use the
`decision-log.md` template (`phase-1-pre-production/templates/`).

**What belongs:** a choice between real alternatives with lasting consequences.
Architectural → also a full ADR. Product/scope/strategy/ops → a memory + a row.
**What does NOT:** status (roadmap), narrative (session log), task tracking, or
pure style preferences.

**The decision sweep.** Compaction moves narrative out of the hot files into the
session log. The hazard: a *decision* embedded in that prose gets archived into a
660 KB journal where it is no longer retrievable. So compaction gains one step —
before trimming, scan the prose being removed for any decision (not just status)
and confirm it is captured in `docs/decisions.md` (and an ADR/memory if
warranted) before it leaves the hot file. This is folded into the Compaction
Recipe below as step 0.

## Mechanical Enforcement

Discipline that depends only on memory will drift. Three mechanical safeguards:

### 1. Pre-Commit Hook (Husky)

Add to `.husky/pre-commit`:

```sh
#!/usr/bin/env sh

check_size() {
  file="$1"; warn_kb="$2"; fail_kb="$3"
  [ ! -f "$file" ] && return 0
  bytes=$(wc -c <"$file" | tr -d ' ')
  warn_bytes=$((warn_kb * 1024))
  fail_bytes=$((fail_kb * 1024))
  if [ "$bytes" -ge "$fail_bytes" ]; then
    echo "ERROR: $file is ${bytes} bytes (limit ${fail_kb}KB)."
    echo "       Narrative accreting. Move per-session prose to"
    echo "       docs/playbook-session-log.md."
    return 1
  fi
  if [ "$bytes" -ge "$warn_bytes" ]; then
    echo "WARN:  $file is ${bytes} bytes (warn ${warn_kb}KB, hard ${fail_kb}KB)."
  fi
}

check_claude_project_context() {
  [ ! -f CLAUDE.md ] && return 0
  bytes=$(awk '/^## Project Context/{flag=1; next} /^## /{flag=0} flag' CLAUDE.md | wc -c | tr -d ' ')
  if [ "$bytes" -ge $((8 * 1024)) ]; then
    echo "ERROR: CLAUDE.md 'Project Context' section is ${bytes} bytes."
    echo "       Restore 'Current State' to a one-sentence pointer."
    return 1
  fi
  [ "$bytes" -ge $((4 * 1024)) ] && echo "WARN: CLAUDE.md 'Project Context' is ${bytes} bytes."
  return 0
}

check_size docs/playbook-state.md 15 20 || exit 1
check_claude_project_context || exit 1

npx lint-staged
```

The hook fires on every commit. Bypassable only with `--no-verify`, which is loud and intentional.

### 2. Prettier Ignore for Append-Only Docs

If you use Prettier with markdown table formatting, add to `.prettierignore`:

```text
# Prettier markdown table-align silently 30x's file size on
# append-only docs by padding every cell to the longest cell width.
docs/playbook-state.md
docs/playbook-session-log.md
```

This was the most surprising failure mode in the case study above — a single long Deliverables Log cell caused Prettier to pad 128 other rows to match, multiplying file size 30x without any visible content change.

### 3. Compaction Recipe

When bloat is already present (legacy projects adopting this discipline):

0. **Decision sweep.** Before trimming anything, scan the prose you're about to
   move/remove for any *decision* (not just status). For each, confirm it's
   captured in `docs/decisions.md` (and an ADR/memory if warranted). This keeps
   decisions retrievable instead of buried in the archived narrative.
1. Create `docs/playbook-session-log.md` from the skeleton in Rule A
2. Move per-session narrative paragraphs verbatim from state file / priming doc into a "Snapshot" section of the session log (preserved as historical record — don't try to split or edit)
3. Replace the state file's "Current Step" with a one-sentence description of where the project actually is right now
4. Trim Deliverables Log to the most recent ~15 entries with ≤200-char description cells; older rows move to the session log as a "Historical deliverables" appendix
5. Prune priming doc "Current State" to one sentence + pointer to the session log
6. Add the hook + prettierignore so it doesn't bloat again

Verify with `wc -c` against the size budgets.

## Anti-Patterns

| Anti-Pattern | What it costs | Fix |
| --- | --- | --- |
| Pasting wrapup paragraphs into 4 surfaces every session | Linear bloat with session count; cold-start tokens grow forever | Narrative to session log only; deltas elsewhere |
| Multi-paragraph "Current Step" field in state file | State file unreadable; 32K-token single lines | One sentence; if you need more, it goes in session log |
| "Session NN:" blocks stacked at top of roadmap | Roadmap loses its forward-planning identity; becomes a backwards-facing log | Edit theme status lines in place |
| Prose stuffed into table cells (Deliverables Log description column) | Prettier table-align cascades into 30x file bloat | ≤ 200 chars; if more is needed, the entry belongs in the session log |
| Refreshing every foundation doc on every wrapup | Doc-edit churn; obscured signal about what actually changed | Trigger-based refresh only (theme-close, ADR, new platform dep) |
| Bypassing the pre-commit hook with `--no-verify` | The discipline is now optional in your project | Treat hook failures as a real signal; compact, don't bypass |
| Adopting the discipline only in CLAUDE.md (the priming doc) | State file still bloats invisibly; agent reads it via state-file pointer | Discipline applies to all 4 surfaces, not just the priming doc |
| Decisions live only in session-log narrative | "Why did we choose X?" means grepping a 660 KB journal; decisions get buried on compaction | Index significant decisions in `docs/decisions.md`; run the decision sweep before compaction |

## When This Discipline Fires

In playbook phase terms:

- **Phase 0-1:** Surface roles aren't fully established yet; state file may not exist. Get the session log skeleton in place by end of Phase 1 so Phase 2 starts clean.
- **Phase 2 (Production):** Discipline applies on every sprint wrapup. This is where the discipline is tested and where the bloat compounds if absent.
- **Phase 3-5:** Lower wrapup frequency; discipline still applies but is easier to maintain.

The discipline scales: a 2-week project barely needs it, a 6-month project critically needs it, a multi-year project cannot survive without it.

## Related

- `phase-2-production/steps/02-retrospective.md` — wrapup discipline lives here
- `docs/ai-token-optimization.md` — broader context on why narrative bloat is expensive
- `phase-1-pre-production/` — establish session log + hooks during dev environment setup
- `phase-1-pre-production/templates/adr.md` — Architecture Decision Record template (Nygard format)
- `phase-1-pre-production/templates/decision-log.md` — decision-log index template
