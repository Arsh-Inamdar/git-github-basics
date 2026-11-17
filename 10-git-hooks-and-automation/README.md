# Git Hooks and Automation

Git hooks are scripts that Git executes automatically at specific
points in the Git workflow. They help developers automate tasks,
enforce rules, and maintain code quality.

## Learning Objectives

- Understand Git hooks and their purpose.
- Differentiate client-side and server-side hooks.
- Identify commonly used Git hooks.
- Create and configure custom Git hooks.
- Automate checks before committing changes.
- Understand hook execution and troubleshooting.

## 1. Introduction to Git Hooks

Git hooks are executable scripts that run automatically when
specific Git events occur.

They can be used to:

- Validate code before committing.
- Check commit messages.
- Run automated tests.
- Prevent unwanted changes.
- Automate development workflows.

Git hooks help maintain consistency and reduce repetitive manual work.

## 2. Types of Git Hooks

Git hooks are classified into two main categories.

### 2.1 Client-Side Hooks

Client-side hooks execute on the developer's local machine.

Examples:

- pre-commit
- prepare-commit-msg
- commit-msg
- post-commit
- pre-push

They are commonly used for code validation, testing, and commit checks.

### 2.2 Server-Side Hooks

Server-side hooks execute on the Git server.

Examples:

- pre-receive
- update
- post-receive

They can enforce repository policies and automate server-side tasks.

Server-side hooks are generally available when administrators control
the Git server. Hosted platforms such as GitHub do not allow ordinary
repository users to install arbitrary server-side Git hooks.

## 3. Location of Git Hooks

By default, Git stores repository-specific hooks in:

\`\`\`text
.git/hooks/
\`\`\`

To inspect the available hooks, run:

\`\`\`bash
ls .git/hooks
\`\`\`

Git provides sample hook files with the extension \`.sample\`.

These sample files are not executed automatically. A hook must have
the appropriate filename and be executable.

## 4. Important Git Hooks

| Hook | Purpose |
|---|---|
| pre-commit | Runs before a commit is created |
| prepare-commit-msg | Prepares the default commit message |
| commit-msg | Validates the commit message |
| post-commit | Runs after a commit is created |
| pre-push | Runs before changes are pushed |
| pre-receive | Checks incoming updates on a server |
| update | Checks an individual reference update |
| post-receive | Runs after references are updated |

### Understanding Hook Execution

A simplified commit workflow:

1. Changes are staged.
2. Git runs pre-commit.
3. Git runs prepare-commit-msg.
4. Git runs commit-msg.
5. Git creates the commit.
6. Git runs post-commit.

If a required pre-commit or commit-msg hook exits with a non-zero
status, Git normally stops the commit.

## 5. Creating a Custom Git Hook

Let's create a simple pre-commit hook.

First, navigate to the hooks directory:

\`\`\`bash
cd .git/hooks
\`\`\`

Create a file named:

\`\`\`text
pre-commit
\`\`\`

Add the following script:

\`\`\`bash
#!/bin/sh

echo "Running pre-commit checks..."

if ! git diff --cached --check; then
    echo "Commit rejected: fix whitespace errors first."
    exit 1
fi

echo "Pre-commit checks passed."
exit 0
\`\`\`

Make the hook executable:

\`\`\`bash
chmod +x pre-commit
\`\`\`

Return to the repository root:

\`\`\`bash
cd ../..
\`\`\`

Now Git will run this script before creating a commit.

The script checks staged changes for whitespace errors.

## 6. Understanding the Hook Script

### Shebang

\`\`\`bash
#!/bin/sh
\`\`\`

Specifies the shell used to execute the script.

### Running a Git Check

\`\`\`bash
git diff --cached --check
\`\`\`

Checks staged changes for whitespace problems, including trailing
whitespace and certain conflict-marker issues.

### Exit Status

\`\`\`bash
exit 0
\`\`\`

Indicates that the hook succeeded.

\`\`\`bash
exit 1
\`\`\`

Indicates that the hook failed and the commit should be stopped.

## 7. Configuring a Custom Hooks Directory

Git allows developers to use a custom hooks directory.

For example:

\`\`\`bash
git config core.hooksPath .githooks
\`\`\`

This tells Git to look for hooks inside the repository's
\`.githooks\` directory instead of the default \`.git/hooks\` directory.

A custom hooks directory can be useful when a team wants to keep
hook scripts in version control and share them with collaborators.

Remember that each developer may need to configure the hooks path
locally. Cloning a repository does not automatically activate its hooks.

## 8. Bypassing Git Hooks

Git provides an option to bypass certain commit hooks:

\`\`\`bash
git commit --no-verify -m "commit message"
\`\`\`

This skips the pre-commit and commit-msg hooks for that commit.

Use this option only when necessary. Bypassing checks can allow
unwanted changes into the repository.

It does not bypass every Git hook.

## 9. Git Hooks and Automation

Git hooks can automate many development activities.

Common examples:

- Running tests before committing.
- Checking formatting and code quality.
- Validating commit message conventions.
- Running checks before pushing.
- Enforcing repository-specific rules.

Hooks are useful for local automation, while CI/CD platforms such
as GitHub Actions provide automation on hosted repositories.

## 10. Troubleshooting Git Hooks

Common problems include:

| Problem | Possible Solution |
|---|---|
| Hook does not execute | Check its filename and executable permission |
| Permission denied | Run chmod +x on the hook |
| Hook fails unexpectedly | Inspect the script and its exit status |
| Hook is in the wrong directory | Check git config core.hooksPath |
| Hook works locally but not for teammates | Share and document the hook setup |

To inspect the configured hooks directory:

\`\`\`bash
git config --show-origin --get core.hooksPath
\`\`\`

If no custom hooks path is configured, Git uses its default hooks
directory.

## 11. Practical Exercises

1. Inspect the default Git hooks directory.
2. Identify client-side and server-side hooks.
3. Create a custom pre-commit hook.
4. Make the hook executable.
5. Stage a file containing a whitespace error.
6. Attempt a commit and observe the hook rejecting it.
7. Fix the whitespace error and commit successfully.
8. Inspect the commit history.

## 12. Best Practices

- Keep hook scripts simple and focused.
- Make scripts executable where required.
- Provide clear error messages.
- Avoid unnecessarily slow operations.
- Test hooks before relying on them.
- Document shared hook installation steps.
- Do not bypass checks without a valid reason.
- Use CI/CD for checks that must run consistently on a server.

## 13. Key Takeaways

- Git hooks are scripts triggered by Git events.
- Client-side hooks run on a developer's machine.
- Server-side hooks run on the Git server.
- Hooks can automate validation and development tasks.
- The default hooks directory is \`.git/hooks/\`.
- A non-zero exit status can stop certain Git operations.
- Custom hooks directories can be configured with core.hooksPath.
- Git hooks complement, but do not replace, CI/CD automation.

---

## Navigation

**Previous Guide:** [Advanced Git Inspection and Recovery](../09-advanced-git-inspection-and-recovery/README.md)

**Next Guide:** [Git Worktrees and Submodules](../11-git-worktrees-and-submodules/README.md)

**Back to Main README:** [Git & GitHub Mastery](../README.md)
