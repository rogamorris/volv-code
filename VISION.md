# Volv: Vision

*Last updated: 2025-01-07*

---

## The Problem

Claude Code is powerful but undisciplined by default. Without guidance, it:

- Writes hundreds of lines in a single response
- Makes design decisions mid-stream without asking
- Skips tests when it seems convenient
- Loses context between sessions
- Repeats the same mistakes across projects

Teams using Claude Code often get inconsistent results. What worked yesterday might not work today. Hard-won lessons disappear when the session ends.

**The deeper problem:** There's no mechanism for Claude Code to get better at working on *your* codebase over time.

---

## Prerequisites

This workflow assumes certain capabilities are in place. Without them, the principles don't work—or worse, become ritual without benefit.

### For Existing Codebases

**You need:**

| Capability | Why It Matters |
|------------|----------------|
| Automated test suite | Can't do TDD without a test framework. Can't refactor safely without tests. |
| CI pipeline | Tests must run automatically. Manual test runs don't scale. |
| Ability to deploy frequently | "Small deployable increments" requires actually deploying. If deploys are monthly, this workflow doesn't fit. |
| Deployment takes < 30 minutes | If deploys take hours, the feedback loop is too slow. |
| Rollback mechanism | Small increments are only safe if you can undo them quickly. |

**You probably need:**

| Capability | Why It Matters |
|------------|----------------|
| Feature flags or toggles | Deploy incomplete work safely. Decouple deploy from release. |
| Basic observability | Know if something broke in production. Close the feedback loop. |
| Team buy-in | One person can't adopt this unilaterally if the team ships large batches. |

**If you don't have these:** Fix that first. The plugin won't help a codebase that can't deploy safely. The walking skeleton approach (Principle 1) is specifically designed to establish this infrastructure early.

### For New Codebases

This workflow is ideal for greenfield projects. Start with:

1. A walking skeleton that touches all boundaries
2. CI/CD from day one (even if deploying to staging)
3. Test framework wired up before writing production code

The first commit should be a deployable skeleton, not application code. This establishes the infrastructure as non-negotiable.

### For Legacy Codebases Without CI/CD

This plugin is not the right starting point. Instead:

1. First, establish a deployment pipeline (even a minimal one)
2. Add test coverage incrementally (characterization tests)
3. Get to a place where you can deploy a small change safely
4. Then adopt these practices

There's no shame in not being ready. The goal is to get there eventually, not to pretend you're already there.

---

## The Bet

This plugin is built on a specific engineering philosophy. If you don't buy these premises, the plugin won't make sense.

### The Core Premise

**Build systems that can change safely by working in small steps, validating continuously, and using feedback to guide architecture and design.**

Everything else serves that goal. Speed comes from confidence. Confidence comes from small, validated steps. This feels slower but is faster—because you spend less time debugging, reverting, and untangling.

### The Principles

#### 1. Start with a Walking Skeleton

Prove the system works end-to-end before building depth.

- Implement the smallest possible vertical slice
- Touch every architectural boundary (UI → domain → persistence → external)
- Use stubs and fakes where necessary
- Automate build, test, and deploy from day one

**Why it matters:** Surfaces architectural mistakes early. Prevents speculative abstractions. Establishes the deployment pipeline as infrastructure, not afterthought.

#### 2. Work in Very Small Increments

Keep risk and reasoning scope low.

