---
name: end
description: Wrap up a working session cleanly
disable-model-invocation: true
allowed-tools: Bash(git *), Bash(gh *), AskUserQuestion
---

# Session wrap-up

You are helping the user end a working session. Your job is to make sure nothing is lost and the remote is current.

**Scope: this session only.** All operations target the current worktree and branch. The user may have other Claude Code sessions running on other worktrees — do not touch other branches, worktrees, or directories.

## Current state

```
Branch:
!`git branch --show-current`

Status:
!`git status --short`

Commits ahead of main:
!`git log main..HEAD --oneline 2>/dev/null`

Remote tracking:
!`git rev-parse --abbrev-ref @{upstream} 2>/dev/null || echo "no remote"`

Worktrees:
!`git worktree list`
```

## Steps

1. **Show session state.** Display the branch, any uncommitted changes, and commits ahead of main. Orient the user on where things stand.

2. **Handle uncommitted work.** If there are uncommitted changes (staged or unstaged), ask:
   - **Commit** — review the diff, draft a commit message, and commit
   - **Leave it** — the user wants to keep working later without committing

   If the working tree is clean, skip this step silently.

3. **Push to remote.** If the branch has local commits that aren't pushed, push with `-u` to set up tracking. If already up to date, skip silently. Don't ask — pushing a feature branch is always safe and prevents losing work.

4. **Offer PR creation.** If the branch has commits ahead of main, check for an existing PR with `gh pr list --head <branch-name>`. If no PR exists, ask:
   - **Create PR** — generate a title and body from the branch's commits, then create with `gh pr create`
   - **Skip** — not ready yet

   If a PR already exists, show its URL and skip. If on main, skip this step entirely.

5. **Worktree cleanup.** If working in a worktree (not the main repo directory), ask:
   - **Keep it** — more work to do on this branch
   - **Remove it** — work is merged or done

   If removing, determine the main worktree path from `git worktree list` (the first entry). Tell the user to run these commands after exiting:
   ```
   cd <main-worktree-path>
   git worktree remove <current-worktree-path>
   ```
   You cannot remove a worktree from inside it. Do NOT attempt to run the removal commands yourself.

   If on the main repo directory, skip this step.

6. **Session summary.** Print a short recap:
   - What was committed and pushed (if anything)
   - Whether a PR was created or already exists
