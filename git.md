# Git Commands

## Basic Terms

### Staging

**Staging** is a temporary area where changes are placed before creating a commit.

**Command:** `git add`

### Commit

A **commit** is a saved snapshot of the staged changes in the local Git repository.

**Command:** `git commit`

### Merge

**Merge** combines the changes from two branches into one branch.

**Command:** `git merge`

---

# 1. Initialize / Start Tracking

## `git init`

Initializes a new Git repository in the current directory and allows Git to start tracking changes.

```bash
git init
```

---

# 2. Staging

## `git add --all` / `git add -A`

Stages **all changes** in the entire repository, including:

- New files
- Modified files
- Deleted files

These changes are then ready to be committed.

```bash
git add --all
```

or

```bash
git add -A
```

---

## `git add .`

Stages changes in the **current directory and its subdirectories**.

It stages:

- New files
- Modified files
- Deleted files

```bash
git add .
```

> **Note:** The scope depends on the directory from which the command is executed.

---

## `git add *`

Stages files matched by the shell's `*` wildcard.

It generally stages **new and modified files**, but it does **not reliably include deleted files**, because shell expansion does not include filenames that no longer exist.

```bash
git add *
```

> **Recommended:** Use `git add .` or `git add -A` when you want predictable staging behavior.

---

# 3. Remove Changes from Staging

## `git reset`

Removes staged changes from the staging area and moves them back to the working directory.

It **does not delete the actual changes**.

```bash
git reset
```

### Example

If you accidentally run:

```bash
git add .
```

you can use:

```bash
git reset
```

to unstage the changes.

---

## `git reset HEAD~`

Moves `HEAD` back by one commit.

In the common default mode, it:

- Undoes the last commit
- Keeps the changes in the working directory
- Unstages those changes

```bash
git reset HEAD~
```

> **Note:** This command is normally used after a commit when you want to undo the latest commit while keeping its changes.

---

## `git reset --hard`

Resets the working directory, staging area, and `HEAD` to the specified state.

```bash
git reset --hard
```

For example:

```bash
git reset --hard HEAD
```

This discards **unstaged and staged changes** that are not committed.

> ⚠️ **Warning:** This can permanently remove uncommitted changes. Use it carefully.

---

# 4. Deleting Files Using `git rm`

## `git rm file_name`

Deletes the specified file from the working directory and stages the deletion.

```bash
git rm file_name
```

This performs two operations together:

1. Deletes the file
2. Stages the deletion

---

## `git rm -f file_name`

Forcefully removes the file and stages the deletion.

```bash
git rm -f file_name
```

This is useful when the file has modifications that have not been staged.

> ⚠️ **Warning:** The uncommitted changes in the file can be lost.

---

## `git rm --cached file_name`

Removes the file from Git's tracking/staging while **keeping the physical file in the working directory**.

```bash
git rm --cached file_name
```

A common use is when a file was accidentally added to Git but should not be tracked.

For example:

```bash
git rm --cached .env
```

The `.env` file remains on your computer but is removed from Git tracking.

> **Tip:** Add the file to `.gitignore` if you do not want Git to track it again.

---

## `git rm -r folder_name`

Recursively removes a directory and the files/subdirectories inside it.

```bash
git rm -r folder_name
```

Here, `-r` means **recursive**.

> **Note:** This is for removing a directory/folder, not a Git repository itself.

---

# 5. View Commits

## `git log`

Displays the commit history of the current branch.

```bash
git log
```

It shows information such as:

- Commit ID
- Author
- Date
- Commit message

---

## `git log --oneline`

Displays the commit history in a short and compact format.

```bash
git log --oneline
```

Example:

```text
a82f31c Add login feature
b51d902 Fix database connection
91ac721 Initial commit
```

This is useful for quickly finding commit IDs.

---

# 6. Move Between Commits

## `git switch --detach commit_id`

Moves to a specific commit so you can inspect the project at that point in its history.

First, identify the required commit using:

```bash
git log --oneline
```

Then:

```bash
git switch --detach commit_id
```

Example:

```bash
git switch --detach a82f31c
```

> **Note:** When you move directly to a commit instead of a branch, Git places you in a **detached HEAD** state.

---

## `git checkout commit_id`

Older Git syntax for moving to a specific commit.

```bash
git checkout commit_id
```

> **Note:** `git switch` is the modern command for switching branches, while `git checkout` is still supported and can perform several different operations.

---

## `git diff commit_id1 commit_id2`

Shows the differences between two commits.

```bash
git diff commit_id1 commit_id2
```

Example:

```bash
git diff a82f31c b51d902
```

---

# 7. Branching

## `git branch`

Displays the branches in the repository.

```bash
git branch
```

The current branch is marked with `*`.

---

## `git branch branch_name`

Creates a new branch.

```bash
git branch branch_name
```

Example:

```bash
git branch development
```

> **Note:** This creates the branch but does **not** switch to it.

---

## `git checkout branch_name`

