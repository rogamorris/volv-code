# Volv

A minimal workflow system for Claude Code. Drop one file into your repo and get disciplined, test-driven AI assistance.

---

## What This Is

A single `CLAUDE.md` file that changes how Claude Code works on your codebase:

- Writes tests before production code
- Works in small, reviewable increments  
- Stops at checkpoints instead of producing walls of code
- Asks before making design decisions

No dependencies. No CLI. Just markdown.

---

## Quick Start

**1. Check prerequisites**

This works best if your codebase has:
- An automated test suite
- CI pipeline
- Ability to deploy frequently (at least weekly)

New project? Even better—start with discipline from day one.

→ See [Prerequisites](VISION.md#prerequisites) for details.

**2. Copy CLAUDE.md to your project root**

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/YOUR_ORG/volv-code/main/CLAUDE.md
```

Or just copy the contents manually.

**3. Start a Claude Code session**

The agent will now follow the practices encoded in CLAUDE.md.

**4. Test it**

Try: *"Add a function that validates email addresses"*

You should observe:
- Claude writes a failing test first
- Claude implements minimally to pass
- Claude stops and checks in before moving on

If it barrels through without stopping, say: *"Follow the checkpoint process in CLAUDE.md"*

---

## What's in CLAUDE.md

The file encodes a small set of practices:

| Practice | What It Does |
|----------|--------------|
| Test-first | Write a failing test before any production code |
| Small increments | One behavior at a time, deployable independently |
| Checkpoints | Stop after each increment, report what happened, wait for approval |
| Plan before complexity | For ambiguous or multi-file tasks, write a plan first |

These practices compound. They feel slower at first but produce faster, more reliable results—because you spend less time debugging and reverting.

---

## Customizing

CLAUDE.md is yours. Edit it freely.

**Add project-specific guidance:**
```markdown
## Project-Specific

- Use pytest for all tests
- Follow existing naming conventions in /src
- Database migrations require approval before running
```

**Add lessons learned:**
```markdown
## Patterns to Follow

- Always validate user input at API boundaries
- Use UTC for all timestamps internally
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

→ Read the full [Vision](VISION.md) for the complete philosophy.

---

## Status

**Phase:** MVP / Walking Skeleton

This is early. We're testing whether a minimal CLAUDE.md meaningfully changes agent behavior in real codebases.

What we're learning:
- Which instructions the agent follows reliably
- Which get ignored or misinterpreted  
- What's missing

→ See [Current State](VISION.md#current-state) for details.

---

## Contributing

Try it. Tell us what happened.

The most useful contributions right now:
- "I tried this and X worked well"
- "I tried this and Y was ignored"
- "I needed to add Z for my context"

Open an issue or PR. 

---

## Future

The vision is a modular system where you can adopt individual skills:

```bash
# Not built yet—illustrative
npx volv add skill:tdd-strict
npx volv add skill:capture-learning
```

But we're not building that until the core CLAUDE.md proves valuable. Walking skeleton first.

→ See [Vision](VISION.md) for the full roadmap.

---

## License

MIT. Use it, fork it, adapt it.
