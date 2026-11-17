# GitHub Issues and Projects

[← Previous: Git Worktrees and Submodules](../11-git-worktrees-and-submodules/README.md) | [Next: GitHub Discussions and Collaboration →](../13-github-discussions-and-collaboration/README.md)

## Introduction

GitHub Issues and Projects help developers plan, organize, track, and manage work in a repository.

GitHub Issues are used to report bugs, suggest improvements, discuss ideas, and track development tasks.

GitHub Projects provides project-management tools to organize issues and pull requests using boards, tables, fields, and workflows.

This guide explains how to use Issues and Projects as part of a professional GitHub workflow.

## 1. Understanding GitHub Issues

A GitHub Issue represents a task, bug, feature request, or improvement that needs attention.

### Common Uses

- Reporting software bugs.
- Requesting new features.
- Planning documentation improvements.
- Tracking development tasks.
- Discussing proposed changes.
- Organizing work among contributors.

### Important Issue Components

| Component | Purpose |
|---|---|
| Title | Briefly describes the issue |
| Description | Explains the task or problem |
| Assignee | Identifies the responsible person |
| Labels | Categorize the issue |
| Milestone | Groups related issues |
| Comments | Support discussion and updates |
| Projects | Connect the issue to project management |

## 2. Creating a GitHub Issue

Follow these steps:

1. Open the GitHub repository.
2. Navigate to the Issues tab.
3. Click New issue.
4. Enter a descriptive title.
5. Write a detailed description.
6. Add relevant labels.
7. Assign a responsible contributor.
8. Select a milestone if applicable.
9. Submit the issue.

### Example Issue

**Title:** `docs: add Git branching workflow examples`

**Description:**

```markdown
## Description

Improve the Git branching documentation by adding practical examples.

## Tasks

- [ ] Explain the purpose of feature branches.
- [ ] Demonstrate creating and switching branches.
- [ ] Show how to merge a feature branch.
- [ ] Explain branch cleanup after merging.
- [ ] Include a complete practical example.

## Expected Outcome

Beginners should understand and practise a basic feature-branch workflow.
```

A well-written issue should clearly explain what needs to be done and what the expected result should be.

## 3. Managing GitHub Issues

### Assignees

Assignees identify the people responsible for working on an issue.

An issue can be assigned to one or more contributors.

### Labels

Labels categorize issues and make them easier to search and filter.

Common examples:

| Label | Purpose |
|---|---|
| bug | Identifies a software defect |
| enhancement | Suggests an improvement |
| documentation | Documentation-related work |
| question | Requests clarification |
| help wanted | Invites contributions |

### Milestones

Milestones group related issues around a common goal or release.

Example:

**Milestone:** `Documentation Improvements`

A milestone can contain multiple documentation-related issues.

GitHub displays the number of open and closed issues associated with a milestone.

## 4. Linking Issues with Development Work

GitHub allows issues to be referenced from branches, commits, and pull requests.

### Referencing an Issue

Use the issue number with the `#` symbol.

Example:

```text
#10
```

This creates a reference to Issue #10 in supported GitHub contexts.

### Closing Issues Automatically

GitHub supports closing keywords in commit messages and pull request descriptions.

Common closing keywords include:

- `Fixes #10`
- `Closes #10`
- `Resolves #10`

Example pull request description:

```markdown
## Summary

- Add practical branching workflow examples.
- Explain feature branch creation and cleanup.

Closes #10
```

When the pull request is merged into the repository's default branch, GitHub automatically closes the referenced issue.

**Important:** Merely mentioning an issue number does not automatically close it. Use a supported closing keyword when appropriate.

## 5. Understanding the Issue Lifecycle

A typical issue lifecycle follows these steps:

1. Create an issue.
2. Assign responsibility.
3. Add appropriate labels.
4. Associate a milestone if applicable.
5. Discuss and plan the work.
6. Create a dedicated feature branch.
7. Implement the changes.
8. Commit and push the changes.
9. Open a pull request referencing the issue.
10. Review and merge the pull request.
11. Close the issue and update project tracking.

### Issue Lifecycle Diagram

```text
Create Issue
     |
     v
Assign and Label
     |
     v
Plan the Work
     |
     v
Create Feature Branch
     |
     v
Implement Changes
     |
     v
Commit and Push
     |
     v
Create Pull Request
     |
     v
Review and Merge
     |
     v
Close Issue
```

## 6. Understanding GitHub Projects

GitHub Projects provides tools to organize and track development work.

Projects help contributors:

- Plan upcoming tasks.
- Track ongoing work.
- Monitor completed tasks.
- Organize priorities.
- Manage development progress.
- Coordinate team activities.

### Common Project Views

| View | Purpose |
|---|---|
| Board | Displays items in workflow columns |
| Table | Displays items in rows and columns |
| Roadmap | Organizes work along a timeline |

Available views and features depend on the project configuration.

## 7. Understanding Kanban Boards

A Kanban board organizes work according to its current status.

Common statuses include:

| Status | Meaning |
|---|---|
| Todo | Work has not started |
| In Progress | Work is actively being performed |
| Done | Work has been completed |

### Example Kanban Workflow

```text
+----------+       +-------------+       +----------+
|   Todo   | ----> | In Progress | ----> |   Done   |
+----------+       +-------------+       +----------+
```

Items can be moved between statuses as work progresses.

### Benefits of Kanban Boards

