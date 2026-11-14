# 09 - Advanced Git Inspection and Recovery

Git provides several commands that help developers investigate repository history, understand changes, trace modifications, and recover work after accidental operations.

This guide covers five important commands:

1. `git show`
2. `git diff`
3. `git blame`
4. `git reflog`
5. `git fsck`

These commands are especially useful when working with feature branches, pull requests, merges, rebases, resets, and repository recovery.

---

## 1. Git Show

The `git show` command displays information about Git objects, especially commits. It can show commit metadata, messages, and changes.

### Basic command

```bash
git show
```

Displays information about the latest commit.

### Inspect a specific commit

```bash
git show HEAD
git show <commit-id>
```

### Display a compact commit summary

```bash
git show --stat --oneline HEAD
```

Shows the commit ID, message, and summary of changed files.

### Inspect commit metadata

```bash
git show --no-patch --pretty=raw HEAD
```

Displays raw commit metadata, including parent commit IDs.

### Inspect a merge commit

```bash
git show --no-patch --pretty=raw HEAD
git show -m --stat HEAD
```

A normal merge commit usually has two parents:

- **First parent:** The commit that was at the tip of the branch receiving the merge.
- **Second parent:** The commit that was at the tip of the branch being merged.

The `-m` option displays a separate comparison against each parent of a merge commit.

### When to use

- Investigating previous commits.
- Reviewing changes before reverting.
- Understanding merge commits.
- Checking which files were modified.
- Inspecting commit metadata.

---

## 2. Git Diff

The `git diff` command compares changes between your working directory, staging area, and commits.

Understanding these comparisons helps prevent accidental commits and makes code reviews easier.

### Compare working directory with staging area

```bash
git diff
```

Displays changes that have not yet been staged.

### Compare staged changes with the latest commit

```bash
git diff --staged
```

Displays changes that are staged and ready to be committed.

### Compare working directory with the latest commit

```bash
git diff HEAD
```

Displays both staged and unstaged changes compared with the latest commit.

### Compare two branches or commits

```bash
git diff main..HEAD
```

Compares the snapshots at `main` and `HEAD`.

### Understand two-dot and three-dot comparisons

```bash
git diff main..HEAD
git diff main...HEAD
```

These commands have different purposes:

- `git diff main..HEAD` compares the endpoint snapshots of `main` and `HEAD`.
- `git diff main...HEAD` compares the common ancestor of `main` and `HEAD` with `HEAD`.

The three-dot form is particularly useful when reviewing a feature branch because it shows the changes introduced on that branch since it diverged from the base branch.

### Display a summary of changed files

```bash
git diff --stat
```

### Check for whitespace errors

```bash
git diff --check
```

### When to use

- Reviewing modifications before committing.
- Checking staged and unstaged changes.
- Comparing branches.
- Reviewing feature branch changes before a pull request.
- Detecting whitespace errors.

---

## 3. Git Blame

The `git blame` command identifies the commit and author associated with the latest modification of each line in a file.

### Basic command

```bash
git blame README.md
```

### Inspect a specific line range

```bash
git blame -L 1,20 README.md
```

Displays the history information for lines 1 through 20.

### Investigate the history of specific lines

```bash
git log -L 1,20:README.md --oneline
```

Shows the changes affecting the selected line range.

### Information displayed

- Commit ID
- Author
- Date
- Line number
- Line content

**Important:** Git blame identifies the last commit that modified the current version of a line. It does not necessarily identify the original author.

### When to use

- Investigating bugs.
- Tracing when a change was introduced.
- Understanding the history of specific code.
- Finding the commit associated with a modification.

---

## 4. Git Reflog

The `git reflog` command records recent movements of local references, especially `HEAD` and branch references.

It is particularly useful when commits become difficult to locate after reset, rebase, or other history-changing operations.

### Display recent HEAD movements

```bash
git reflog
```

### Display the last ten entries

```bash
git reflog -10
```

### Inspect a particular branch's reflog

```bash
git reflog show main -10
```

### Inspect repository commit history

```bash
git log --oneline --decorate -10
```

### Git log vs Git reflog

