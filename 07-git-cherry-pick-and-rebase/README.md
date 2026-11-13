# Git Cherry-pick and Rebase

Cherry-pick and rebase are advanced Git operations used to manage commit history and apply changes between branches.

Cherry-pick allows developers to apply specific commits to another branch, while rebase moves or replays commits onto a different base commit.

This guide explains both operations through practical examples, command explanations, commit-history diagrams, and exercises.

## Table of Contents

1. [Introduction](#1-introduction)
2. [Understanding Git Cherry-pick](#2-understanding-git-cherry-pick)
3. [Cherry-pick Commands](#3-cherry-pick-commands)
4. [Handling Cherry-pick Conflicts](#4-handling-cherry-pick-conflicts)
5. [Understanding Git Rebase](#5-understanding-git-rebase)
6. [Basic Rebase Workflow](#6-basic-rebase-workflow)
7. [Interactive Rebase](#7-interactive-rebase)
8. [Handling Rebase Conflicts](#8-handling-rebase-conflicts)
9. [Merge vs Rebase vs Cherry-pick](#9-merge-vs-rebase-vs-cherry-pick)
10. [Common Commands](#10-common-commands)
11. [Best Practices](#11-best-practices)
12. [Practice Exercises](#12-practice-exercises)

---

## 1. Introduction

Git provides multiple ways to manage changes and commit history across branches.

Three important operations are:

- **Merge:** Combines the histories of two branches.
- **Cherry-pick:** Applies the changes introduced by a specific commit to the current branch.
- **Rebase:** Replays commits onto another base commit.

These operations help developers manage feature development, maintain clean histories, and work with different branches.

## 2. Understanding Git Cherry-pick

Cherry-pick is used to apply the changes introduced by a particular commit to the current branch without merging the entire source branch.

### 2.1 Basic Syntax

```bash
git cherry-pick <commit-hash>
```

Here, `<commit-hash>` represents the commit whose changes you want to apply.

### 2.2 Example

Suppose two branches exist:

```text
main
 |
 A---B

feature
 |
 A---B---C---D
```

Commit `C` contains a useful change that is needed in `main`.

First, switch to the target branch:

```bash
git switch main
```

Apply the selected commit:

```bash
git cherry-pick C
```

Result:

```text
main
 |
 A---B---C'
```

The changes from commit `C` are applied to `main` as a new commit, represented by `C'`.

**Important:** Cherry-picking creates a new commit with a different commit hash because it has a different parent history.

### 2.3 When to Use Cherry-pick

- Applying a specific bug fix to another branch.
- Moving an individual feature or improvement between branches.
- Selecting only required commits instead of merging an entire branch.
- Backporting fixes to maintenance or release branches.

## 3. Cherry-pick Commands

| Command | Purpose |
|---|---|
| `git cherry-pick <hash>` | Apply a specific commit |
| `git cherry-pick --no-commit <hash>` | Apply changes without creating a commit automatically |
| `git cherry-pick --continue` | Continue after resolving conflicts |
| `git cherry-pick --abort` | Cancel the current cherry-pick |
| `git cherry-pick --skip` | Skip the current commit during a sequence |

### 3.1 Cherry-pick Without Automatic Commit

```bash
git cherry-pick --no-commit <commit-hash>
```

This applies the selected commit's changes to the working tree and staging area without automatically creating a new commit.

You can inspect the changes and create your own commit:

```bash
git status
git diff --cached
git commit -m "Apply selected changes"
```

## 4. Handling Cherry-pick Conflicts

A cherry-pick conflict occurs when Git cannot automatically apply a commit because its changes conflict with the current branch.

### 4.1 Conflict Resolution Workflow

1. Start the cherry-pick operation.
2. Git reports a conflict.
3. Check the repository status.
4. Open the conflicted file.
5. Resolve the conflicting content.
6. Stage the resolved file.
7. Continue the cherry-pick.

Check the status:

```bash
git status
```

After resolving the file:

```bash
git add <file-name>
git cherry-pick --continue
```

To cancel the operation:

```bash
git cherry-pick --abort
```

### 4.2 Understanding Conflict Markers

Git may display conflict markers like these:

```text
 <<<<<<< HEAD
Current branch version
 =======
Incoming commit version
 >>>>>>> commit-hash
```

- `<<<<<<< HEAD` marks the current branch's content.
- `=======` separates the conflicting versions.
- `>>>>>>> commit-hash` marks the end of the conflict section and identifies the incoming commit.

Edit the file to keep the correct content and remove all conflict markers before continuing.

**Important:** Always review the final file before staging it.

## 5. Understanding Git Rebase

Rebase is used to move or replay commits from one branch onto another base commit.

It helps maintain a cleaner and more linear commit history.

### 5.1 Basic Syntax

```bash
git rebase <branch-name>
```

### 5.2 Example

Suppose the repository has the following history:

```text
main:
A---B---C

feature:
     \
      D---E
```

The main branch receives a new commit `C`.

Rebasing the feature branch onto main:

```bash
git switch feature
git rebase main
```

Result:

```text
main:
A---B---C

feature:
         \
          D'---E'
```

Git replays the feature commits on top of the latest main commit.

The resulting commits have new hashes because their parent history has changed.

### 5.3 Why Rebase Is Used

- Keeping feature branches updated with the latest base branch.
- Maintaining a linear commit history.
- Organizing commits before merging.
- Cleaning up local development history.

## 6. Basic Rebase Workflow

### Step 1: Switch to the feature branch

```bash
git switch feature
```

### Step 2: Rebase onto the target branch

```bash
git rebase main
```

### Step 3: Verify the commit history

```bash
git log --oneline --graph --all
```

### Step 4: Resolve conflicts if required

If a conflict occurs, resolve the affected file and stage it:

```bash
git add <file-name>
git rebase --continue
```

To cancel the rebase:

```bash
git rebase --abort
```

To skip the current commit:

```bash
git rebase --skip
```

Use `--skip` carefully because it omits the changes introduced by the current commit.

## 7. Interactive Rebase

Interactive rebase allows developers to modify, reorder, combine, rename, or remove commits before sharing their history.

### 7.1 Basic Syntax

```bash
git rebase -i HEAD~N
```

Here, `N` specifies how many recent commits should be included.

For example:

```bash
git rebase -i HEAD~3
```

This opens an editor containing the selected commits.

### 7.2 Interactive Rebase Commands

| Command | Purpose |
|---|---|
| `pick` | Keep the commit |
| `reword` | Change the commit message |
| `edit` | Pause to modify the commit |
| `squash` | Combine commits and edit the combined message |
| `fixup` | Combine commits and discard the current commit message |
| `drop` | Remove a commit |

### 7.3 Reordering Commits

Move the commit lines into the desired order in the interactive rebase editor.

Git will replay the commits in the new order.

### 7.4 Squashing Commits

Squashing combines multiple commits into one.

Example:

```text
pick   abc123 First commit
squash def456 Second commit
squash ghi789 Third commit
```

The three commits are combined into one commit with an editable commit message.

### 7.5 Rewording a Commit

Use `reword` instead of `pick`:

```text
reword abc123 Old commit message
```

Git opens the commit message editor so you can provide a new message.

### 7.6 Editing a Commit

Use `edit`:

```text
edit abc123 Commit to modify
```

Git pauses at that commit. Make the required changes and amend it:

```bash
git add <file-name>
git commit --amend
git rebase --continue
```

**Note:** Interactive rebase changes commit history and should be used carefully.

## 8. Handling Rebase Conflicts

A rebase conflict occurs when Git cannot replay a commit automatically onto the new base.

### 8.1 Conflict Resolution Steps

1. Start the rebase.
2. Git reports a conflict.
3. Check the repository status.
4. Open and edit the conflicted file.
5. Remove conflict markers and keep the correct content.
6. Stage the resolved file.
7. Continue the rebase.

Commands:

```bash
git status
git add <file-name>
git rebase --continue
```

### 8.2 Aborting a Rebase

If you want to cancel the entire operation:

```bash
git rebase --abort
```

Git returns the branch to its state before the rebase began.

### 8.3 Understanding Rebase Conflict Markers

During a rebase, the conflict markers can be confusing.

For a typical rebase conflict:

```text
 <<<<<<< HEAD
New base branch version
 =======
Commit being replayed
 >>>>>>> commit-hash
```

Resolve the file carefully, save it, stage it, and continue.

**Important:** During a rebase, Git replays commits one at a time. Additional conflicts may occur in later commits.

## 9. Merge vs Rebase vs Cherry-pick

| Feature | Merge | Rebase | Cherry-pick |
|---|---|---|---|
| Main purpose | Combine branch histories | Replay commits onto a new base | Apply selected commit changes |
| Works with | Branches | Branches or commits | Individual commits |
| Rewrites existing commits | No | Yes, for replayed commits | No; creates a new commit |
| Preserves original commit hashes | Yes | No for replayed commits | No for copied commits |
| Typical use | Integrating branches | Updating feature branches | Applying selected fixes |

### Choosing the Right Operation

- Use **merge** when you want to integrate branch histories while preserving their existing commits.
- Use **rebase** when you want to replay commits onto a new base and maintain a linear history.
- Use **cherry-pick** when you need only specific commits from another branch.

## 10. Common Commands

| Command | Description |
|---|---|
| `git cherry-pick <hash>` | Apply a selected commit |
| `git cherry-pick --no-commit <hash>` | Apply changes without automatic commit |
| `git cherry-pick --continue` | Continue cherry-pick |
| `git cherry-pick --abort` | Abort cherry-pick |
| `git rebase <branch>` | Rebase onto another branch |
| `git rebase -i HEAD~N` | Start interactive rebase |
| `git rebase --continue` | Continue rebase |
| `git rebase --abort` | Abort rebase |
| `git rebase --skip` | Skip current commit |
| `git commit --amend` | Modify the latest commit |
| `git log --oneline --graph --all` | Inspect commit history |

## 11. Best Practices

- Always verify the current branch before cherry-picking or rebasing.
- Inspect commit hashes carefully before applying changes.
- Use meaningful commit messages.
- Resolve conflicts carefully and review the final file.
- Use interactive rebase to organize local commits before sharing.
- Avoid rebasing shared branches because it rewrites commit history.
- Coordinate with teammates before rewriting published history.
- Keep a backup branch when experimenting with complex history changes.
- Understand the difference between merge, rebase, and cherry-pick before choosing an operation.

**Important:** Rebasing a branch that has already been shared may require a force push. If a force push is genuinely necessary, prefer `git push --force-with-lease` over `git push --force`, and coordinate with collaborators first.

## 12. Practice Exercises

Complete these exercises in a separate practice repository or dedicated practice branches.

### Exercise 1: Basic Cherry-pick

- Create a source branch.
- Make two separate commits.
- Switch to a target branch.
- Cherry-pick one selected commit.
- Verify the result using `git log`.

### Exercise 2: Cherry-pick Without Commit

- Select a commit from another branch.
- Apply it using `git cherry-pick --no-commit`.
- Inspect the staged changes.
- Create a commit manually.

### Exercise 3: Cherry-pick Conflict

- Modify the same line in two branches.
- Cherry-pick the conflicting commit.
- Resolve the conflict.
- Continue the operation.

### Exercise 4: Basic Rebase

- Create a base branch and a feature branch.
- Add commits to both branches.
- Rebase the feature branch onto the updated base branch.
- Inspect the new commit history.

### Exercise 5: Interactive Rebase

Practice the following operations:

- Squash multiple commits.
- Reword a commit message.
- Edit an existing commit.
- Reorder commits.

### Exercise 6: Rebase Conflict

- Create two branches from a common base.
- Modify the same line in both branches.
- Rebase one branch onto the other.
- Resolve the conflict and continue.
- Verify the final commit history.

---

## Conclusion

Cherry-pick and rebase are important Git operations for managing selected changes and maintaining commit history.

Cherry-pick provides control over which changes are applied, while rebase helps reorganize and replay commits onto a different base.

Understanding these operations improves confidence when working with feature branches, resolving conflicts, and managing professional development workflows.

---

**Learning through practice. Improving through consistency.**

**Previous Guide:** [Git Merge Strategies and Conflict Resolution](../06-git-merge-strategies-and-conflicts/README.md)

**Next Guide:** [Git Tags](../08-git-tags/README.md)