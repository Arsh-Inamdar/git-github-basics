# Git Worktrees and Submodules

Git worktrees and submodules are advanced Git features that help
developers manage multiple working directories and integrate
external repositories into a project.

## Learning Objectives

- Understand Git worktrees and their purpose.
- Create and manage multiple working trees.
- Switch between branches without repeatedly checking them out.
- Understand Git submodules.
- Add, initialize, update, and remove submodules.
- Understand common submodule workflows and best practices.

## 1. Introduction

Git normally uses one working directory for a repository.

Git worktrees allow multiple working directories to be associated
with the same repository.

Git submodules allow one Git repository to be included inside
another repository while maintaining separate histories.

These features are useful in larger projects and professional
development workflows.

## 2. Git Worktrees

A Git worktree is an additional working directory connected to
an existing Git repository.

Each worktree can have a different branch checked out.

All worktrees share the repository's underlying Git object database.

### Benefits of Git Worktrees

- Work on multiple branches simultaneously.
- Avoid repeatedly switching branches.
- Keep feature development separate from bug fixes.
- Review another branch without disturbing current changes.
- Reduce the need for multiple full repository clones.

## 3. Creating a Git Worktree

The general command is:

\`\`\`bash
git worktree add <path> <branch>
\`\`\`

Example:

\`\`\`bash
git worktree add ../project-hotfix hotfix
\`\`\`

This creates an additional working directory at
\`../project-hotfix\` and checks out the \`hotfix\` branch.

If the branch does not exist, create it with:

\`\`\`bash
git worktree add -b hotfix ../project-hotfix
\`\`\`

The \`-b\` option creates a new branch and checks it out in the
new worktree.

## 4. Listing Git Worktrees

To list all working trees associated with a repository:

\`\`\`bash
git worktree list
\`\`\`

The output displays the paths, associated branches, and commit
information for each worktree.

## 5. Working with Multiple Worktrees

Suppose you are developing a feature on the \`main\` branch and
need to fix an urgent bug.

Without worktrees, you may need to stash changes or switch branches.

With worktrees, you can keep separate working directories.

Example structure:

\`\`\`text
Projects/
├── project-main/
└── project-hotfix/
\`\`\`

- \`project-main\` can contain the main development branch.
- \`project-hotfix\` can contain the hotfix branch.

Changes and commits made in one worktree are part of the same
underlying repository history.

A branch can normally be checked out in only one worktree at a time.

## 6. Removing a Git Worktree

To remove an additional worktree:

\`\`\`bash
git worktree remove <path>
\`\`\`

Example:

\`\`\`bash
git worktree remove ../project-hotfix
\`\`\`

Git normally prevents removal if the worktree contains uncommitted
changes.

To inspect worktree information:

\`\`\`bash
git worktree list --porcelain
\`\`\`

To clean up stale worktree references:

\`\`\`bash
git worktree prune
\`\`\`

Use prune when a worktree directory has been removed outside Git
and its administrative information remains.

## 7. Git Submodules

A Git submodule is an embedded Git repository inside another
Git repository.

It allows a project to depend on an external repository while
preserving that repository's independent history.

Examples of use:

- Shared libraries.
- External components.
- Reusable project dependencies.
- Separately maintained modules.

A submodule is not simply a copied folder. The parent repository
records a reference to a specific commit of the submodule.

## 8. Adding a Git Submodule

The general command is:

\`\`\`bash
git submodule add <repository-url> <path>
\`\`\`

Example:

\`\`\`bash
git submodule add https://github.com/example/library.git external/library
\`\`\`

This adds the external repository as a submodule.

Git also creates or updates the \`.gitmodules\` configuration file.

The parent repository records the submodule's commit reference.

Use a real repository URL when performing this exercise.

## 9. Initializing and Updating Submodules

After cloning a repository containing submodules, initialize and
download them with:

\`\`\`bash
git submodule update --init --recursive
\`\`\`

Alternatively, clone a repository and its submodules together:

\`\`\`bash
git clone --recurse-submodules <repository-url>
\`\`\`

To update submodules to the commits recorded by the parent repository:

\`\`\`bash
git submodule update --recursive
\`\`\`

To update a submodule to the latest commit from its configured
remote tracking branch:

\`\`\`bash
git submodule update --remote
\`\`\`

The parent repository must then record the updated submodule
commit reference in a new commit.

## 10. Inspecting Submodules

To display the configured submodules:

\`\`\`bash
git submodule status
\`\`\`

To inspect the submodule configuration:

\`\`\`bash
cat .gitmodules
\`\`\`

The \`.gitmodules\` file stores information such as submodule
paths and repository URLs.

## 11. Removing a Git Submodule

Removing a submodule requires more care than deleting an ordinary
directory.

A typical removal workflow is:

1. Deinitialize the submodule.
2. Remove its registration.
3. Remove its tracked entry.
4. Commit the parent repository changes.

Example:

\`\`\`bash
git submodule deinit -f -- external/library
git rm -f external/library
rm -rf .git/modules/external/library
git commit -m "chore: remove library submodule"
\`\`\`

Adjust the paths to match the actual submodule location.

Review the repository state before removing anything.

## 12. Worktrees vs Submodules

| Feature | Git Worktrees | Git Submodules |
|---|---|---|
| Main purpose | Multiple working directories | Include another repository |
| Repository relationship | Share one repository | Separate repository |
| Branch usage | Different branches in separate worktrees | Independent submodule history |
| Common use | Parallel development | External dependencies |
| Configuration | Git worktree metadata | .gitmodules file |

## 13. Common Problems and Solutions

| Problem | Possible Solution |
|---|---|
| Branch already checked out | Use another branch or worktree |
| Worktree contains changes | Commit, stash, or safely resolve changes before removal |
| Submodule directory is empty | Run git submodule update --init --recursive |
| Submodule points to an unexpected commit | Inspect git submodule status |
| Submodule changes are not recorded | Stage the submodule reference in the parent repository |
| Stale worktree information | Run git worktree prune after checking the paths |

## 14. Best Practices

- Use worktrees when parallel branch development is needed.
- Give additional worktrees clear directory names.
- Inspect worktree status before removing directories.
- Use submodules only when independent repository history is useful.
- Document submodule dependencies for collaborators.
- Initialize submodules after cloning when necessary.
- Commit updated submodule references in the parent repository.
- Avoid manually deleting Git metadata without understanding its impact.

## 15. Practical Exercises

### Git Worktrees

1. Inspect the current worktree using git worktree list.
2. Create a new branch and an additional worktree.
3. Enter the additional directory.
4. Verify the checked-out branch.
5. Create a test file and commit it.
6. Return to the original working directory.
7. Inspect both worktrees.
8. Remove the additional worktree safely.

### Git Submodules

1. Inspect a public Git repository that contains submodules.
2. Clone it using --recurse-submodules.
3. Inspect its .gitmodules file.
4. Run git submodule status.
5. Initialize and update its submodules.
6. Observe how the parent repository records submodule references.

Perform submodule exercises in a separate practice repository,
not inside the main learning repository unless the dependency
is genuinely useful.

## 16. Key Takeaways

- Git worktrees provide multiple working directories connected
  to one repository.
- Different branches can be checked out in separate worktrees.
- Git submodules integrate separate repositories into a project.
- The .gitmodules file stores submodule configuration.
- Submodule updates are recorded as commit references in the
  parent repository.
- Worktrees and submodules solve different development problems.

---

## Navigation

**Previous Guide:** [Git Hooks and Automation](../10-git-hooks-and-automation/README.md)

**Next Guide:** [GitHub Issues and Projects](../12-github-issues-and-projects/README.md)

**Back to Main README:** [Git & GitHub Mastery](../README.md)
