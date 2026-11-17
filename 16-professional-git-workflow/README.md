# Professional Git Workflow

[← Previous: Releases, Security and Open Source](../15-releases-security-and-open-source/README.md)

## 1. Introduction

Git is not only a tool for saving code. It is a complete version control system that supports professional software development, collaboration, code review, and project maintenance.

A professional Git workflow helps developers:

- Track changes systematically.
- Maintain clean and understandable commit history.
- Work safely with branches.
- Collaborate through pull requests.
- Review and test changes before merging.
- Resolve conflicts.
- Recover from mistakes.
- Maintain secure and organized repositories.
- Release stable versions of software.

This guide combines the important concepts from the previous guides into one practical workflow.

## 2. Understanding a Professional Git Workflow

A typical professional development workflow follows these stages:

1. Clone the repository.
2. Synchronize with the remote repository.
3. Create a feature branch.
4. Make changes.
5. Inspect and test changes.
6. Stage the intended files.
7. Create meaningful commits.
8. Push the branch.
9. Create a pull request.
10. Participate in code review.
11. Merge the approved changes.
12. Delete the completed branch.
13. Synchronize the local main branch.

The workflow can be represented as:

```text
Clone Repository
       |
       v
Synchronize Main
       |
       v
Create Feature Branch
       |
       v
Make Changes
       |
       v
Review and Test
       |
       v
Stage and Commit
       |
       v
Push Feature Branch
       |
       v
Create Pull Request
       |
       v
Code Review
       |
       v
Merge Pull Request
       |
       v
Delete Feature Branch
       |
       v
Synchronize Main
```

## 3. Setting Up a Repository

### 3.1 Clone a repository

To start working on an existing project, clone its remote repository.

```bash
git clone https://github.com/OWNER/REPOSITORY.git
```

Move into the project directory:

```bash
cd REPOSITORY
```

Cloning downloads the repository, including its tracked files and Git history.

### 3.2 Inspect the repository

Check the current branch and working tree:

```bash
git status
```

Display the configured remotes:

```bash
git remote -v
```

View recent commits:

```bash
git log --oneline -5
```

List local branches:

```bash
git branch
```

### 3.3 Configure your Git identity

Git uses your configured identity when creating commits.

Set your name:

```bash
git config --global user.name "Your Name"
```

Set your email:

```bash
git config --global user.email "you@example.com"
```

Verify the settings:

```bash
git config --global --list
```

Use an appropriate email address for your development identity. Do not share passwords or access tokens.

## 4. Synchronizing with the Remote Repository

Before starting new work, make sure your local branch is up to date.

### 4.1 Fetch remote changes

```bash
git fetch origin
```

`git fetch` downloads remote updates without automatically merging them into your current branch.

### 4.2 Pull remote changes

```bash
git pull origin main
```

Pull normally fetches remote changes and integrates them into the current branch.

The exact integration behavior depends on Git configuration and command options.

### 4.3 Pull using rebase

```bash
git pull --rebase origin main
```

This fetches remote changes and replays your local commits on top of the updated branch.

Use the workflow preferred by your project team.

### 4.4 Inspect synchronization status

```bash
git status -sb
```

Example:

```text
## main...origin/main
```

This indicates that the local branch and its configured upstream branch are synchronized.

Other status indicators may show that the local branch is ahead, behind, or has diverged.

## 5. Professional Branch Management

Branches allow developers to work on separate features or fixes without directly changing the main development branch.

### 5.1 Common branch types

| Branch type | Purpose | Example |
|---|---|---|
| main | Stable primary branch | main |
| feature | New functionality | feature/user-profile |
| bugfix | Fixing an existing bug | bugfix/login-validation |
| hotfix | Urgent production fix | hotfix/payment-error |
| docs | Documentation changes | docs/api-guide |
| test | Testing-related work | test/login-cases |
| refactor | Internal code improvements | refactor/auth-service |

