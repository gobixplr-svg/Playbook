# AI Token Optimization Guide

## Overview

AI-assisted development is powerful but token-expensive. Every message you send carries context — your CLAUDE.md, conversation history, file contents, tool definitions — and you pay for all of it on every exchange. This guide documents techniques to get more value per token across the entire development lifecycle.

**Core insight:** 99% of token usage is *input* tokens. The AI reads 100-160x more than it writes. The biggest savings come from controlling what enters context, not from shorter responses.

## The Cost Model

Understanding where tokens go is the first step to optimizing them.

| Context Source | Loaded When | Impact |
| --- | --- | --- |
| CLAUDE.md | Every message | Persistent — keep under 200 lines |
| Conversation history | Every message (growing) | Compounds — clear between tasks |
| MCP tool definitions | Every message (deferred by default) | Per-server overhead — disable unused |
| File reads | On demand | Largest variable — read only what you need |
| Extended thinking | Every response | Output tokens — adjust budget per task |
| Subagent contexts | Per subagent | Isolated — verbose output stays contained |

## Universal Techniques

These apply regardless of which phase you're in.

### 1. Write Specific Prompts

The single highest-impact habit. Vague prompts trigger broad exploration; specific prompts resolve in one pass.

| Prompt | Tokens Used | Why |
| --- | --- | --- |
| "Fix the login bug" | 50-100K | Reads auth files, tests, configs, guesses at the issue |
| "Fix JWT expiry check in `src/auth/validate.ts:42` — it's comparing seconds to milliseconds" | 5-10K | Goes straight to the line and fixes it |

**Rule of thumb:** Include the file path, the function name, and what's wrong. If you know the line number, include it.

### 2. Manage Context Proactively

- **Clear between tasks.** Use `/clear` when switching to unrelated work. Stale context from a bug fix wastes tokens on every message of your next feature task.
- **Rename before clearing.** Use `/rename` so you can `/resume` later if needed.
- **Use `/compact` mid-session.** When a conversation grows long, compact it with focus instructions: `/compact Focus on the auth module changes and test results`.
- **Monitor with `/cost`.** Check token spend periodically. If a task is burning more than expected, stop and reassess your approach.

### 3. Optimize CLAUDE.md

Your CLAUDE.md loads on every single message. Every unnecessary line costs tokens across the entire session.

- **Target: under 200 lines.** Include only project overview, architecture essentials, conventions, and common commands.
- **Move specialized content to skills.** Deployment procedures, PR review checklists, database migration guides — these should load on demand, not on every message.
- **Use tiered context architecture:**
  - **Tier 1 (CLAUDE.md):** <800 tokens — project identity, key conventions, active phase
  - **Tier 2 (Skills/docs):** Loaded on demand — component guides, API references, workflow docs
  - **Tier 3 (Reference):** Linked but never preloaded — full specs, historical decisions

### 4. Exclude Noise With .claudeignore

Build artifacts, dependencies, and generated files add noise and waste tokens.

```text
# .claudeignore
node_modules/
.next/
dist/
build/
*.lock
*.generated.*
.git/
coverage/
```

For iOS projects:
```text
DerivedData/
*.xcodeproj/xcuserdata/
Pods/
.build/
```

### 5. Choose the Right Model for the Task

Not every task needs the most capable model.

| Task Type | Model | Why |
| --- | --- | --- |
| Architecture decisions, complex debugging | Opus | Deep reasoning justifies the cost |
| Feature implementation, refactoring | Sonnet | Handles most coding tasks well at lower cost |
| File exploration, simple lookups, formatting | Haiku (subagent) | Fast, cheap, good enough for read-only work |

Switch mid-session with `/model`. Set subagent models with `model: haiku` in agent configurations.

### 6. Delegate Verbose Operations to Subagents

Test output, log analysis, and documentation searches can consume massive context. Subagents run in isolated context windows — only a summary returns to your main conversation.

**Use subagents for:** running test suites, searching documentation, exploring unfamiliar code, fetching web content.

**Keep in the main context:** architectural decisions, code writing, debugging with context you've already built up.

### 7. Use Hooks to Preprocess Data

Instead of Claude reading a 10,000-line log to find errors, a hook can grep for `ERROR` and return only matching lines — reducing context from tens of thousands of tokens to hundreds.

**High-value hooks:**
- Filter test output to show only failures
- Truncate build logs to errors and warnings
- Summarize git diffs before Claude sees them

### 8. Install Code Intelligence Plugins

Language server plugins give Claude precise "go to definition" navigation instead of text-based grep-and-read. One symbol lookup replaces reading multiple candidate files.

### 9. Reduce MCP Server Overhead

Each connected MCP server adds tool definitions to context. Tool definitions are deferred by default (only names loaded until used), but unused servers still add overhead.

- Run `/mcp` to see configured servers
- Disable servers you're not using this session
- Prefer CLI tools (`gh`, `aws`, `sentry-cli`) when available — zero per-tool listing overhead

### 10. Adjust Extended Thinking

Extended thinking improves complex reasoning but burns output tokens. For simple tasks, reduce the budget:

- `/effort` to lower effort level for routine work
- `MAX_THINKING_TOKENS=8000` for a hard cap
- Disable thinking entirely in `/config` for pure implementation tasks

## Phase-Specific Optimization

Different development phases have fundamentally different token profiles. What wastes tokens in Phase 0 is different from what wastes them in Phase 2.

### Phase 0: Concept — Broad but Bounded