| Command | Purpose |
|---|---|
| `git log` | Displays commit history reachable from the selected reference |
| `git reflog` | Displays recent local movements of references |

### Recover a previous commit

First, identify the correct commit ID from the reflog.

Then create a recovery branch:

```bash
git branch recovery-branch <commit-id>
```

This creates a new branch pointing to the identified commit without moving your current branch.

Inspect the recovered commit:

```bash
git log --oneline recovery-branch
```

**Important:** Reflog is local to your repository. Its entries can expire, so it is not a substitute for remote backups.

### When to use

- Recovering commits after accidental resets.
- Locating commits after rebasing.
- Investigating recent branch movements.
- Finding previous repository states.

---

## 5. Git Fsck

The `git fsck` command checks the integrity and connectivity of Git objects and can identify dangling or unreachable objects.

Git stores repository information using objects such as blobs, trees, commits, and tags.

### Basic integrity check

```bash
git fsck
```

### Full integrity check

```bash
git fsck --full
```

### Find unreachable objects

```bash
git fsck --full --no-reflogs --unreachable
```

The `--no-reflogs` option tells Git not to consider reflog entries when determining reachability.

### Git object types

| Object | Description |
|---|---|
| Blob | Stores file contents |
| Tree | Represents directories and file names |
| Commit | Stores a repository snapshot and parent references |
| Tag | Stores tag information |

### Understanding the results

- **Unreachable objects:** Objects that cannot be reached from the references considered by the check.
- **Dangling objects:** Unreachable objects that are not directly referenced by another object in the expected object graph.
- **Dangling commits:** Commits that may contain useful work even though no current branch points to them.

Unreachable objects are not necessarily corrupted. They may remain after resets, rebases, deleted branches, or other operations.

### Inspect an unreachable commit

```bash
git show --stat --oneline <commit-id>
```

### Inspect its contents

```bash
git show <commit-id>
```

### Recover a useful commit

```bash
git branch recovery-branch <commit-id>
```

This preserves the commit under a new branch name so that you can inspect and recover its work safely.

**Warning:** Do not delete unreachable objects simply because Git reports them. Inspect them first. Avoid manually pruning objects when recovery may be necessary.

### When to use

- Investigating unreachable objects.
- Checking repository object integrity.
- Finding commits after accidental operations.
- Recovering potentially lost work.

---

## 6. Git Inspection Command Comparison

| Command | Main purpose |
|---|---|
| `git show` | Inspect commits and their changes |
| `git diff` | Compare changes |
| `git blame` | Trace line-level modifications |
| `git reflog` | Inspect recent reference movements |
| `git fsck` | Check object integrity and find unreachable objects |

---

## 7. Practical Exercises

Complete these exercises in your practice repository.

1. Inspect the latest commit using `git show`.
2. Display the summary of the latest commit.
3. Compare staged and unstaged changes using `git diff`.
4. Compare `main..HEAD` and `main...HEAD`.
5. Inspect the first 20 lines of README using `git blame`.
6. Investigate the history of README lines using `git log -L`.
7. Display the last ten HEAD movements using `git reflog`.
8. Compare the output of `git log` and `git reflog`.
9. Run a full object integrity check using `git fsck --full`.
10. Inspect an unreachable commit without modifying repository history.

---

## 8. Best Practices

- Inspect changes before committing.
- Use `git show` to understand previous commits.
- Use `git diff` to review changes before creating pull requests.
- Understand the difference between staged and unstaged changes.
- Use `git blame` to investigate history, not to assign fault.
- Use `git reflog` when locating commits after accidental operations.
- Inspect unreachable objects before attempting recovery.
- Create a recovery branch before experimenting with an old commit.
- Avoid pruning objects when recovery may be necessary.
- Keep important work pushed to a remote repository.

---

## 9. Conclusion

Advanced Git inspection commands help developers understand repository history, investigate changes, identify the source of modifications, and recover potentially lost work.

Learning these commands builds confidence when managing complex Git workflows, troubleshooting problems, reviewing pull requests, and maintaining professional repositories.

---

## Navigation

- **Previous:** [08 - Git Tags](../08-git-tags/README.md)
- **Next:** Guide 10 — Coming Soon