Each change should be:
- Small (describable in one sentence)
- Reviewable (understandable in minutes)
- Reversible (easy to undo)
- Independently deployable (doesn't require other changes)

Prefer vertical slices over horizontal layers. If you can't deploy it today, it's too big.

#### 3. Use TDD as a Design Tool

TDD is not about tests—it's about controlling complexity through design pressure.

**Red:** Write a failing test that expresses intent. This forces clarity on behavior and API shape before implementation.

**Green:** Make it pass in the simplest way possible. Resist premature generalization.

**Refactor:** Improve structure without changing behavior. Extract functions, clarify names, reduce duplication, strengthen boundaries.

The red-green-refactor cycle shapes code that is simple, testable, and adaptable.

#### 4. Refactor Continuously, Not Periodically

Refactoring is not a phase. It happens:
- After tests pass
- While context is fresh
- In small, safe steps

Focus on: improving cohesion, reducing coupling, strengthening information hiding, making change more local.

**Architecture emerges from repeated, disciplined refactoring—not from upfront design.**

#### 5. Let Domain Understanding Drive Structure

Align code with how the problem domain actually works.

- Model the domain in names, APIs, and boundaries
- Expect the model to change as understanding improves
- Refactor aggressively when you learn something new

This is DDD as a learning tool, not a ceremony. No heavy upfront modeling. No dogmatic patterns. Focus on clarity and changeability.

#### 6. Keep a Strict Deployment Pipeline

Turn uncertainty into fast feedback.

A good pipeline includes:
- Automated tests (fast → slow)
- Static analysis
- Security checks
- One-click (or automatic) deploys
- Fast rollback

**If it's painful to deploy, you'll stop doing it—and complexity will grow unchecked.**

#### 7. Use Production as a Learning Environment

Close the loop.

- Monitor behavior in production
- Track metrics that reflect outcomes: lead time, deployment frequency, change failure rate, mean time to restore
- Feed learnings back into design, architecture, tests, and domain model

Production is where hypotheses get validated.

#### 8. Prefer Evolutionary Architecture Over Control

Accept uncertainty. Make decisions that are reversible and cheap to change.

- Avoid "big design up front"
- Optimize for fast feedback, low blast radius, safe experimentation
- When unsure, choose the option that's easiest to change later

### The Loop

These principles form a continuous cycle:

```
Hypothesis (small behavior)
    ↓
Test (TDD: write failing test)
    ↓
Implement (minimal code to pass)
    ↓
Refactor (improve while green)
    ↓
Integrate (continuously)
    ↓
Deploy (to production)
    ↓
Observe (metrics, behavior)
    ↓
Learn (capture insights)
    ↓
Repeat
```

Each loop should:
- Improve the product, AND
- Improve the system's ability to change

### What This Means for the Plugin

We believe:

1. **Small, explicit guidance beats large, implicit hope.** A 30-line CLAUDE.md that encodes these practices will outperform hoping the agent "figures it out."

2. **Discipline compounds.** Test-first, small increments, and human checkpoints feel slower but produce faster, more reliable results—because you spend less time fixing mistakes.

3. **The system should learn.** Mistakes captured and synthesized into improved guidance create a flywheel. Each project gets smarter over time.

4. **Less is more.** A modular system where you adopt only what you need will see broader adoption than a comprehensive framework.

5. **We should follow our own principles.** This plugin will be built using walking skeletons, small increments, and continuous learning—not big upfront design.

---

## What Success Looks Like

### For an Individual Developer

> "I dropped a single file into my repo and Claude Code immediately started behaving more predictably. It writes tests first, stops to check in with me, and doesn't go off on tangents."

**Observable indicators:**
- Agent writes failing test before production code (without being asked)
- Agent stops at natural checkpoints instead of producing walls of code
- Agent asks clarifying questions for ambiguous tasks instead of guessing
- Changes are small enough to review in a few minutes

### For a Team

> "We have a shared CLAUDE.md that encodes how we work. New team members (human or AI) can read it and understand our practices. It evolves as we learn."

**Observable indicators:**
- CLAUDE.md captures team-specific patterns and lessons
- Mistakes get documented and turned into guidance
- Guidance improves measurably over weeks/months
- Less time spent correcting agent behavior, more time on actual work

### For the Ecosystem

> "I found a plugin that let me start with just the basics and add more as I needed it. I wasn't forced to adopt someone else's entire workflow—I composed my own."

**Observable indicators:**
- Adoption starts with copy-paste (no install required)
- Users add skills incrementally based on actual need
- Users modify and extend the system for their context
- Community contributes skills that others find useful

---

## Design Principles

### 1. Value at Every Layer

The README alone should teach useful practices. The minimal CLAUDE.md should change behavior. Each skill should provide standalone value. No "you need to adopt everything for this to work."

### 2. You Own It

Everything is markdown in your repo. No runtime dependencies. No external services. Fork it, edit it, delete parts you don't like. We deliver text, not software.

### 3. Small Pieces, Loosely Joined

Skills are independent. Workflows are compositions of skills. Nothing requires anything else. If you only want TDD discipline, take only that.

### 4. The System Learns

Capturing mistakes and synthesizing them into guidance is a first-class concern, not an afterthought. A static CLAUDE.md is a decaying asset. An evolving one compounds.

### 5. Opinionated but Not Dictatorial

We have strong opinions (test-first, small increments, human checkpoints). But these are defaults, not mandates. The user can override, modify, or ignore.

---

## What We're NOT Building

- **A framework with runtime dependencies.** It's just markdown.
- **A comprehensive solution.** It's a toolkit, not a monolith.
- **An AI agent.** We shape behavior through instructions, not code.
- **A proprietary system.** Everything is open source and forkable.
- **Something that requires buy-in.** Works for a solo dev; scales to teams.

---

## Boundaries

### In Scope

- CLAUDE.md templates and guidance
- Skill library (modular practices)
- Working file templates (PLAN.md, LEARNINGS.md, etc.)
- CLI for convenience (optional)
- Documentation and examples

### Out of Scope (For Now)

- IDE integrations
- Automated metrics/analytics
- Multi-agent orchestration
- Paid features or services
- Anything that requires infrastructure

---

## How We'll Develop This

We'll follow our own principles:

1. **Walking skeleton first.** Minimal CLAUDE.md that proves the concept in a real codebase.

2. **Small increments.** Each addition should be usable and valuable on its own.

3. **Test on real work.** Validate in production codebases, not toy examples.

4. **Capture learnings.** What works? What doesn't? What did we get wrong?

5. **Evolve the vision.** This document should change as we learn.

---

## Current State

**Phase:** Walking Skeleton

**Focus:** Validate that a minimal CLAUDE.md changes agent behavior in a meaningful way.

**Next milestone:** Test in telecom BSS codebase with 12 contributors. Observe whether:
- Agent follows TDD discipline
- Agent stops at checkpoints
- Increment size feels appropriate
- Any instructions get ignored or misinterpreted

**Success criteria for this milestone:** At least one team member says "this made Claude easier to work with" AND we have a concrete list of what to improve.

---

## Open Questions

1. **What's the right granularity for skills?** Too fine = overhead. Too coarse = back to monolith.

2. **How do we handle existing CLAUDE.md files?** Merge? Replace? Coexist?

3. **Will the agent reliably stop at checkpoints?** This is the key behavior—if it doesn't work, much of the value disappears.

4. **How much guidance is too much?** Token limits and attention are real constraints.

5. **What's the learning capture UX?** If it's too much friction, people won't do it.

---

## Revision History

| Date | Change |
|------|--------|
| 2025-01-07 | Initial vision document |

---

*This document is a test. If we can't articulate what success looks like, we can't know if we're building the right thing.*
