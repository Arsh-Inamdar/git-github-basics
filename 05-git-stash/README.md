# Git Stash

Git stash temporarily saves changes in your working directory so you can switch tasks or branches without committing unfinished work.

## 1. Why Use Git Stash?

Git stash is useful when:

- You have unfinished changes.
- You need to switch to another branch quickly.
- You want to keep your working directory clean.
- You are not ready to create a commit.

## 2. Save Changes

Save your current tracked changes:

```bash
git stash
```

Save changes with a descriptive message:

```bash
git stash push -m "description of changes"
```

## 3. View Stashes

List all saved stashes:

```bash
git stash list
```

View the changes in the latest stash:

```bash
git stash show -p
```

View a summary of the latest stash:

```bash
git stash show --stat
```

## 4. Restore Stashed Changes

Apply the latest stash without deleting it:

```bash
git stash apply
```

Apply the latest stash and remove it after successful application:

```bash
git stash pop
```

**Difference:**
- `apply` restores changes but keeps the stash.
- `pop` restores changes and removes the stash after successful application.

Both `apply` and `pop` use the latest stash by default. You can specify another stash using its reference, such as `stash@{1}`.

## 5. Stash Untracked Files

By default, Git stash does not include untracked files.

Include untracked files using:

```bash
git stash -u
```

Or use a descriptive message:

```bash
git stash push -u -m "save untracked files"
```

The `-u` option includes untracked files but does not include ignored files.

To include ignored files as well, use:

```bash
git stash -a
```

## 6. Remove Stashes

Remove a specific stash:

```bash
git stash drop stash@{0}
```

Remove all saved stashes:

```bash
git stash clear
```

**Warning:** These commands permanently remove the selected stash entries. Make sure you no longer need them before running these commands.

## 7. Important Notes

- Stashes are stored locally and are not pushed to GitHub.
- Git stash is useful for temporarily saving work, not as a permanent backup.
- Applying a stash can cause merge conflicts if the same files have changed.
- Use `git stash -u` when you also need to save untracked files.

## 8. Practice Exercises

1. Create a temporary change in README.md.
2. Save it using `git stash push -m`.
3. View your saved stash using `git stash list`.
4. Restore it using `git stash apply`.
5. Remove the restored change from your working directory.
6. Practise saving an untracked file using `git stash -u`.
7. Restore the untracked file using `git stash pop`.

## Conclusion

Git stash helps developers manage unfinished work without creating unnecessary commits. Understanding `stash`, `apply`, `pop`, and `-u` makes branch switching and daily Git workflows easier.

---

**Previous Guide:** [04 — Git Reset and Revert](../04-git-reset-and-revert/README.md)

**Next Guide:** [06 — Git Merge Strategies and Conflict Resolution](../06-git-merge-strategies-and-conflicts/README.md)