- Provides a visual representation of work.
- Makes pending tasks easier to identify.
- Helps track ongoing activities.
- Shows completed work.
- Improves task organization.

## 8. Adding Issues to GitHub Projects

An existing GitHub Issue can be added to a Project.

Follow these steps:

1. Open the GitHub Project.
2. Find the Add item option.
3. Search using the issue number or title.
4. Select the required issue.
5. Verify that it appears on the project board.

An issue can be tracked in a Project without changing its original repository.

### Automatic Project Status Updates

GitHub Projects supports built-in workflows that can automatically update item statuses.

For example, when an issue is closed, an enabled project workflow can automatically move it from Todo to Done.

Always verify the project's workflow configuration when automatic status changes are expected.

**Important:** Issue closure and Project status are separate concepts. Their synchronization depends on the configured automation.

## 9. Understanding Project Fields

Project fields store structured information about project items.

Examples:

| Field | Purpose |
|---|---|
| Status | Tracks the current stage |
| Priority | Indicates importance |
| Start date | Records the planned start |
| Target date | Records the expected completion date |
| Effort | Estimates the required work |

### Priority Field

A priority field can use options such as:

- Urgent
- High
- Medium
- Low

Priority helps contributors identify which tasks should receive attention first.

### Benefits of Project Fields

- Store consistent information.
- Organize project items.
- Filter and sort tasks.
- Improve project visibility.
- Support planning and reporting.

## 10. Practical Exercise: Issue-to-Pull-Request Workflow

Practise the complete workflow using a documentation improvement task.

### Step 1: Create an Issue

Create a GitHub Issue describing a documentation improvement.

### Step 2: Configure the Issue

- Assign yourself.
- Add documentation and enhancement labels.
- Associate a relevant milestone.

### Step 3: Create a Feature Branch

Start from the latest main branch.

```bash
git switch main
git pull origin main
git switch -c docs/issue-branch
```

### Step 4: Make and Commit Changes

Check your changes:

```bash
git status
git diff
```

Stage and commit the modified file:

```bash
git add path/to/changed-file
git commit -m "docs: improve documentation"
```

Replace the example file path and commit message with the actual file and task.

### Step 5: Push the Feature Branch

```bash
git push -u origin docs/issue-branch
```

### Step 6: Create a Pull Request

Create a pull request using:

- Base branch: `main`
- Compare branch: `docs/issue-branch`

Reference the related issue in the description:

```text
Closes #10
```

Replace `10` with the actual issue number.

### Step 7: Review and Merge

1. Inspect the Files changed tab.
2. Review the modifications.
3. Resolve feedback if required.
4. Merge the pull request.

### Step 8: Synchronize and Clean Up

```bash
git switch main
git pull origin main
git branch -d docs/issue-branch
git fetch --prune
```

### Step 9: Track Completed Work

- Verify that the issue is closed.
- Add the issue to a GitHub Project.
- Confirm its project status.
- Review the project's activity history.

## 11. Common Mistakes

| Mistake | Better Practice |
|---|---|
| Vague issue titles | Use clear and specific titles |
| Missing descriptions | Explain the task and expected outcome |
| Incorrect labels | Apply relevant labels |
| Unassigned tasks | Assign a responsible contributor |
| Unlinked pull requests | Reference the related issue |
| Missing closing keywords | Use supported closing keywords |
| Outdated project statuses | Keep project tracking updated |
| Duplicate issues | Search existing issues before creating new ones |
| Merging without review | Inspect changes before merging |

## 12. Best Practices

- Write clear and descriptive issue titles.
- Provide sufficient information in issue descriptions.
- Use relevant labels.
- Assign issues to responsible contributors.
- Use milestones to organize related work.
- Create dedicated branches for individual tasks.
- Reference related issues in pull requests.
- Review changes before merging.
- Keep project statuses accurate.
- Close issues when work is completed.
- Avoid unnecessary project fields and duplicate issues.
- Maintain clear communication with contributors.

## 13. Practice Exercises

1. Create a GitHub Issue for a documentation improvement.
2. Assign yourself to the issue.
3. Add documentation and enhancement labels.
4. Create a milestone and associate the issue with it.
5. Create a dedicated feature branch.
6. Implement a small documentation change.
7. Commit and push the branch.
8. Open a pull request referencing the issue.
9. Review and merge the pull request.
10. Verify that the issue is closed.
11. Create a GitHub Project.
12. Add the issue to the Project.
13. Explore Board and Table views.
14. Understand how Project status automation works.

## 14. Quick Revision

| Term | Meaning |
|---|---|
| Issue | Tracks a task, bug, or improvement |
| Assignee | Person responsible for an issue |
| Label | Categorizes an issue |
| Milestone | Groups related issues |
| Project | Organizes and tracks work |
| Board | Displays work using status columns |
| Field | Stores structured project information |
| Pull Request | Proposes changes for review and merging |
| Closing keyword | Connects merged work with issue closure |
| Automation | Performs configured actions automatically |

## Conclusion

GitHub Issues and Projects help developers organize tasks, collaborate with contributors, and monitor development progress.

Combining issues, labels, milestones, feature branches, pull requests, and project boards creates a structured workflow for managing software development.

Understanding these tools is essential for contributing effectively to professional and open-source projects.

---

[← Previous: Git Worktrees and Submodules](../11-git-worktrees-and-submodules/README.md) | [Next: GitHub Discussions and Collaboration →](../13-github-discussions-and-collaboration/README.md)
