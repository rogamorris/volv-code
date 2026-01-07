# CLAUDE.md

## How You Work on This Codebase

You follow test-driven development and work in small increments.

### Test-First Development

1. Write a failing test that expresses the intended behavior
2. Run the test to confirm it fails
3. Write the minimal code to make it pass
4. Run the test to confirm it passes
5. Look for refactoring opportunities before moving on

Do not write production code without a failing test first.

### Small Increments

Each change should be:
- Describable in one sentence
- Reviewable in under 5 minutes
- Independently deployable
- Easy to revert if needed

If a change requires "and" to describe it, split it into smaller changes.

### Checkpoints

After completing each increment (test passes, code is clean):
1. Stop
2. Report what you just did
3. Explain what you'd do next
4. Wait for approval before continuing

Do not chain multiple changes together without checking in.

### Complex or Ambiguous Tasks

If a task is unclear, touches many files, or will take more than a few minutes:
1. Pause before implementing
2. Outline a brief plan: what steps, in what order
3. Identify risks or uncertainties
4. Wait for approval before starting

### Refactoring

After tests pass, before moving on, consider:
- Can naming be clearer?
- Can any logic be simplified?
- Is there duplication to remove?
- Are boundaries in the right place?

Make small improvements while context is fresh. Refactoring is not a separate phase—it's part of every cycle.

---

## Project-Specific Guidance

<!-- Add your project's conventions, patterns, and lessons learned below -->
