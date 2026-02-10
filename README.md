# Volv

Disciplined workflow skills for Claude Code — session lifecycle, code review, and test-driven development practices.

---

## What This Is

A Claude Code plugin that gives your agent disciplined engineering practices:

- **Session management** — structured start and end to every work session
- **Code review** — Dave Farley-style assessment of changes, plans, and codebases
- **Test-driven workflow** — a CLAUDE.md template encoding TDD, small increments, and checkpoints

Install once, use across all your projects.

---

## Quick Start

### As a plugin (recommended)

```
/plugin install volv@rogamorris/volv-code
```

This gives you all five skills, available in any project via the `/volv:` prefix.

### As a standalone template

If you just want the TDD workflow without the plugin:

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/rogamorris/volv-code/main/templates/CLAUDE.md
```

Or copy the contents of [`templates/CLAUDE.md`](templates/CLAUDE.md) manually.

**Prerequisites** for either approach:
- An automated test suite (or willingness to start one)
- CI pipeline
- Ability to deploy frequently

> See [Prerequisites](VISION.md#prerequisites) for details.

---

## Skills

### `/volv:start` — Session setup

Sets up a working session with git worktrees. Shows repo state, pulls main if needed, and helps you create a worktree for new work or stay on the current branch.

### `/volv:end` — Session wrap-up

Wraps up cleanly: commits uncommitted work, pushes to remote, offers PR creation, and handles worktree cleanup. Nothing gets lost.

### `/volv:review-change` — Change review

Reviews recent code changes (the last commit or staged diff) through the lens of Dave Farley's Modern Software Engineering. Evaluates locality, comprehensibility, test quality, accidental complexity, and reversibility.

### `/volv:review-plan` — Plan review

Reviews a plan with the central question: "Is this the smallest plan that moves us forward and lets us learn something?" Looks for scope creep, premature abstraction, and coupled steps.

### `/volv:review-codebase` — Full codebase review

Comprehensive codebase review across seven dimensions. Produces a dimensional assessment and a prioritized refactoring file in `docs/tech-debt/` that fresh Claude Code sessions can work through independently.

---

## The CLAUDE.md Template

The standalone template (`templates/CLAUDE.md`) encodes a small set of practices:

| Practice | What It Does |
|----------|--------------|
| Test-first | Write a failing test before any production code |
| Small increments | One behavior at a time, deployable independently |
| Checkpoints | Stop after each increment, report what happened, wait for approval |
| Plan before complexity | For ambiguous or multi-file tasks, write a plan first |

These practices compound. They feel slower at first but produce faster, more reliable results — because you spend less time debugging and reverting.

### Customizing the template

CLAUDE.md is yours. Edit it freely.

**Add project-specific guidance:**
```markdown
## Project-Specific

- Use pytest for all tests
- Follow existing naming conventions in /src
- Database migrations require approval before running
```

**Remove what doesn't fit:**

If checkpoints feel too heavy for your workflow, remove that section. The file is a starting point, not a mandate.

---

## Philosophy

This project is built on a specific engineering philosophy:

> Build systems that can change safely by working in small steps, validating continuously, and using feedback to guide architecture and design.

Key principles:
- Walking skeletons over big-bang delivery
- TDD as a design tool, not just testing
- Continuous refactoring, not periodic cleanup
- Evolutionary architecture over upfront design

> Read the full [Vision](VISION.md) for the complete philosophy.

---

## Status

**Phase:** Plugin MVP

The core CLAUDE.md template has been validated in production codebases. The plugin structure packages battle-tested skills for broader distribution via Claude Code's plugin system.

What we're learning:
- Which skills provide the most value across different project types
- How session lifecycle management (start/end) changes work quality
- Whether review skills meaningfully improve code and plan quality

> See [Current State](VISION.md#current-state) for details.

---

## Contributing

Try it. Tell us what happened.

The most useful contributions right now:
- "I tried this and X worked well"
- "I tried this and Y was ignored"
- "I needed to add Z for my context"

Open an issue or PR.

---

## License

MIT. Use it, fork it, adapt it.
