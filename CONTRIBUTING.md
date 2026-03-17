# Contributing to the Development Playbook

Thanks for your interest in improving the Playbook! Whether you're fixing a typo, improving a step guide, or proposing a new phase template, contributions are welcome.

## Ways to Contribute

- **Fix errors** — Typos, broken links, incorrect cross-references
- **Improve clarity** — Rewrite confusing instructions, add missing context
- **Share experience** — Add anti-patterns, tips, or common pitfalls you've encountered
- **Propose new templates** — Suggest templates that would help others following the process
- **Report issues** — Open an issue if something doesn't work or doesn't make sense

## Guidelines

### Content Principles

- **Actionable over theoretical** — Every step should tell someone what to do, not just what to think about
- **Platform-agnostic** — Steps and templates should work for any tech stack unless explicitly scoped (e.g., iOS-specific asset sizes)
- **Solo-dev friendly** — Don't assume teams, sprints, or enterprise tooling. Scale up, not down
- **Opinionated but adaptable** — Recommend a clear default path, but acknowledge alternatives

### Formatting

- Use spaced table separators: `| ---- |` not `|------|`
- Include a language specifier on all code blocks (e.g., `text`, `markdown`, `swift`)
- Blank lines around headings and lists
- Step files follow this structure:
  1. Purpose
  2. Why This Matters
  3. Prerequisites
  4. Process
  5. Anti-Patterns
  6. Deliverable
  7. Definition of Done
  8. Tips

### Commit Messages

- Use [Conventional Commits](https://www.conventionalcommits.org/) style
- Common prefixes: `fix`, `feat`, `docs`, `refactor`
- Scope to the phase when relevant: `docs(phase-2): add sprint estimation tips`

## How to Submit Changes

1. Fork the repository
2. Create a branch from `main` (`git checkout -b docs/improve-phase-3-testing`)
3. Make your changes
4. Verify all internal links still work
5. Open a pull request with a clear description of what changed and why

## What We Won't Merge

- Changes that add dependencies on proprietary tools or paid services without free alternatives
- Steps that assume a specific tech stack unless clearly labeled as such
- Content that duplicates what already exists in another step — cross-reference instead
- AI-generated filler content without real-world experience behind it

## Questions?

Open an issue. We'd rather answer a question than review a PR that went in the wrong direction.