Switches to another branch.

```bash
git checkout branch_name
```

Example:

```bash
git checkout development
```

---

## `git switch branch_name`

Modern command for switching to another branch.

```bash
git switch branch_name
```

Example:

```bash
git switch development
```

---

# 8. Merge

## `git merge branch_name`

Combines the specified branch into the **currently checked-out branch**.

```bash
git merge branch_name
```

### Example

Suppose you have:

```text
main
development
```

If you want to merge `development` into `main`:

```bash
git switch main
git merge development
```

This means:

> Take the changes from `development` and merge them into `main`.

---

## Merge with a Commit Message

You may see:

```bash
git merge branch_name -m "Message"
```

Example:

```bash
git merge development -m "Merge development into main"
```

However, Git normally generates an appropriate merge commit message automatically when a merge commit is required.

---

## Merge Conflict

A **merge conflict** happens when Git cannot automatically combine changes from different branches.

For example, if two branches modify the **same part of the same file differently**, Git may not know which version should be kept.

The developer must manually resolve the conflict and then commit the result.

---

# 9. Push, Fetch and Pull

## `git push`

Sends local commits to a remote repository.

```bash
git push
```

Example:

```text
Local repository → Remote repository
```

---

## `git fetch`

Downloads the latest changes and information from the remote repository **without merging them into your current branch**.

```bash
git fetch
```

After fetching, you can inspect the changes and then merge them if required.

```bash
git merge
```

> **Simple idea:**
> `git fetch` = Download remote changes, but don't merge them.

---

## `git pull`

Downloads changes from the remote repository and integrates them into the current local branch.

```bash
git pull
```

In the common case:

```text
git pull = git fetch + git merge
```

> **Note:** Depending on your Git configuration, `git pull` can also use rebase instead of merge.

---

## `git push origin main`

Pushes the local `main` branch to the remote repository named `origin`.

```bash
git push origin main
```

Here:

- `origin` = name of the remote repository
- `main` = branch being pushed

---

## `git push origin branch_name`

Pushes another branch to the remote repository.

```bash
git push origin branch_name
```

Example:

```bash
git push origin development
```

---

# 10. Git Restore

## `git restore file_name`

Restores a file in the working directory to its version from the latest commit.

```bash
git restore file_name
```

### Example scenario

Suppose you are developing a feature and make changes to a file. Later, you realize that the changes are not useful and want to discard them.

You can use:

```bash
git restore file_name
```

> ⚠️ **Warning:** This discards the file's uncommitted working-directory changes.

---

## `git restore --staged file_name`

Removes the file from the staging area while **keeping your actual changes in the working directory**.

```bash
git restore --staged file_name
```

Example:

```bash
git add file_name
git restore --staged file_name
```

The file becomes **unstaged**, but your modifications are not deleted.

---

## `git restore --staged .`

Unstages all currently staged files.

```bash
git restore --staged .
```

> **Important:** `git restore --staged` does **not** undo your actual file modifications. It only removes them from staging.

---

# 11. Git Stash

## What is Git Stash?

**Git stash** temporarily stores your uncommitted changes so that you can work on something else without committing unfinished work.

Think of it as a **temporary stack of unfinished changes**.

---

## `git stash`

Temporarily saves your current working changes.

```bash
git stash
```

By default, it stashes:

- Modified tracked files
- Staged changes

It does not normally stash untracked files unless you use an appropriate option such as:

```bash
git stash -u
```

---

### Example Scenario

Suppose you are working on a feature in the `development` branch.

Your work is incomplete, so you do not want to commit it yet.

Another developer asks you to switch branches and check another feature.

You can temporarily save your work:

```bash
git stash
```

Then switch branches:

```bash
git switch another-branch
```

After finishing the other work, you can return and restore your previous changes.

---

## `git stash pop`

Restores the most recently stashed changes and removes that stash from the stash list.

```bash
git stash pop
```

> **Simple idea:**
> `stash` = save temporarily
> `stash pop` = restore and remove from stash list

---

## `git stash list`

Shows all saved stashes.

```bash
git stash list
```

Example:

```text
stash@{0}: WIP on development
stash@{1}: WIP on main
stash@{2}: WIP on feature-login
```

This is useful when you have used `git stash` multiple times.

---

## `git stash apply`

Restores the stashed changes **without removing the stash from the stash list**.

```bash
git stash apply
```

This is useful when you want to restore the changes but also keep the stash for future use.

You can also apply a specific stash:

```bash
git stash apply stash@{1}
```

---

## `git stash drop`

Deletes a stash from the stash list.

```bash
git stash drop
```

Example:

```bash
git stash drop stash@{0}
```

> **Important:** `git stash drop` removes the stash. It does not mean "restore the saved changes."

To remove all stashes:

```bash
git stash clear
```

> ⚠️ **Warning:** `git stash clear` removes all stashes.

---

# 12. Git Revert

## `git revert commit_id`

