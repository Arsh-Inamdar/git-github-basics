# Git Reset and Revert

A practical guide to understanding how Git handles commit history and how to safely undo changes.

## 1. Introduction

Git provides different commands for undoing changes. Two important commands are:

- `git reset` – moves the current branch to an earlier commit.
- `git revert` – creates a new commit that reverses the changes introduced by an earlier commit.

Understanding the difference helps developers manage Git history safely.

## 2. Git Reset

The `git reset` command moves the current branch pointer to a specified commit.

It has three commonly used modes.

### 2.1 Git Reset --soft

```bash
git reset --soft HEAD~1
```

**What it does:**
- Moves the current branch pointer back by one commit.
- Keeps changes in the staging area.
- Preserves changes in the working directory.

**Use case:** When you want to undo the latest commit but keep its changes staged for another commit.

### 2.2 Git Reset --mixed

```bash
git reset --mixed HEAD~1
```

**What it does:**
- Moves the current branch pointer back by one commit.
- Resets the staging area.
- Preserves working-directory changes.

**Use case:** When you want to undo a commit but keep the changes as unstaged modifications.

Note: `--mixed` is the default mode of `git reset`.

### 2.3 Git Reset --hard

```bash
git reset --hard HEAD~1
```

**What it does:**
- Moves the current branch pointer back by one commit.
- Resets the staging area.
- Resets tracked files in the working directory to match the target commit.

**Use case:** When you intentionally want to discard tracked changes and return to an earlier state.

**Warning:** This command can permanently discard uncommitted tracked changes. Use it carefully.

## 3. Git Revert

The `git revert` command creates a new commit that reverses the changes introduced by an earlier commit.

### Syntax

```bash
git revert <commit-id>
```

### Example

```bash
git revert 1aef4c4
```

**What it does:**
- Identifies the changes introduced by the specified commit.
- Creates an inverse change.
- Records that reversal as a new commit.
- Preserves the original commit in history.

**Use case:** When you need to undo a change without rewriting existing project history.

Git revert is generally safer than reset when undoing changes on shared branches.

## 4. Difference Between Reset and Revert

| Feature | Git Reset | Git Revert |
|---|---|---|
| Main purpose | Moves branch history | Reverses a commit |
| Creates a new commit | No | Yes |
| Original commit in history | No longer on the current branch's history | Preserved |
| Shared branches | Use with caution | Generally safer |
| Common use | Local commit correction | Undoing shared changes |

## 5. Important Commands

| Command | Purpose |
|---|---|
| `git log --oneline` | View compact commit history |
| `git status` | Check repository status |
| `git diff` | View unstaged changes |
| `git diff --cached` | View staged changes |
| `git reset --soft HEAD~1` | Undo latest commit, keep changes staged |
| `git reset --mixed HEAD~1` | Undo latest commit, keep changes unstaged |
| `git reset --hard HEAD~1` | Undo latest commit and reset tracked files |
| `git revert <commit-id>` | Reverse a specific commit |

## 6. Best Practices

1. Check `git status` before performing reset or revert operations.
2. Review commit history using `git log --oneline`.
3. Avoid using `git reset --hard` when you have important uncommitted work.
4. Prefer `git revert` when undoing changes already shared with others.
5. Use meaningful commit messages.
6. Practise potentially destructive commands on separate branches.

## 7. Practice Exercises

- Create a practice file and commit it.
- Use `git reset --soft` and inspect the staging area.
- Use `git reset --mixed` and inspect the working directory.
- Use `git reset --hard` on a disposable practice branch.
- Use `git revert` to reverse a previous commit.
- Compare the commit histories before and after each operation.

## 8. Conclusion

Git reset and git revert are important tools for managing project history.

Reset is useful for correcting local commit history, while revert is useful for safely reversing changes through a new commit.

Practising both commands helps developers understand Git's version-control workflow and collaborate more effectively.