Projects may use different naming conventions. Follow the repository's contribution guidelines.

### 5.2 Create a feature branch

First switch to main:

```bash
git switch main
```

Synchronize it:

```bash
git pull origin main
```

Create and switch to a new branch:

```bash
git switch -c feature/user-profile
```

### 5.3 Verify the current branch

```bash
git branch --show-current
```

### 5.4 Rename a branch

Rename the current branch:

```bash
git branch -m feature/profile-page
```

### 5.5 Delete a merged local branch

```bash
git branch -d feature/user-profile
```

Use `-d` for a branch that has been merged.

The `-D` option force-deletes a branch and can discard unmerged work. Use it only when you understand the consequences.

## 6. Making and Reviewing Changes

After creating a feature branch, work on the required task.

### 6.1 Check working tree status

```bash
git status
```

This identifies:
- Modified tracked files.
- Untracked files.
- Staged changes.
- Current branch information.

### 6.2 Inspect unstaged changes

```bash
git diff
```

This displays changes that have not been staged.

### 6.3 Inspect staged changes

```bash
git diff --cached
```

This displays changes already added to the staging area.

### 6.4 Inspect a particular file

```bash
git diff -- README.md
```

### 6.5 Review a summary of changes

```bash
git diff --stat
```

Reviewing changes before committing helps identify accidental edits, debugging statements, and unrelated modifications.

### 6.6 Run tests

Before committing, run the tests and validation commands required by the project.

Examples:

```bash
npm test
```

```bash
python -m pytest
```

```bash
mvn test
```

These commands apply to different project environments. Use the appropriate test command for the repository.

## 7. Staging Changes Correctly

The staging area allows developers to select exactly which changes should be included in the next commit.

### 7.1 Stage a specific file

```bash
git add README.md
```

### 7.2 Stage multiple files

```bash
git add README.md src/app.js
```

### 7.3 Stage all changes

```bash
git add .
```

Use this carefully because it can stage unrelated files.

### 7.4 Interactively stage changes

```bash
git add -p
```

This allows you to select individual change sections.

### 7.5 Unstage a file

```bash
git restore --staged README.md
```

This removes the file from staging without discarding its working directory changes.

### 7.6 Review the staging area

```bash
git diff --cached
```

A professional developer checks the staged changes before creating a commit.

## 8. Writing Professional Commit Messages

Commit messages explain why a change was made.

Good commit messages make project history easier to understand and maintain.

### 8.1 Conventional commit format

A common format is:

```text
type(scope): description
```

Examples:

```text
feat(auth): add user login
fix(api): handle invalid response
docs(readme): update installation steps
test(login): add validation cases
refactor(user): simplify profile service
chore(deps): update dependencies
ci(actions): add documentation checks
```

### 8.2 Common commit types

| Type | Purpose |
|---|---|
| feat | New functionality |
| fix | Bug fix |
| docs | Documentation changes |
| test | Adding or updating tests |
| refactor | Code restructuring without intended behavior changes |
| style | Formatting changes |
| perf | Performance improvements |
| build | Build system or dependency changes |
| ci | Continuous integration changes |
| chore | Maintenance tasks |

### 8.3 Create a commit

```bash
git commit -m "feat(profile): add user profile page"
```

### 8.4 Amend the latest commit

If you need to correct the latest commit:

```bash
git commit --amend
```

This replaces the latest commit with a new commit.

If the original commit was already shared, coordinate with collaborators before rewriting published history.

### 8.5 Professional commit practices

- Write short, descriptive messages.
- Use consistent commit conventions.
- Keep commits focused.
- Avoid messages such as `update`, `changes`, or `final`.
- Do not include unrelated modifications.
- Review staged changes before committing.

## 9. Pushing Changes to GitHub

After creating commits, publish the feature branch.

### 9.1 Push a new branch

```bash
git push -u origin feature/user-profile
```

The `-u` option sets the upstream tracking branch.

### 9.2 Push later changes

