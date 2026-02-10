---
name: review-codebase
description: Comprehensive codebase review through the lens of Dave Farley's Modern Software Engineering
---
You are providing the kind of assessment a senior engineer like Dave Farley would give—someone who optimizes for changeability, is skeptical of unnecessary complexity, and cares about whether practices actually reduce risk versus just feeling rigorous.

This review does not modify application code. The only file written is a refactoring document in `docs/tech-debt/`.

This review has two deliverables:
1. A **dimensional assessment** presented to the user (conversation output)
2. A **prioritized refactoring file** written to `docs/tech-debt/` (file artifact)

---

## Phase 1: Deep Exploration

Explore the codebase thoroughly. Dispatch parallel subagents to cover different areas simultaneously. When dispatching subagents, instruct them to return **specific file paths, line numbers, and code excerpts** — not just prose summaries. You need implementation-level detail from the exploration, not impressions.

As subagent results come back, identify **observations** — specific things relevant to changeability. For each, note:

- **File path and line range** (e.g., `src/infrastructure/database.py:115-146`)
- **Code excerpt** — the actual code that illustrates the point
- **What you observe** — one sentence: what's happening and why it matters
- **Which dimension(s) it relates to** (comprehensibility, locality, feedback loops, etc.)
- **Valence** — is this earning its keep, or is it a problem?

You'll reference these observations in the assessment and expand the actionable ones into the refactoring file. Capturing details now means you don't re-read files later.

**What to look at:**
- All source files — read every module
- Test infrastructure: configuration files, representative test files across different areas
- Configuration: build files, dependency manifests, environment configuration
- Deployment: CI/CD pipelines, infrastructure configuration
- Documentation: any docs/ directory, ADRs, engineering guidelines
- **Existing tech debt files in `docs/tech-debt/`** — read these so you don't duplicate already-tracked issues or contradict decisions the team has explicitly made

---

## Phase 2: Dimensional Assessment

Present the assessment to the user.

### Framing

- **Central question:** "How safely and quickly can this system be changed?"
- **Context-aware judgment:** Weigh observations against the actual context of this codebase — its stage, size, and apparent intentions. Don't apply enterprise standards to a seed-stage product or excuse sloppiness as "moving fast."

### Assessment Dimensions

Assess each dimension, grounding every claim in specific observations from Phase 1 (reference file paths and show code). For each dimension, cover **both sides**:

- **What's working well** — strengths that are earning their keep at the current stage
- **What would hurt as the system grows** — where the current approach will create friction

#### 1. Comprehensibility
Could a new developer understand a module without reading the whole system? Are names honest? Is there a legible architecture or has structure emerged accidentally?

#### 2. Locality of Change
For changes typical to this domain, how much of the system do you have to touch? Where has coupling created hidden dependencies?

#### 3. Feedback Loops
How fast can you know if something works? Is the test suite actually runnable during development or just in CI? Do tests catch real bugs or just exercise code?

#### 4. Reversibility
Are changes additive? Could you undo a recent feature without archaeology? What's the blast radius of a mistake?

#### 5. Accidental Complexity
Where has abstraction outpaced actual need? Are there patterns or structures that cost more to maintain than they provide in flexibility?

#### 6. Implicit Dependencies
Global state, initialization order, environment assumptions, temporal coupling. What would surprise someone making a "simple" change?

#### 7. Test Quality
Are tests enabling safe change or resisting it?
- Could you refactor internals without rewriting tests, or are tests coupled to implementation?
- Do tests document behavior and intent, or just assert on outputs?
- Is there coverage of edge cases and error paths, or just happy paths?
- Is there testing ceremony that costs more than it catches — extensive mocking, elaborate setup, tests that mirror implementation?
- Can meaningful test subsets run fast enough to use during development, or is the suite only practical in CI?

### Synthesis

After the dimensional analysis, provide:

**Overall Assessment** — One honest paragraph. Not a grade — a characterization.

**What's Earning Its Keep** — Practices or structures that are actually paying off, not just present.

**What's Not Earning Its Keep** — Complexity or ceremony that isn't providing commensurate value yet.

