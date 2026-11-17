# GitHub Actions and CI/CD

[← Previous: GitHub Discussions and Collaboration](../13-github-discussions-and-collaboration/README.md) | [Next: Releases, Security and Open Source →](../15-releases-security-and-open-source/README.md)

## Introduction

GitHub Actions is an automation platform integrated into GitHub. It allows developers to automate software development workflows, including testing, building, deployment, and documentation validation.

CI/CD helps teams identify problems early and deliver software more reliably.

## 1. Understanding GitHub Actions

GitHub Actions allows developers to create automated workflows that respond to repository events.

### Common Uses

- Running automated tests.
- Building applications.
- Checking code quality.
- Validating documentation.
- Deploying applications.
- Automating repetitive tasks.

### Benefits

- Reduces repetitive manual work.
- Detects problems early.
- Improves consistency.
- Supports collaboration.
- Integrates directly with GitHub repositories.

## 2. Understanding CI and CD

CI/CD represents practices used to automate software integration and delivery.

### Continuous Integration (CI)

Continuous Integration is the practice of frequently integrating code changes into a shared repository and automatically validating them.

Typical activities:
- Checking out source code.
- Running tests.
- Performing code-quality checks.
- Building the application.

### Continuous Delivery

Continuous Delivery keeps software ready for release. Deployment to production usually requires an approval or manual decision.

### Continuous Deployment

Continuous Deployment automatically releases validated changes to production without requiring a separate manual release approval.

### Comparison

| Practice | Purpose |
|---|---|
| CI | Integrate and validate changes |
| Continuous Delivery | Keep changes ready for release |
| Continuous Deployment | Automatically release validated changes |

## 3. Understanding Workflow Files

GitHub Actions workflows are defined using YAML files.

Workflow files must be stored inside:

```text
.github/
└── workflows/
    └── workflow-name.yml
```

A repository can contain multiple workflow files for different automation tasks.

### Basic Workflow Example

```yaml
name: Example Workflow

on:
  push:
    branches:
      - main

jobs:
  example:
    runs-on: ubuntu-latest
    steps:
      - name: Display a message
        run: echo "GitHub Actions is working!"
```

This workflow runs when changes are pushed to the main branch.

## 4. Understanding Workflow Components

### Workflow

A workflow is an automated process defined in a YAML file.

### Event

An event determines when a workflow should run.

Common events include:

- `push`
- `pull_request`
- `workflow_dispatch`
- `schedule`
- `issues`

### Job

A job is a group of steps executed on a runner.

Jobs run in parallel by default unless dependencies are defined.

### Step

A step is an individual task inside a job.

A step can execute a shell command or use an existing action.

### Runner

A runner is the machine that executes workflow jobs.

Common GitHub-hosted runner labels include:

- `ubuntu-latest`
- `windows-latest`
- `macos-latest`

### Action

An action is a reusable unit of automation.

For example, `actions/checkout` checks out repository code so later workflow steps can access it.

## 5. Understanding YAML Syntax

YAML is a human-readable data serialization format used to configure workflows.

### Important Rules

- Use spaces for indentation.
- Do not use tabs for indentation.
- Keep nested elements aligned.
- Use a colon to separate keys and values.
- Use hyphens for list items.

### Example

```yaml
name: Documentation Validation

on:
  push:
    branches:
      - main

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Check repository
        run: echo "Checking documentation"
```

Incorrect indentation can cause workflow errors.

## 6. Understanding Workflow Triggers

Triggers determine when a workflow starts.

### Push

Runs when commits are pushed to a repository.

```yaml
on:
  push:
    branches:
      - main
```

### Pull Request

Runs when a Pull Request targets the specified branch.

```yaml
on:
  pull_request:
    branches:
      - main
```

### Manual Trigger

Allows a workflow to be started manually from GitHub.

```yaml
on:
  workflow_dispatch:
```

### Scheduled Trigger

