---
name: start
description: Set up a working session with git worktrees
disable-model-invocation: true
allowed-tools: Bash(git *), AskUserQuestion
---

# Session setup

You are helping the user start a working session. Your job is to show them the current state of their repo and worktrees, then help them either start new work in a worktree or stay in the current directory.

## Current state

```
Worktrees:
!`git worktree list`

Current branch:
!`git branch --show-current`

Status:
!`git status --short`

Recent commits on this branch:
!`git log --oneline -5`
```

## Steps

1. **Show the state above clearly.** If there are existing worktrees beyond the main repo, call them out — the user may want to continue work there.

2. **Pull main if on main.** If the current branch is `main`, run `git pull` to ensure it's current. If not on main, skip this.

3. **Ask what the user wants to do:**
   - **Start new work** — create a worktree and branch for a new task
   - **Work here** — stay in this directory on the current branch

4. **If starting new work:**
   - Ask for a short branch name (e.g., `tier3-enrichment`, `add-search`). Suggest one if the user described what they want to do.
   - Derive the repo name from the current directory (e.g., if working in `my-project/`, use `my-project`).
   - Create the worktree: `git worktree add ../<repo-name>-<name> -b <branch-type>/<name>` (use `feature/` prefix by default)
   - Tell the user clearly:
     ```
     Worktree ready. To start working:
       1. Exit this session (Ctrl+C or /exit)
       2. cd ../<repo-name>-<name>
       3. claude
     ```

5. **If working here:** Confirm the branch and that they're ready to go. If on main, note that this is fine for quick tasks (running commands, hotfixes) but parallel feature work should use worktrees.
