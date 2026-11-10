# 01 — Git and GitHub Fundamentals

## Introduction

Git and GitHub are essential tools used in modern software development. They help developers track changes, manage source code, collaborate with others, and maintain project history.

This guide introduces version control, Git, GitHub, repository creation, installation, and the basic terminology required to begin working with Git.

## 1. What Is Version Control?

Version control is a system that records changes made to files over time.

It allows developers to:
- Track changes in a project.
- Identify who made a particular change.
- Restore earlier versions when necessary.
- Work on different features independently.
- Collaborate with other developers.

### Types of Version Control Systems

1. **Local Version Control:** Stores file versions on a local computer.
2. **Centralized Version Control:** Uses a central server to manage project versions.
3. **Distributed Version Control:** Each user can have a complete copy of the repository and its history.

Git is a distributed version control system.

## 2. What Is Git?

Git is a free, open-source, distributed version control system used to track changes in files and manage project history.

### Why Use Git?

- Tracks changes in source code.
- Maintains a history of commits.
- Supports branching and merging.
- Allows developers to work independently.
- Helps manage and recover previous versions.
- Supports collaborative development.

### Important Git Features

- **Distributed:** Every clone contains the repository's history.
- **Fast:** Most operations are performed locally.
- **Branching:** Developers can work on separate lines of development.
- **Reliable:** Git uses content-based hashing to identify and protect repository objects.

## 3. What Is GitHub?

GitHub is a cloud-based platform for hosting Git repositories and collaborating on software projects.

It provides tools for:
- Hosting repositories online.
- Sharing source code.
- Reviewing code changes.
- Managing issues and project discussions.
- Creating and reviewing pull requests.
- Collaborating with other developers.

GitHub uses Git for version control.

## 4. Git vs GitHub

| Git | GitHub |
|---|---|
| Version control system | Online development platform |
| Installed locally | Accessed through a website or application |
| Tracks changes and commits | Hosts Git repositories |
| Works without an internet connection for local operations | Requires internet access for online collaboration |
| Developed by the Git project | Owned by Microsoft |

**In simple terms:** Git manages version history, while GitHub provides an online platform to host and collaborate on Git repositories.

## 5. Why Is Version Control Important?

Version control is useful because it:

1. Prevents the loss of important project history.
2. Makes it easier to identify and fix mistakes.
3. Supports teamwork.
4. Allows multiple features to be developed independently.
5. Provides a record of project development.
6. Helps maintain organized and reliable code.

## 6. Installing Git on Windows

Git can be installed on Windows using Git for Windows.

### Installation Steps

1. Visit the official Git website: https://git-scm.com/
2. Download the Windows installer.
3. Run the downloaded installer.
4. Follow the installation wizard.
5. Complete the installation using the selected configuration.
6. Open Git Bash to access the Git command-line interface.

### Verify Installation

Run:

```bash
git --version
```

Example output:

```text
git version 2.x.x
```

The exact version depends on the installed release.

## 7. Configuring Git

After installing Git, configure your identity.

Git uses this information to identify the author of commits.

### Configure Username

```bash
git config --global user.name "Your Name"
```

### Configure Email

```bash
git config --global user.email "your-email@example.com"
```

Use an email address associated with your GitHub account if you want your commits to be associated with that account.

### Verify Configuration

```bash
git config --global user.name
git config --global user.email
```

To display all global configuration settings:

```bash
git config --global --list
```

**Note:** Global configuration applies to repositories for your operating-system user. Repository-specific configuration can override it.

## 8. Understanding Git Repositories

A repository is a location where Git tracks project files and their history.

There are two common ways to start working with a repository:

- Initialize a new local repository.
- Clone an existing repository.

### Initialize a Local Repository

Navigate to a project directory and run:

```bash
git init
```

This initializes Git tracking in the current directory by creating a `.git` directory.

### Clone an Existing Repository

Cloning creates a local copy of an existing remote repository.

Command syntax:

```bash
git clone <repository-url>
```

Example:

```bash
git clone https://github.com/username/project-name.git
```

Replace the example URL with the actual repository URL.

## 9. Creating a Repository on GitHub

### Steps

1. Sign in to GitHub.
2. Select the **New repository** option.
3. Enter a repository name.
4. Add a short description.
5. Choose Public or Private visibility.
6. Optionally initialize the repository with a README file.
7. Select a license if required.
8. Click **Create repository**.

After creation, GitHub displays the repository page and provides its remote URL.

## 10. Understanding the `.git` Directory

When a Git repository is initialized or cloned, Git maintains a hidden directory named `.git`.

This directory stores repository metadata, configuration, references, and object data required to manage version history.

To view hidden files and directories in Git Bash:

```bash
ls -a
```

Example:

```text
.  ..  .git  README.md
```

**Important:** Avoid manually modifying or deleting the `.git` directory. Doing so can damage repository metadata or make the repository unusable.

## 11. Important Git Terminology

| Term | Meaning |
|---|---|
| Repository | Project directory tracked by Git |
| Working Directory | Files currently being worked on |
| Commit | A recorded snapshot of changes |
| Branch | An independent line of development |
| Clone | A local copy of a remote repository |
| Remote | A reference to another repository |
| Origin | Conventional name for the default remote |
| GitHub | Platform for hosting and collaborating on Git repositories |
| `.git` | Hidden directory containing Git repository data |

## 12. Common Beginner Mistakes

- Installing Git but not verifying the installation.
- Forgetting to configure username and email.
- Confusing Git with GitHub.
- Running commands in the wrong directory.
- Accidentally deleting the `.git` directory.
- Assuming that creating a GitHub repository automatically creates a local working copy.
- Sharing passwords or access tokens in public repositories.

## 13. Best Practices

- Use a professional Git username and commit identity.
- Understand commands before executing them.
- Keep repositories organized.
- Write meaningful commit messages.
- Never upload passwords, API keys, or private credentials.
- Read command output carefully.
- Use GitHub to document and share meaningful work.

## 14. Hands-On Practice Exercises

1. Install Git on your computer.
2. Verify the installation using `git --version`.
3. Configure your Git username and email.
4. Verify your global Git configuration.
5. Create a new repository on GitHub.
6. Clone the repository to your computer.
7. Open the cloned repository in Git Bash.
8. Use `ls -a` to locate the `.git` directory.

## 15. Quick Revision

- Git is a distributed version control system.
- GitHub is an online platform for hosting Git repositories.
- Version control tracks changes to files over time.
- `git --version` checks the installed Git version.
- `git config` manages Git configuration.
- `git init` initializes a local repository.
- `git clone` copies an existing repository.
- `.git` stores the repository's internal data.

---

**Next Guide:** [02 — Basic Git Workflow](../02-basic-git-workflow/README.md)