Runs a workflow according to a cron schedule.

```yaml
on:
  schedule:
    - cron: "0 0 * * 1"
```

The example runs every Monday at midnight UTC.

## 7. Understanding Jobs and Steps

A workflow can contain multiple jobs, and each job can contain multiple steps.

### Example

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Display current directory
        run: pwd

      - name: List files
        run: ls -la
```

### Important Properties

| Property | Purpose |
|---|---|
| `name` | Gives a workflow, job, or step a readable name |
| `on` | Defines workflow triggers |
| `jobs` | Defines workflow jobs |
| `runs-on` | Selects the runner |
| `steps` | Defines tasks within a job |
| `uses` | Runs a reusable action |
| `run` | Executes a shell command |
| `needs` | Defines job dependencies |
| `if` | Controls conditional execution |

## 8. Understanding Job Dependencies

Jobs run in parallel by default.

The `needs` keyword makes one job wait for another job to finish successfully.

### Example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building project"

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing project"
```

The test job starts after the build job succeeds.

## 9. Understanding Secrets and Variables

Workflows sometimes need configuration values and sensitive information.

### Variables

Variables store non-sensitive configuration values.

Examples:
- Application environment.
- Project configuration.
- Deployment region.

### Secrets

Secrets store sensitive information such as:
- API tokens.
- Deployment credentials.
- Access keys.

Secrets should never be hardcoded in workflow files or committed to Git.

GitHub provides repository, organization, and environment-level secrets depending on the account and repository configuration.

### Security Practices

- Never expose tokens in logs.
- Give workflows minimum required permissions.
- Avoid printing secret values.
- Review third-party actions before using them.
- Do not execute untrusted code with privileged secrets.

## 10. Understanding Workflow Permissions

Workflow permissions control what the automatically generated `GITHUB_TOKEN` can access.

Example:

```yaml
permissions:
  contents: read
```

This grants read access to repository contents.

Use only the permissions required by the workflow. Some operations, such as creating releases or updating repository content, require additional permissions.

## 11. Practical Exercise: Create a GitHub Actions Workflow

In this exercise, create a real workflow that checks whether your learning repository contains its expected documentation files.

### Step 1: Create the Workflow Folder

From the repository root, run:

```bash
mkdir -p .github/workflows
```

### Step 2: Create the Workflow File

Create:

```text
.github/workflows/documentation-check.yml
```

### Step 3: Add the Workflow Configuration

```yaml
name: Documentation Check

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read

jobs:
  validate:
    name: Validate Guide Documentation
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Check main README
        run: test -s README.md

      - name: Check all 16 guide files
        run: |
          for number in $(seq -w 1 16); do
            guide=$(find . -maxdepth 2 -type f -path "./${number}-*/README.md" -print -quit)

            if [ -z "$guide" ]; then
              echo "Missing README for guide $number"
              exit 1
            fi

            if [ ! -s "$guide" ]; then
              echo "Empty README: $guide"
              exit 1
            fi

            echo "Verified: $guide"
          done

      - name: Validation completed
        run: echo "All 16 guide README files are present and non-empty."
```

### Step 4: Understand the Workflow

| Component | Purpose |
|---|---|
| `name` | Names the workflow |
| `push` | Runs after pushes to main |
| `pull_request` | Runs for Pull Requests targeting main |
| `workflow_dispatch` | Allows manual execution |
| `permissions` | Limits repository access |
| `checkout` | Downloads repository code onto the runner |
| `ubuntu-latest` | Selects an Ubuntu runner |
| `seq -w 1 16` | Generates guide numbers 01 through 16 |
| `find` | Locates each guide README |
| `test -s` | Checks that a file exists and is not empty |

The workflow fails if a guide README is missing or empty.

### Step 5: Stage and Review

```bash
git status --short
git add .github/workflows/documentation-check.yml
git diff --cached --check
git diff --cached
```

Review the workflow configuration before committing.

