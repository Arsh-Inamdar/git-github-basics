# Git Merge Strategies and Conflict Resolution
A practical guide to understanding Git merge strategies, combining branches, handling merge conflicts, and maintaining a clean project history.
## Table of Contents
1. [Introduction to Git Merge](#1-introduction-to-git-merge)
2. [Understanding Merge Strategies](#2-understanding-merge-strategies)
3. [Fast-Forward Merge](#3-fast-forward-merge)
4. [Three-Way Merge](#4-three-way-merge)
5. [No-Fast-Forward Merge](#5-no-fast-forward-merge)
6. [Squash Merge](#6-squash-merge)
7. [Understanding Merge Conflicts](#7-understanding-merge-conflicts)
8. [Resolving Merge Conflicts](#8-resolving-merge-conflicts)
9. [Common Merge Commands](#9-common-merge-commands)
10. [Best Practices](#10-best-practices)
11. [Practice Exercises](#11-practice-exercises)
---
## 1. Introduction to Git Merge
Git merge is a command used to combine changes from different branches into the current branch.
Branches allow developers to work on separate features or fixes without directly affecting the main development branch.
When the work is complete, Git merge integrates those changes into the target branch.
### Basic Syntax
```bash
git merge <branch-name>
```
**Example:**
```bash
git switch main
git merge feature-login
```
This merges the changes from `feature-login` into the currently checked-out `main` branch.
### Important Concept
Git merges the specified branch into the current branch. The branch you are currently on is the destination of the merge.
---
## 2. Understanding Merge Strategies
Git supports different ways of integrating changes between branches.
| Strategy | Description |
|---|---|
| Fast-forward | Moves the current branch pointer forward. |
| Three-way | Combines changes using two branch tips and their common ancestor. |
| No-fast-forward | Creates a merge commit even when fast-forward is possible. |
| Squash | Combines changes into one staged change set before committing. |
Each approach has a different effect on the project's commit history.
---
## 3. Fast-Forward Merge
A fast-forward merge occurs when the target branch has no new commits since the feature branch was created.
Git can simply move the target branch pointer forward to the latest commit of the feature branch.
### Example
```bash
git switch main
git switch -c feature-fast-forward
echo "Fast-forward merge example" > fast-forward.txt
git add fast-forward.txt
git commit -m "Add fast-forward merge example"
git switch main
git merge feature-fast-forward
```
### Expected Result
Git moves the `main` branch pointer forward without creating a separate merge commit.
Example output:
```text
Updating abc1234..def5678
Fast-forward
&#x20;fast-forward.txt | 1 +
&#x20;1 file changed, 1 insertion(+)
```
### Visual Representation
```text
Before merge:
A---B  main
&#x20;    \\
&#x20;     C---D  feature-fast-forward
After merge:
A---B---C---D  main, feature-fast-forward
```
### Advantages
- Keeps commit history simple and linear.
- Does not create an unnecessary merge commit.
- Useful for straightforward feature integrations.
### Limitation
It does not preserve a separate merge commit showing when the feature branch was integrated.
---
## 4. Three-Way Merge
A three-way merge occurs when both the target branch and feature branch have new commits after they diverged from their common ancestor.
Git uses three points to combine the changes:
1. Common ancestor — the commit where the branches diverged.
2. Target branch tip — the latest commit on the current branch.
3. Feature branch tip — the latest commit on the branch being merged.
### Example
```bash
git switch main
git switch -c feature-three-way
echo "Feature branch change" > feature.txt
git add feature.txt
git commit -m "Add feature branch change"
git switch main
echo "Independent main branch change" > main.txt
git add main.txt
git commit -m "Add independent main branch change"
git merge feature-three-way
```
### Expected Result
Git creates a merge commit that combines the changes from both branches.
Example history:
```text
&#x20;     C---D  feature-three-way
&#x20;    /     \\
A---B-------M  main
```
Here, `M` represents the merge commit.
### Advantages
- Integrates independent development work.
- Preserves the relationship between both branches.
- Records the integration in project history.
### Important Note
A three-way merge does not always create conflicts. Conflicts occur only when Git cannot automatically reconcile the changes.
---
## 5. No-Fast-Forward Merge
A no-fast-forward merge forces Git to create a merge commit, even when a fast-forward merge would be possible.
It is useful when developers want to preserve a visible record of feature branch integration.
### Syntax
```bash
git merge --no-ff <branch-name>
```
### Example
```bash
git switch main
git switch -c feature-no-ff
echo "No-fast-forward merge example" > no-ff.txt
git add no-ff.txt
git commit -m "Add no-fast-forward merge example"
git switch main
git merge --no-ff feature-no-ff
```
Git may open a text editor to confirm the merge commit message. Save and close the editor to complete the merge.
### Expected Result
Git creates a separate merge commit.
Example history:
```text
&#x20;     C---D  feature-no-ff
&#x20;    /     \\
A---B-------M  main
```
### Advantages
- Preserves feature branch integration history.
- Makes it easier to identify completed feature branches.
- Useful for team workflows and structured development.
### Difference Between Normal Merge and No-Fast-Forward
| Normal merge | No-fast-forward merge |
|---|---|
| Fast-forward may occur when possible. | Always creates a merge commit. |
| Can produce linear history. | Preserves a visible merge point. |
| Uses default merge behavior. | Uses `--no-ff`. |
---
## 6. Squash Merge
A squash merge combines all changes from a feature branch into one staged change set on the target branch.
Unlike a normal merge, it does not automatically create a merge commit or preserve the individual feature commits as ancestors of the target branch.
### Syntax
```bash
git merge --squash <branch-name>
```
### Example
```bash
git switch main
git switch -c feature-squash
echo "First change" > squash.txt
git add squash.txt
git commit -m "Add first squash change"
echo "Second change" >> squash.txt
git add squash.txt
git commit -m "Add second squash change"
git switch main
git merge --squash feature-squash
git status
git commit -m "Add squash merge changes"
```
### Expected Result
Git combines the changes into the target branch's staging area.
The developer then creates one new commit containing the combined changes.
### Advantages
- Keeps the target branch history cleaner.
- Combines several small commits into one meaningful commit.
- Useful when feature branches contain many temporary or intermediate commits.
### Important Difference
A squash merge does not move the feature branch into the target branch's commit ancestry.
The original feature branch commits remain on the feature branch unless that branch is deleted.
### Normal Merge vs Squash Merge
| Normal merge | Squash merge |
|---|---|
| Preserves commit ancestry. | Combines changes into one new commit. |
| May create a merge commit. | Requires a separate commit after squashing. |
| Keeps feature commit history connected. | Does not preserve individual feature commits in target ancestry. |
---
## 7. Understanding Merge Conflicts
A merge conflict occurs when Git cannot automatically combine changes from different branches.
This commonly happens when:
- Two branches modify the same lines in a file.
- One branch deletes a file while another modifies it.
- Different changes cannot be reconciled automatically.
### Example of a Merge Conflict
Suppose two branches modify the same line.
**Target branch version:**
```text
This is the TARGET branch version of the file.
```
**Feature branch version:**
```text
This is the FEATURE branch version of the file.
```
When Git tries to merge these changes, it may report:
```text
CONFLICT (content): Merge conflict in example.txt
Automatic merge failed; fix conflicts and then commit the result.
```
### Understanding Conflict Markers
Git inserts special markers into the conflicted file:
```text
 <<<<<<< HEAD
This is the TARGET branch version of the file.
 =======
This is the FEATURE branch version of the file.
 >>>>>>> feature-branch
```
| Marker | Meaning |
|---|---|
| `<<<<<<< HEAD` | Beginning of the current branch's version. |
| `=======` | Separates the two conflicting versions. |
| `>>>>>>> feature-branch` | End of the incoming branch's version. |
**Important:** These markers are temporary instructions for resolving the conflict. They must not remain in the final resolved file.
---
## 8. Resolving Merge Conflicts
Git cannot always decide which version of a change should be kept. The developer must review the conflicting content and resolve it.
### Step-by-Step Resolution
**Step 1: Check repository status**
```bash
git status
```
**Step 2: Open the conflicted file**
Use a text editor such as VS Code or Notepad.
**Step 3: Review both versions**
Understand the changes from the current and incoming branches.
**Step 4: Edit the file**
Choose one version, combine both versions, or write a suitable replacement.
Remove all conflict markers.
**Step 5: Stage the resolved file**
```bash
git add example.txt
```
**Step 6: Complete the merge**
```bash
git commit -m "Resolve merge conflict"
```
### Verify the Resolution
```bash
git status
git log --oneline --graph
```
The repository should have no unresolved conflicts, and the merge commit should appear in the history.
### Abort a Merge
If you want to cancel an in-progress merge before completing it, use:
```bash
git merge --abort
```
This attempts to return the working tree to its state before the merge began.
**Warning:** Always inspect your working tree before merging or aborting. Avoid using destructive commands without understanding their effects.
---
## 9. Common Merge Commands
| Command | Purpose |
|---|---|
| `git branch` | List local branches. |
| `git switch <branch>` | Switch to another branch. |
| `git switch -c <branch>` | Create and switch to a new branch. |
| `git merge <branch>` | Merge another branch into the current branch. |
| `git merge --no-ff <branch>` | Force a merge commit. |
| `git merge --squash <branch>` | Squash changes into the staging area. |
| `git status` | Check repository status. |
| `git diff` | View unstaged changes. |
| `git diff --staged` | View staged changes. |
| `git add <file>` | Stage a resolved file. |
| `git merge --abort` | Abort an in-progress merge. |
| `git log --oneline --graph --all` | Visualize commit history. |
---
## 10. Best Practices
1. Always check `git status` before starting a merge.
2. Switch to the correct target branch before merging.
3. Pull the latest changes before integrating shared work.
4. Use meaningful branch names.
5. Write clear and descriptive commit messages.
6. Review conflicting changes carefully before resolving them.
7. Test the project after resolving conflicts.
8. Avoid unnecessary merge commits when a linear history is preferred.
9. Use squash merging when a clean, consolidated commit is desirable.
10. Never use `git reset --hard` as a way to resolve conflicts without understanding its consequences.
---
## 11. Practice Exercises
Complete these exercises in a separate practice repository or dedicated practice branches.
### Exercise 1: Fast-Forward Merge
- Create a feature branch.
- Add and commit a new file.
- Switch to the target branch.
- Perform a normal merge.
- Inspect the commit history.
### Exercise 2: Three-Way Merge
- Create a feature branch.
- Add a commit to the feature branch.
- Add a separate commit to the target branch.
- Merge the feature branch.
- Inspect the merge commit.
### Exercise 3: No-Fast-Forward Merge
- Create a feature branch.
- Make a commit.
- Merge using `git merge --no-ff`.
- Inspect the resulting history.
### Exercise 4: Squash Merge
- Create a feature branch.
- Make at least two commits.
- Squash merge into the target branch.
- Create one consolidated commit.
- Compare both branch histories.
### Exercise 5: Merge Conflict Resolution
- Create a common file on a branch.
- Modify the same line differently on two branches.
- Merge the branches.
- Inspect the conflict markers.
- Resolve the conflict.
- Stage the file and complete the merge.
- Verify the final commit history.
---
## Conclusion
Git merge strategies help developers integrate changes while maintaining a reliable project history.
Understanding fast-forward, three-way, no-fast-forward, and squash merging allows developers to choose the right integration approach.
Learning to resolve merge conflicts is an essential skill for individual development and professional team collaboration.
**Keep practising these workflows to become confident with Git branching and merging.**
