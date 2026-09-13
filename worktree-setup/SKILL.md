---
name: worktree-setup
description: >-
  Use this skill whenever the user asks to create, set up, switch to, or work
  in an isolated Git worktree for a task. Apply it even when the user does not
  name this skill. Create the worktree under `.worktrees/`, preserve the
  current workspace, detect dependencies, and guide environment setup.
---

# Worktree Setup

Use this workflow when creating a new Git worktree for an isolated task within the repository.

## Before Creating the Worktree

- State the source ref, new branch name, and target directory (defaulting to `.worktrees/<branch-name>` inside the project root).
- Ensure `.worktrees/` is ignored (e.g. in `.gitignore` or local exclude) so auxiliary worktree folders are not tracked.
- Check existing worktrees with `git worktree list` and ensure the target path does not already exist.
- Preserve the current worktree and its uncommitted changes. Do not reset, clean, or alter it.

## Setup Workflow

1. **Create Worktree and Branch**:
   - Create the worktree under `.worktrees/<branch-name>` from the requested source ref:
     ```bash
     git worktree add .worktrees/<branch-name> -b <branch-name> <source-ref>
     ```
   - If no source is specified, fetch and use the repository's default remote branch (`origin/main` or `origin/master`).

2. **Detect Package Manager & Install Dependencies**:
   - Inspect the root repository's lockfiles to identify the package manager:
     - `pnpm-lock.yaml` ➔ run `pnpm install` in the new worktree
     - `yarn.lock` ➔ run `yarn install` in the new worktree
     - `bun.lock` / `bun.lockb` ➔ run `bun install` in the new worktree
     - `package-lock.json` (or default) ➔ run `npm install` in the new worktree
   - *Optionally for identical dependency trees*: Symlink `node_modules` (`ln -s ../../node_modules .worktrees/<branch-name>/node_modules`) to avoid re-downloading dependencies and save disk space.
   - If the project has no `package.json`, skip this step and explain why.

3. **Inspect Environment Templates**:
   - Inspect the repository for environment templates, such as `.env.example`, `env-example`, `env-example-relational`, or `env-example-document`.

4. **Environment Setup Reminder**:
   - Tell the user which template is appropriate and remind them to create/configure `.worktrees/<branch-name>/.env` with their local or secret values.
   - Do not copy a template or write secrets unless the user explicitly requests it.

5. **Completion Report**:
   - Report the new worktree path, branch, package manager/dependency-install outcome, and environment setup reminder.

## Failure Handling

- If dependency installation fails, keep the worktree intact and report the command failure. Do not remove it without user approval.
- If the target path or branch already exists, stop and report the conflict instead of overwriting it.
