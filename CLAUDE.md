# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About This Project

Volv is a documentation-only project that provides CLAUDE.md templates for teams using Claude Code. There is no application code, build system, or test suite—the deliverables are markdown files.

**Important distinction:**
- `templates/CLAUDE.md` - The template users copy into their projects
- This file (`CLAUDE.md` at root) - Instructions for working on Volv itself

Do not confuse these. When editing the template, you're changing what users will copy. When following this file, you're working on Volv as a project.

## Project Structure

```
templates/          # Distributable templates users copy
  CLAUDE.md         # Core template
VISION.md           # Full philosophy and design principles
README.md           # User-facing documentation
CONTRIBUTING.md     # Contribution guidelines
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

## Current Phase

Walking Skeleton / MVP. Focus is on validating that the core `templates/CLAUDE.md` meaningfully changes agent behavior before building additional features.
