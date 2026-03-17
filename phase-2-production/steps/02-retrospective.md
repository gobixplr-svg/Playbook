# Step 2: Retrospective

## Purpose

Pause, reflect, and improve — then get back to building. Retros are the mechanism by which the Playbook, the process, and the product get better over time. Without them, mistakes repeat and process debt compounds silently.

## When to Run

Run a retro at **any** of these triggers:

| Trigger | Why |
| --------- | ----- |
| **End of sprint** | Natural checkpoint — what worked, what didn't, what carries over |
| **After major integration** | Backend, SDK, or third-party service integration surfaces process gaps |
| **After a bug-fix session** | Multiple related bugs signal a systemic issue worth capturing |
| **After context compaction** | Session ran long enough to compress — scope was probably too large |
| **After a blocked session** | Something unexpected stopped progress — capture it before it's forgotten |
| **Bi-weekly (minimum)** | If none of the above triggers fire naturally, force one every 2 weeks |

**Rule of thumb:** If you learned something that would change how you'd approach the same work next time, run a retro.

## Time Budget

- **Sprint retro:** 15-30 minutes
- **Integration retro:** 20-40 minutes
- **Lightweight retro** (bug fix, blocked session): 10-15 minutes

If the retro is taking longer than this, you're writing an essay. Be specific, be honest, be brief.

## Process

### 2.1 Set the Scope

Name the retro and define what it covers. Retros without clear boundaries become unfocused rambles.

```markdown
## Retro: [Name]
- **Scope:** [What work period or event this covers]
- **Date:** [When]
- **Trigger:** [Sprint end / Integration / Bug fix / Scheduled / Other]
```

### 2.2 Three Questions

Answer these three questions honestly. Brief, specific answers beat lengthy narratives.

#### What went well?

Things that worked, should continue, or should be replicated. Look for:
- Patterns that prevented bugs or saved time
- Architecture decisions that paid off
- Process steps that produced useful output
- Tools or techniques that were particularly effective

**Write 3-5 bullet points.** If something "went well" but you can't explain why, it might just be luck. Only capture what's repeatable.

#### What didn't go well?

Things that caused friction, wasted time, or produced bugs. Be honest — this is where the value is. Look for:
- Errors that required backtracking
- Assumptions that were wrong
- Missing information that would have changed the approach
- Process steps that were skipped and shouldn't have been
- Work that had to be redone

**Write 3-5 bullet points.** For each one, note whether it was a **one-off mistake** or a **systemic gap**. One-off mistakes don't need process changes. Systemic gaps do.

#### What would you change?

Specific, actionable changes. This is the difference between a retro that improves the process and one that's just venting.

For each change, categorize it:

| Category | What to do | Example |
| ---------- | ----------- | --------- |
| **Playbook change** | Modify an existing step guide | "Add SDK verification step to sprint execution" |
| **New anti-pattern** | Add to the anti-patterns list | "SQL Forward References — define functions before policies" |
| **Memory update** | Add to MEMORY.md | "supabase-swift signUp returns non-optional User" |
| **Process change** | Change how work is structured | "Split 5+ phase plans across multiple sessions" |
| **Template change** | Modify a template | "Add infrastructure pre-flight to sprint tracker" |

**Write 2-4 changes.** If you have more, prioritize. Too many changes at once means none get adopted.

### 2.3 Capture Lessons Learned

Distill findings into two artifact types:

#### Anti-patterns (things to avoid)

Format: **Name** — One-sentence description of what goes wrong and how to prevent it.

Good anti-pattern: "SQL Forward References — Defining functions after the RLS policies that reference them causes migration failures. Always define functions before policies."

Bad anti-pattern: "Be careful with SQL." (Too vague to act on.)

#### Lessons (things to remember)

Format: **What happened** → **What we learned** → **What to do next time.**

Good lesson: "supabase-swift signUp returns non-optional User → we assumed it was optional and wrote a guard-let that didn't compile → read SDK docs or source for specific method signatures before writing service code."

Bad lesson: "Check the docs." (Doesn't capture what specifically to check for.)

### 2.4 Apply Changes

This is the most important section. A retro without applied changes is just a diary entry.

For each change identified in 2.3:

1. **Do it now.** Update the playbook file, add the anti-pattern, update MEMORY.md. Don't create a backlog item — the retro IS the time to apply changes.
2. **Verify the change.** Read the modified file. Does it integrate cleanly with existing content? Does it contradict anything already there?
3. **Mark it done.** Record what was changed and where.

```markdown
### Changes Applied
- [ ] [Change description] → [File modified]
- [ ] [Change description] → [File modified]
```

### 2.5 Update Project Artifacts

After applying playbook changes, update project-level tracking:

- **ROADMAP.md** — Move completed work to Done, update Active section, note any scope changes
- **MEMORY.md** — Add new lessons, patterns, or gotchas that will be useful in future sessions
- **Sprint tracker** — If this is a sprint retro, close out the sprint tracker

### 2.6 Forward Look

End with a brief forward look. This creates continuity for the next session.

```markdown
### What's Next
- [Next priority item]
- [Any risks or blockers to watch for]
- [Preparation needed before next work session]
```

## Retro Quality Checklist

Before closing the retro, verify:

- [ ] Scope is clearly defined (not "everything")
- [ ] "What went well" has specific, repeatable patterns (not just "things worked")
- [ ] "What didn't go well" distinguishes one-off mistakes from systemic gaps
- [ ] "What would you change" has actionable items (not observations)
- [ ] Lessons are specific enough to act on (not generic advice)
- [ ] Changes are **applied** to the actual files (not just documented)
- [ ] ROADMAP.md is updated
- [ ] MEMORY.md is updated (if applicable)

## Anti-Pattern: Retro Theater

The retro anti-pattern is doing retros that don't change anything. Signs:

- "What would you change?" section is empty or says "nothing"
- Lessons are generic platitudes ("test more", "plan better")
- Changes are documented but never applied to files
- The same issues appear in consecutive retros
- Retros are skipped when things are "going well" (that's exactly when they're cheapest)

If your retros aren't producing at least one concrete file change per session, either the process is already perfect (unlikely) or the retros aren't honest enough.

## Deliverable

A completed retro entry. These can live in:
- The sprint tracker (for sprint retros)
- A dedicated `docs/retros/` folder (for integration or milestone retros)
- Inline in ROADMAP.md (for lightweight retros)

The format matters less than the habit. Pick one location and use it consistently.

## Key Principle

> A retro that changes nothing is worse than no retro at all — it creates the illusion of process improvement while consuming time that could have been spent building.