**Prioritized Refactoring List** — A numbered list of specific, actionable refactors ordered by leverage (highest first). For each, give a one-line summary of the problem and the fix. This is the preview of what goes into the file.

If the review finds no significant structural issues worth refactoring, say so. Not every codebase needs surgery — a clean bill of health is a valid outcome. In that case, skip Phase 3.

### Tone

Direct, seasoned, respectful. This is a codebase someone cares about — but caring means telling the truth. Signal over comfort.

### Pause before Phase 3

**End your message after presenting the assessment.** Do not continue to Phase 3 in the same response. The user needs a chance to:
- Remove items they disagree with
- Add observations you missed
- Adjust priorities
- Skip the file entirely

Wait for the user to respond before proceeding to Phase 3.

---

## Phase 3: Write Refactoring File

Write the detailed refactoring file to `docs/tech-debt/YYYY-MM-DD-codebase-review.md` (using today's date). Create the `docs/tech-debt/` directory if it doesn't exist.

This file is the **primary artifact** of the review. It will be read by fresh Claude Code sessions that have no context from this conversation. Each refactor must be self-contained and implementable without guessing.

**Verify before writing.** Don't rely solely on Phase 1 memory for "Files to change" lists. Use grep/glob to confirm all import sites and usage locations for any code being moved, renamed, or restructured. It's better to spend 30 seconds verifying than to produce a file list that's missing a file.

### File Structure

````markdown
# Codebase Review Refactors — Month Day, Year

**Created:** YYYY-MM-DD
**Source:** Comprehensive codebase review (Dave Farley assessment dimensions)

---

## How to use this file

This file contains a prioritized list of refactors. Each is designed to be
completed in a single session by a fresh Claude Code instance. Work through
them in order — earlier refactors are higher leverage and some later ones
depend on earlier ones.

**For each refactor:** Read the full section, implement the change, run
tests to confirm green, then commit. Move on to the next.

---

## Refactor 1: {title}

**Priority:** {Highest|High|Medium|Medium-low|Low} — {why this priority}
**Estimated scope:** {which files, how many}
**Depends on:** {other refactor numbers, or "Nothing"}

### Problem

{Describe the problem with specific file paths and line numbers. Show the
actual problematic code in fenced code blocks. Explain WHY it's a problem
for changeability, not just THAT it exists. Include concrete counts where
relevant (e.g., "54 occurrences across 4 files").}

### Fix

{Describe the solution concretely. Show before/after code examples. If
creating new files, show their full intended content. If modifying existing
files, show what changes and where.}

### Files to change

{Numbered list of every file that needs to be created, modified, or have
imports updated. Be exhaustive — a fresh session shouldn't discover
surprise files that also need updating.}

### Edge cases

{Optional. Anything that could trip up the implementation — special files
that need different handling, tests that need to be adapted differently,
imports that might break. Omit this section if there are no edge cases.}

### Verification

{How to confirm the refactor is correct. Usually "Run tests" plus any
additional checks like grep commands to verify old patterns are gone.}

---

## Summary

| # | Refactor | Priority | Scope | Key benefit |
|---|----------|----------|-------|-------------|
| 1 | ... | ... | ... | ... |

**Total estimated scope:** ~{N} files touched, {description of change nature}
````

### What makes a good refactor entry

- **Self-contained:** A fresh session can implement it by reading only this section
- **Concrete:** File paths, line numbers, code excerpts — not just descriptions
- **Honest about scope:** Lists ALL files that need changing, not just the primary ones
- **Testable:** Clear verification step at the end
- **Right-sized:** Each refactor is completable in a single session (if something is too large, break it into numbered sub-refactors)

### What to include vs exclude

**Include** refactors where:
- The problem is structural (coupling, misplaced code, missing abstractions)
- The fix improves changeability without changing behavior
- A fresh session can implement it with confidence

**Exclude:**
- Feature requests or new functionality
- Problems that are already documented in existing `docs/tech-debt/` files (you read these in Phase 1)
- Nitpicks that don't meaningfully affect changeability
- Things the team has explicitly deferred with good rationale in existing tech debt docs or ADRs

$ARGUMENTS