**Token profile:** High exploration, moderate generation. The AI is helping you think, not build.

**Optimize for:**
- Use the Guided Concept Session (Step 0) — it's a structured conversation that avoids the back-and-forth waste of open-ended brainstorming
- Front-load context: paste your rough idea in full rather than revealing it piece by piece across messages
- For competitive analysis, give the AI specific competitors to research rather than "find competitors for me" — broad searches burn tokens on irrelevant results
- Save the Concept Brief at the end — this compressed summary prevents future sessions from re-reading the full Concept Document

### Phase 1: Pre-Production — Heavy Spec Writing

**Token profile:** Moderate reads, heavy generation. Lots of structured output (specs, schemas, plans).

**Optimize for:**
- Use Plan Mode (Shift+Tab) before generating specs — preview the approach before committing tokens to full output
- Work one spec at a time. Don't ask the AI to "write the architecture and API spec" in one message — the combined context explodes
- Reference previous specs by summary, not by re-reading. "The architecture uses MVVM with a Supabase backend (see arch doc)" is cheaper than having the AI re-read the full architecture document
- Template-driven generation saves tokens — the AI fills in structure rather than inventing it
- Run cross-spec consistency checks (Step 7.7) as a focused task, not embedded in other work

### Phase 2: Production — Targeted Implementation

**Token profile:** Highest total spend. Many sessions, each with targeted reads and writes.

**Optimize for:**
- **Prompt specificity matters most here.** Every sprint task should reference the file, function, and acceptance criteria by name
- Structure sessions around single sprint tasks. One feature per session, `/clear` between tasks
- Keep sprint context in CLAUDE.md: current sprint number, active stories, architecture summary. This prevents re-discovery at session start
- Commit frequently — small commits are cheap save points. Re-doing work after an AI wrong turn is the most expensive token waste
- Use `/rewind` or Escape immediately when the AI heads the wrong direction. Every wrong-direction message compounds context cost
- Run tests scoped to the module you changed, not the full suite — "run auth tests" not "run all tests"

### Phase 3: Testing & QA — Repetitive Validation

**Token profile:** Lots of test execution and log reading. Repetitive patterns.

**Optimize for:**
- Hooks that filter test output to failures only — full test output is the #1 token waste in QA
- Subagents for running test suites — keep verbose output out of your main context
- Batch related bugs into a single session rather than one session per bug (if they're in the same module)
- For performance profiling, summarize Instruments/profiler output before pasting it in — raw profiler data is extremely token-expensive

### Phase 4: Launch — Checklist Execution

**Token profile:** Low. Mostly checklist-following and asset preparation.

**Optimize for:**
- Use skills for compliance checklists and store submission procedures — load on demand rather than holding in context
- Screenshot and asset generation doesn't need AI context — use dedicated tools (AppShot, BatchSnap) directly
- For App Store description writing, provide the concept brief and keywords in one prompt rather than iterating

### Phase 5: Post-Launch — Monitoring and Iteration

**Token profile:** Intermittent. Spiky around update cycles, low during monitoring.

**Optimize for:**
- Summarize analytics and crash reports before bringing them to the AI — raw dashboards are token-heavy
- For iteration planning, start with the roadmap and feedback log, not a full codebase re-read
- Update cycles are mini Phase 2 sprints — apply the same Phase 2 optimizations
- Keep CLAUDE.md current with the latest architecture state so returning sessions don't waste tokens re-discovering what changed

## Session Hygiene Checklist

Use this before and during every AI-assisted session:

- [ ] **CLAUDE.md is lean** — under 200 lines, no stale content
- [ ] **.claudeignore exists** — build artifacts and dependencies excluded
- [ ] **Unused MCP servers disabled** — check with `/mcp`
- [ ] **Clear between tasks** — `/rename` then `/clear` when switching context
- [ ] **Prompts include specifics** — file paths, function names, line numbers when known
- [ ] **Monitor spend** — `/cost` check after complex operations
- [ ] **Compact when long** — `/compact` with focus instructions mid-session
- [ ] **Right model selected** — Opus for architecture, Sonnet for implementation, Haiku for subagents

## Anti-Patterns

These are the most common ways tokens get wasted. Avoid them.

| Anti-Pattern | Token Cost | Fix |
| --- | --- | --- |
| "Improve this codebase" | 100K+ | Name the specific file and improvement |
| Re-reading files the AI already read this session | 60-90% of Read tokens | Track what's been read; reference by summary |
| CLAUDE.md over 500 lines | Thousands per message | Move specialized content to skills |
| Running full test suite after every change | 10-50K per run | Scope tests to changed module |
| Launching broad search agents for known files | 20-50K | Use Read or Glob directly |
| Open-ended "find competitors" or "research this" | 30-100K | Name specific competitors or topics |
| Not clearing between unrelated tasks | Compounds 10-30% per message | `/clear` is free; stale context isn't |
| Dumping raw logs or profiler output | 10-100K per paste | Preprocess with hooks or summarize first |

## Measuring Improvement

Track these metrics across projects to gauge optimization effectiveness:

- **Cost per sprint** — total `/cost` across all sessions in a sprint
- **Tokens per task** — average token spend per completed sprint task
- **Session count per feature** — fewer sessions = better context management
- **Redo rate** — how often you `/rewind` or restart after wrong-direction work

Compare early projects against later ones. As your CLAUDE.md, .claudeignore, hooks, and prompting habits improve, these numbers should trend down.