```bash
git push
```

After upstream tracking is configured, Git can normally identify the destination.

### 9.3 Check remote branches

```bash
git branch -r
```

### 9.4 Verify the remote repository

```bash
git remote -v
```

Confirm that the branch was pushed to the intended repository.

## 10. Pull Requests and Code Reviews

A pull request (PR) proposes changes from one branch into another.

It allows team members to review, discuss, test, and approve changes before merging.

### 10.1 Creating a pull request

1. Push your feature branch to GitHub.
2. Open the repository.
3. Select **Pull requests**.
4. Select **New pull request**.
5. Choose the base branch, usually `main`.
6. Choose your feature branch.
7. Review the changed files.
8. Enter a clear title and description.
9. Create the pull request.

### 10.2 Writing a good pull request description

A useful pull request description includes:

- What was changed.
- Why the change was necessary.
- How the change was implemented.
- Tests performed.
- Screenshots when useful.
- Related issue references.

Example:

```markdown
## Summary

Added a user profile page.

## Changes

- Added profile information display.
- Added profile editing form.
- Improved validation.

## Testing

- Verified profile loading.
- Tested form validation.
- Checked responsive layout.

Closes #25
```

### 10.3 Code review

A code review checks whether a proposed change is correct, understandable, maintainable, and suitable for the project.

Reviewers may examine:
- Correctness.
- Readability.
- Security.
- Performance.
- Test coverage.
- Documentation.
- Compatibility.

### 10.4 Responding to review comments

1. Read the feedback carefully.
2. Ask for clarification when necessary.
3. Make the requested changes.
4. Test the changes.
5. Commit and push the updates.
6. Reply to the review comments.
7. Request another review when appropriate.

### 10.5 Approval and merging

A pull request may be merged after the required approvals and checks are complete.

Common merge strategies include:

- Merge commit.
- Squash and merge.
- Rebase and merge.

Follow the project's chosen merge strategy.

## 11. Choosing the Right Merge Strategy

### 11.1 Merge

```bash
git switch main
git merge feature/user-profile
```

Merge integrates changes from another branch.

It may create a merge commit when a merge commit is required.

### 11.2 Squash merge

A squash merge combines multiple commits from a feature branch into one commit on the target branch.

It is commonly available through GitHub's pull request interface.

### 11.3 Rebase

```bash
git switch feature/user-profile
git rebase main
```

Rebase replays commits onto another base commit.

It can create a more linear history, but it rewrites commit identities.

### 11.4 Which strategy should you choose?

| Strategy | Suitable situation |
|---|---|
| Merge commit | Preserve the feature branch's history |
| Squash merge | Keep the target branch history concise |
| Rebase | Reorganize local commits onto an updated base |

Do not rewrite shared branch history without coordinating with collaborators.

## 12. Handling Merge Conflicts

A merge conflict occurs when Git cannot automatically reconcile changes.

### 12.1 Identify conflicts

```bash
git status
```

Git identifies the files that need attention.

### 12.2 Understand conflict markers

A conflicted file may contain:

```text
< <<<<<<< HEAD
Current branch content
< =======
Incoming branch content
< >>>>>>> feature/user-profile
```

The sections represent competing changes.

### 12.3 Resolve the conflict

1. Open the conflicted file.
2. Understand both versions.
3. Decide which content should remain.
4. Edit the file to produce the correct result.
5. Remove all conflict markers.
6. Run tests.

### 12.4 Mark the conflict as resolved

```bash
git add filename
```

### 12.5 Complete the merge

```bash
git commit
```

When resolving a rebase conflict, use:

```bash
git rebase --continue
```

### 12.6 Abort an operation

Abort a merge:

```bash
git merge --abort
```

Abort a rebase:

```bash
git rebase --abort
```

Only abort when you intend to cancel the current operation.

## 13. Choosing Between Reset, Revert, and Restore

These commands have different purposes and should not be treated as interchangeable.