Creates a **new commit that reverses the changes introduced by an earlier commit**.

```bash
git revert commit_id
```

It does **not delete the old commit** from the history.

### Example

Suppose the history is:

```text
A → B → C
```

If commit `C` introduced a broken feature:

```bash
git revert C
```

Git creates a new commit:

```text
A → B → C → C'
```

Here, `C'` reverses the changes made by `C`.

---

### Why Use `git revert`?

Suppose a developer commits a feature, but later discovers that the feature is broken.

Instead of deleting the commit from history, the developer can use:

```bash
git revert commit_id
```

This keeps the original commit in the history while creating a new commit that fixes/reverses its changes.

This is generally safer for **shared or collaborative branches**.

---

## `git reset` vs `git revert`

| `git reset`                                        | `git revert`                        |
| -------------------------------------------------- | ----------------------------------- |
| Moves the branch pointer to another commit         | Creates a new commit                |
| Can rewrite history                                | Preserves existing history          |
| Can remove commits from the visible branch history | Does not remove the original commit |
| Useful for local/private work                      | Safer for shared branches           |

> **Simple idea:**
> **Reset = move/remove history from the branch**
> **Revert = keep history and create a new reversing commit**

---

# 13. Git Rebase

## `git rebase`

Reapplies commits from one branch on top of another base commit.

It is commonly used to create a **cleaner, more linear commit history**.

Example:

```bash
git switch development
git rebase main
```

This takes the commits from `development` and reapplies them on top of the latest `main`.

### Before rebase

```text
A---B---C  main
     \
      D---E  development
```

### After rebase

```text
A---B---C---D'---E'  development
```

The commits `D` and `E` are recreated as `D'` and `E'`.

---

## Important Note About Rebase

Rebase **rewrites commit history**.

Therefore, avoid rebasing commits that have already been shared with other developers unless the team has agreed on it.

For example, if multiple developers are working on the same remote branch and one developer rebases and force-pushes it, the other developers' local histories may no longer match the remote history.

This can cause synchronization problems and require additional Git operations to recover.

### Simple Rule

```text
Personal/local work → Rebase can be useful
Shared/public history → Prefer merge unless the team agrees otherwise
```

---

# 14. Git Diff

## `git diff`

Shows the changes in the working directory that have **not yet been staged**.

```bash
git diff
```

It is useful for checking what you changed before running:

```bash
git add .
```

---

## `git diff --staged`

Shows the changes that are currently **staged** and are ready to be committed.

```bash
git diff --staged
```

This is useful for checking your changes before:

```bash
git commit
```

---

# Quick Git Workflow

A common basic Git workflow is:

```text
Working Directory
       ↓
   git add
       ↓
Staging Area
       ↓
  git commit
       ↓
Local Repository
       ↓
   git push
       ↓
Remote Repository
```

### Example

```bash
git init
git add .
git commit -m "Initial commit"
git push origin main
```

---

# Most Important Commands at a Glance

| Command                     | Purpose                                         |
| --------------------------- | ----------------------------------------------- |
| `git init`                  | Initialize a Git repository                     |
| `git add .`                 | Stage changes                                   |
| `git add -A`                | Stage all changes                               |
| `git commit -m "message"`   | Save staged changes as a commit                 |
| `git status`                | Check current Git status                        |
| `git log`                   | View commit history                             |
| `git log --oneline`         | View compact commit history                     |
| `git branch`                | View branches                                   |
| `git branch name`           | Create a branch                                 |
| `git switch name`           | Switch branches                                 |
| `git merge name`            | Merge a branch                                  |
| `git diff`                  | View unstaged changes                           |
| `git diff --staged`         | View staged changes                             |
| `git push`                  | Send local commits to remote                    |
| `git fetch`                 | Download remote changes without merging         |
| `git pull`                  | Fetch and integrate remote changes              |
| `git restore file`          | Discard working-directory changes               |
| `git restore --staged file` | Unstage a file                                  |
| `git stash`                 | Temporarily save uncommitted changes            |
| `git stash pop`             | Restore latest stash and remove it              |
| `git stash list`            | View stashes                                    |
| `git stash apply`           | Restore stash but keep it                       |
| `git stash drop`            | Delete a stash                                  |
| `git revert ID`             | Create a commit that reverses an earlier commit |
| `git reset`                 | Move `HEAD` and/or unstage changes              |
| `git rebase`                | Reapply commits onto another base               |

# Easy Way to Remember

```text
git add       → Put changes into staging
git commit    → Save changes locally
git push      → Send commits to remote
git fetch     → Download remote changes
git pull      → Download + integrate remote changes

git restore   → Discard/unstage changes
git stash     → Temporarily hide unfinished changes

git revert    → Undo a commit safely by creating a new commit
git reset     → Move HEAD / undo local history

git branch    → Create/view branches
git switch    → Move between branches
git merge     → Combine branches
git rebase    → Reapply commits on a new base

git diff      → See changes
git log       → See commit history
```