### Step 6: Commit the Workflow

```bash
git commit -m "ci: add documentation validation workflow"
```

### Step 7: Push to GitHub

```bash
git push origin main
```

### Step 8: Verify Workflow Execution

1. Open your GitHub repository.
2. Select the Actions tab.
3. Open the Documentation Check workflow.
4. Select the latest workflow run.
5. Inspect the job and its individual steps.
6. Confirm that all 16 guide files were verified successfully.

A green check indicates that the workflow passed. A red cross indicates that a job failed and needs investigation.

## 12. Understanding Workflow Failures

Common causes include:

| Problem | Possible Solution |
|---|---|
| Invalid YAML | Correct indentation and syntax |
| Missing file | Verify repository paths |
| Permission denied | Review workflow permissions |
| Action failure | Inspect the action logs |
| Incorrect trigger | Check the `on` configuration |
| Failed command | Read the command output and fix the issue |

### Debugging Steps

1. Open the failed workflow run.
2. Select the failed job.
3. Expand the failed step.
4. Read the error message.
5. Correct the workflow or repository file.
6. Commit and push the fix.
7. Inspect the new workflow run.

## 13. Understanding Artifacts and Caching

### Artifacts

Artifacts are files generated during workflow execution that can be stored and downloaded after a job finishes.

Examples:
- Test reports.
- Build packages.
- Log files.

### Caching

Caching stores reusable dependencies or files to reduce repeated installation and build time.

Artifacts preserve workflow outputs; caching helps speed up future workflow runs.

## 14. Best Practices

- Keep workflows focused and readable.
- Use meaningful workflow and job names.
- Choose the appropriate runner.
- Use minimum required permissions.
- Store sensitive values in GitHub Secrets.
- Review third-party actions before using them.
- Avoid unnecessary workflow triggers.
- Inspect failed workflow logs.
- Keep workflows maintained and updated.
- Test changes before depending on automated deployment.

## 15. Common Mistakes

1. Incorrect YAML indentation.
2. Placing workflow files outside `.github/workflows/`.
3. Using an incorrect event name.
4. Forgetting to check out the repository.
5. Hardcoding access tokens.
6. Giving workflows excessive permissions.
7. Ignoring failed workflow runs.
8. Assuming jobs always execute sequentially.
9. Using incorrect file paths.
10. Forgetting to push workflow files to GitHub.

## 16. Practice Exercises

1. Define GitHub Actions.
2. Explain CI, Continuous Delivery, and Continuous Deployment.
3. Explain workflows, events, jobs, steps, and runners.
4. Write a workflow triggered by a push to main.
5. Explain the purpose of `actions/checkout`.
6. Explain the difference between `run` and `uses`.
7. Describe how job dependencies work.
8. Explain GitHub Secrets and workflow permissions.
9. Differentiate artifacts and caching.
10. Explain how to investigate a failed workflow.

## 17. Quick Revision

| Concept | Key Point |
|---|---|
| GitHub Actions | Workflow automation platform |
| CI | Continuous Integration |
| CD | Continuous Delivery or Deployment |
| Workflow | Automated process |
| Event | Trigger for a workflow |
| Job | Group of workflow steps |
| Step | Individual task |
| Runner | Machine executing a job |
| Action | Reusable automation component |
| YAML | Workflow configuration format |
| Secrets | Secure sensitive values |
| Artifacts | Stored workflow outputs |
| Cache | Reusable files for faster workflows |

## Conclusion

GitHub Actions helps automate important development activities and reduces repetitive manual work.

Understanding workflows, triggers, jobs, steps, runners, permissions, and CI/CD practices is essential for modern software development.

The practical documentation validation workflow demonstrates how GitHub Actions can automatically verify a real repository.

---

[← Previous: GitHub Discussions and Collaboration](../13-github-discussions-and-collaboration/README.md) | [Next: Releases, Security and Open Source →](../15-releases-security-and-open-source/README.md)