| Command | Main purpose |
|---|---|
| git restore | Restore working tree files or unstage changes |
| git reset | Move the current branch pointer and optionally change staging |
| git revert | Create a new commit that reverses an earlier commit |

### 13.1 Restore an unstaged file

```bash
git restore README.md
```

This discards unstaged changes in the specified file.

Be careful: those changes may be lost.

### 13.2 Unstage a file

```bash
git restore --staged README.md
```

This keeps the working directory changes.

### 13.3 Soft reset

```bash
git reset --soft HEAD~1
```

Moves the branch pointer back one commit while keeping the changes staged.

### 13.4 Mixed reset

```bash
git reset --mixed HEAD~1
```

Moves the branch pointer and keeps the changes in the working directory, unstaged.

### 13.5 Hard reset

```bash
git reset --hard HEAD~1
```

Moves the branch pointer and discards tracked changes in the working tree and staging area.

This can permanently discard work. Use it only when you are certain.

### 13.6 Revert a commit

```bash
git revert COMMIT_HASH
```

Creates a new commit that reverses the selected commit's changes.

Revert is generally safer for undoing changes that have already been shared.

## 14. Using Git Stash During Daily Work

Sometimes you need to switch branches before your current work is ready to commit.

Git stash temporarily stores working directory changes.

### 14.1 Save changes

```bash
git stash push -m "work in progress"
```

### 14.2 List stashes

```bash
git stash list
```

### 14.3 Apply a stash

```bash
git stash apply
```

### 14.4 Apply and remove the latest stash

```bash
git stash pop
```

### 14.5 Delete a stash

```bash
git stash drop
```

### 14.6 Stash best practices

- Use descriptive stash messages.
- Review the stash list.
- Apply the correct stash.
- Verify restored changes.
- Remove stashes when no longer needed.

Stash is temporary storage, not a replacement for commits or backups.

## 15. Tags and Releases in a Professional Workflow

Tags mark important commits and releases.

### 15.1 Create an annotated tag

```bash
git tag -a v1.0.0 -m "First stable release"
```

### 15.2 Inspect tags

```bash
git tag
git show v1.0.0
```

### 15.3 Push a tag

```bash
git push origin v1.0.0
```

### 15.4 Follow Semantic Versioning

Use a versioning strategy appropriate for the project.

```text
MAJOR.MINOR.PATCH
```

- MAJOR: incompatible changes.
- MINOR: new backward-compatible functionality.
- PATCH: backward-compatible bug fixes.

### 15.5 Prepare a release

Before publishing a release:

1. Confirm the release branch or target commit.
2. Run the required tests.
3. Review outstanding issues.
4. Update the changelog.
5. Create the release tag.
6. Write release notes.
7. Publish the GitHub Release.

## 16. Repository Organization and Maintenance

A professional repository should be understandable and easy to maintain.

### 16.1 Important repository files

| File | Purpose |
|---|---|
| README.md | Project introduction and instructions |
| LICENSE | Usage and distribution permissions |
| .gitignore | Files Git should ignore |
| CONTRIBUTING.md | Contribution instructions |
| CODE_OF_CONDUCT.md | Community behavior expectations |
| SECURITY.md | Vulnerability reporting policy |
| CHANGELOG.md | Important project changes |

Not every project requires every file, but suitable documentation improves maintainability.

### 16.2 Use a suitable .gitignore

Example:

```gitignore
# Environment files
.env
.env.*

# Dependencies
node_modules/

# Build output
dist/
build/

# Python
__pycache__/
*.pyc
.venv/

# IDE settings
.vscode/
.idea/

# Operating system files
.DS_Store
Thumbs.db
```

Adjust the rules to match the project. Do not ignore files that the team needs to share.

### 16.3 Keep the repository clean

- Remove unused files.
- Avoid committing generated output unnecessarily.
- Keep documentation updated.
- Use meaningful folder names.
- Avoid duplicate or outdated files.
- Maintain a clear project structure.

