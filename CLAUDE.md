# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About This Project

Volv is a Claude Code plugin that provides disciplined workflow skills — session lifecycle, code review, and test-driven development practices. The deliverables are markdown files: skill definitions and a standalone CLAUDE.md template.

**Important distinction:**
- `skills/` - Plugin skills installed via Claude Code's plugin system
- `templates/CLAUDE.md` - Standalone template users copy into their projects
- This file (`CLAUDE.md` at root) - Instructions for working on Volv itself

Do not confuse these. When editing skills or the template, you're changing what users will install or copy. When following this file, you're working on Volv as a project.

## Project Structure

```
.claude-plugin/
  plugin.json         # Plugin manifest
skills/               # Plugin skills
  start/SKILL.md      # Session setup with git worktrees
  end/SKILL.md        # Session wrap-up
  review-change/SKILL.md  # Code change review
  review-plan/SKILL.md    # Plan review
  review-codebase/SKILL.md # Full codebase review
templates/
  CLAUDE.md           # Standalone template (for non-plugin use)
VISION.md             # Full philosophy and design principles
README.md             # User-facing documentation
CONTRIBUTING.md       # Contribution guidelines
```

## Development Workflow

This project follows its own principles from VISION.md:

1. **Small increments** - Each change should be describable in one sentence
2. **Checkpoints** - Stop after each change, report what you did, wait for approval
3. **Plan before complexity** - For multi-file or ambiguous changes, outline a plan first

Since this is a documentation project, "tests" means validating that:
- Markdown renders correctly
- Links work
- Instructions are clear and unambiguous
- Examples are accurate
- Plugin structure validates with `claude plugin validate .`

## Current Phase

Plugin MVP. The core CLAUDE.md template has been validated. Focus is now on packaging battle-tested skills as a proper Claude Code plugin for broader distribution.