### 16.4 Maintain the README

A useful project README can include:

- Project name.
- Description.
- Features.
- Technologies used.
- Installation instructions.
- Usage instructions.
- Screenshots.
- Contribution guidelines.
- License information.

### 16.5 Review branches regularly

Remove completed branches when they are no longer needed.

Check local branches:

```bash
git branch
```

Check remote branches:

```bash
git branch -r
```

Prune deleted remote-tracking branches:

```bash
git fetch --prune
```

## 17. Security in Daily Git Work

Security should be part of every development workflow.

Important practices:

- Never commit passwords or private keys.
- Use environment variables for sensitive configuration.
- Use GitHub Secrets for workflow credentials.
- Review dependencies for known vulnerabilities.
- Enable appropriate security alerts.
- Protect important branches.
- Use two-factor authentication.
- Review collaborator access.
- Avoid blindly trusting third-party GitHub Actions.
- Rotate credentials immediately if they are exposed.

Before pushing, inspect the staged changes:

```bash
git diff --cached
```

Check for accidentally staged sensitive files.

## 18. Professional Team Collaboration

A professional team should follow consistent development practices.

### 18.1 Typical team workflow

1. A task is created as an issue.
2. A developer is assigned the task.
3. The developer creates a feature branch.
4. Changes are implemented and tested.
5. Commits are created using project conventions.
6. A pull request is opened.
7. Automated checks run.
8. Reviewers inspect the changes.
9. Requested changes are completed.
10. The pull request is approved and merged.
11. The issue is closed.
12. The completed branch is deleted.

### 18.2 Communication practices

- Provide regular progress updates.
- Ask questions when requirements are unclear.
- Explain technical decisions.
- Keep pull request discussions respectful.
- Document important decisions.
- Avoid silently changing shared workflows.

### 18.3 Continuous Integration

Automated checks can run whenever code is pushed or a pull request is opened.

Typical checks include:

- Build validation.
- Unit tests.
- Integration tests.
- Linting.
- Formatting checks.
- Security analysis.
- Documentation validation.

A pull request should satisfy the project's required checks before merging.

## 19. Common Professional Git Mistakes

| Mistake | Consequence | Better practice |
|---|---|---|
| Working directly on main | Increases risk to stable code | Use feature branches |
| Forgetting to pull or fetch | Causes avoidable integration problems | Synchronize before starting work |
| Committing unrelated changes | Makes review difficult | Keep commits focused |
| Using unclear commit messages | Makes history difficult to understand | Follow a commit convention |
| Pushing secrets | Exposes sensitive information | Review staged files and use secrets |
| Force-pushing shared branches | Can overwrite collaborators' work | Coordinate before rewriting history |
| Ignoring tests | Can introduce regressions | Run project checks |
| Creating oversized pull requests | Makes review difficult | Submit smaller, focused changes |
| Leaving obsolete branches | Creates repository clutter | Clean up completed branches |
| Using hard reset carelessly | Can discard work | Understand reset modes |
| Ignoring review feedback | Delays collaboration | Address review comments |
| Publishing without release notes | Makes changes unclear to users | Prepare release documentation |

## 20. Daily Professional Git Checklist

Use this checklist when working on a development task.

### Before starting

- [ ] Check the current branch.
- [ ] Check working tree status.
- [ ] Fetch or pull recent changes.
- [ ] Read the assigned issue or task.
- [ ] Create a suitable feature branch.

### During development

- [ ] Make focused changes.
- [ ] Follow project coding conventions.
- [ ] Check the diff regularly.
- [ ] Run relevant tests.
- [ ] Avoid committing secrets.
- [ ] Update documentation when necessary.

### Before committing

- [ ] Run `git status`.
- [ ] Review `git diff`.
- [ ] Stage only intended files.
- [ ] Review `git diff --cached`.
- [ ] Run `git diff --cached --check`.
- [ ] Write a meaningful commit message.

### Before opening a pull request

- [ ] Push the correct feature branch.
- [ ] Confirm the target branch.
- [ ] Write a clear pull request title.
- [ ] Explain the changes.
- [ ] Mention tests performed.
- [ ] Link related issues.

### Before merging

- [ ] Review feedback.
- [ ] Complete required changes.
- [ ] Confirm automated checks pass.
- [ ] Obtain required approvals.
- [ ] Follow the project's merge strategy.

### After merging

- [ ] Confirm the pull request was merged.
- [ ] Confirm related issues are closed when appropriate.
- [ ] Delete the completed branch.
- [ ] Synchronize local main.
- [ ] Verify the working tree is clean.

## 21. Practical Exercise: Complete Feature Workflow

Practice the complete feature branch workflow in a repository where you have permission to make changes.

### Step 1: Synchronize main

```bash
git switch main
git pull origin main
```

### Step 2: Create a branch

```bash
git switch -c feature/workflow-practice
```

### Step 3: Create a practice file

```bash
echo "Professional Git workflow practice" > workflow-practice.txt
```

### Step 4: Inspect changes

```bash
git status
git diff
```

### Step 5: Stage the file

```bash
git add workflow-practice.txt
```

### Step 6: Review staged changes

```bash
git diff --cached
```

### Step 7: Commit the change

```bash
git commit -m "test: practice professional feature workflow"
```

### Step 8: Push the branch

```bash
git push -u origin feature/workflow-practice
```

### Step 9: Create a pull request

Open GitHub and create a pull request from `feature/workflow-practice` into `main`.

### Step 10: Merge the pull request

After reviewing the change and completing required checks, merge it using the project's preferred strategy.

### Step 11: Synchronize main

```bash
git switch main
git pull origin main
```

### Step 12: Clean up

```bash
git branch -d feature/workflow-practice
git fetch --prune
```

### Step 13: Verify the final state

```bash
git status
git log --oneline -5
```

### Expected outcome

You should understand the complete lifecycle of a feature branch, from creation to pull request, merge, and cleanup.

This exercise is intended for a suitable practice repository. Do not run it directly on a production project without permission.

## 22. Revision Questions

1. What is a professional Git workflow?
2. Why should developers synchronize before starting new work?
3. What is the purpose of a feature branch?
4. What is the difference between `git fetch` and `git pull`?
5. What does `git status` display?
6. What is the difference between `git diff` and `git diff --cached`?
7. What is the staging area?
8. Why should developers review staged changes?
9. What is the Conventional Commits format?
10. What is the purpose of `git push -u`?
11. What is a pull request?
12. What should a good pull request description contain?
13. What happens during code review?
14. What are the common merge strategies?
15. What is a merge conflict?
16. How do you abort a merge?
17. What is the difference between reset and revert?
18. What is the purpose of Git stash?
19. Why should published commit history be rewritten carefully?
20. What is Semantic Versioning?
21. Why should repositories use `.gitignore`?
22. What is the purpose of branch protection?
23. Why are automated tests important?
24. What should developers do after a pull request is merged?
25. Why is repository maintenance important?

## 23. Key Takeaways

- Professional Git workflows make development organized and predictable.
- Synchronize your local branch before beginning new work.
- Use feature branches for independent tasks.
- Review changes before staging and committing.
- Write meaningful and consistent commit messages.
- Push branches and use pull requests for collaboration.
- Review code and automated checks before merging.
- Understand merge, rebase, reset, revert, and stash.
- Protect credentials and maintain repository security.
- Keep documentation and repository structure organized.
- Clean up completed branches.
- Communicate respectfully with collaborators.
- Test changes before publishing them.
- Maintain a clean and understandable Git history.

---

**Congratulations!** You have reached the final guide in this Git and GitHub Mastery learning repository.

The goal is not simply to memorize Git commands. It is to understand when and why to use them, work safely with others, and maintain a professional development workflow.
