# Releases, Security and Open Source


## 1. Introduction

A professional Git and GitHub workflow does not end when code is pushed to a repository.

Projects also need:
- Version management.
- Stable releases.
- Release notes and changelogs.
- Appropriate open-source licenses.
- Security policies.
- Vulnerability reporting.
- Responsible collaboration.

This guide explains how to manage releases, protect repositories, and contribute to open-source projects.

## 2. Understanding Git Tags

A Git tag is a reference to a specific commit.

Tags are commonly used to mark important milestones, such as:
- Version 1.0.0.
- First stable release.
- Major feature release.
- Production deployment.

Unlike branches, tags normally do not move when new commits are added.

### 2.1 Lightweight tags

A lightweight tag is simply a name pointing to a commit.

Create a tag:

```bash
git tag v1.0.0
```

List tags:

```bash
git tag
```

Inspect a tag:

```bash
git show v1.0.0
```

### 2.2 Annotated tags

An annotated tag stores additional information:
- Tagger name.
- Tagging date.
- Tag message.
- Optional cryptographic signature.

Create an annotated tag:

```bash
git tag -a v1.0.0 -m "First stable release"
```

Inspect it:

```bash
git show v1.0.0
```

Annotated tags are generally preferred for official releases.

### 2.3 Tag a specific commit

You can tag an older commit by providing its hash:

```bash
git tag -a v1.0.0 COMMIT_HASH -m "First stable release"
```

Replace `COMMIT_HASH` with the actual commit hash.

### 2.4 Push tags to GitHub

Push one tag:

```bash
git push origin v1.0.0
```

Push all local tags:

```bash
git push origin --tags
```

Remember that a normal `git push` does not necessarily push every local tag.

### 2.5 Delete tags

Delete a local tag:

```bash
git tag -d v1.0.0
```

Delete a remote tag:

```bash
git push origin --delete v1.0.0
```

Be careful when deleting published release tags because other users may already depend on them.

## 3. Semantic Versioning

Semantic Versioning provides a consistent way to number software releases.

The standard format is:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
v2.4.1
```

| Part | Meaning | Example |
|---|---|---|
| MAJOR | Incompatible or breaking changes | 2.0.0 |
| MINOR | New backward-compatible features | 2.4.0 |
| PATCH | Backward-compatible bug fixes | 2.4.1 |

### 3.1 Version examples

Suppose a project is currently at version `1.2.3`.

- A bug fix may produce `1.2.4`.
- A new backward-compatible feature may produce `1.3.0`.
- A breaking API change may produce `2.0.0`.

### 3.2 Pre-release versions

Pre-release identifiers can indicate that a version is not yet stable.

Examples:

```text
v2.0.0-alpha.1
v2.0.0-beta.1
v2.0.0-rc.1
```

Common meanings:
- Alpha: early development.
- Beta: broader testing.
- Release candidate (rc): potentially ready for final release.

Semantic Versioning is a convention. A project should document how it applies version numbers.

## 4. GitHub Releases

A GitHub Release represents a specific version of a project.

Releases are commonly associated with Git tags.

A release can include:
- Version number.
- Release title.
- Release description.
- Changelog.
- Source code archives.
- Downloadable assets.
- Pre-release or latest-release status.

### 4.1 Creating a GitHub Release

1. Open your GitHub repository.
2. Navigate to the **Releases** section.
3. Select **Create a new release**.
4. Choose an existing tag or create a new tag.
5. Enter a release title.
6. Write release notes.
7. Attach downloadable files if required.
8. Publish the release.

### 4.2 Release notes

Good release notes explain what changed.

Example:

```text
Version 1.0.0

Features:
- Added user registration.
- Added login functionality.
- Added profile management.

Improvements:
- Improved application performance.

Bug Fixes:
- Fixed login validation issue.
- Fixed incorrect error messages.
```

### 4.3 Draft releases

A draft release is not publicly published.

It allows maintainers to prepare release information before making it available.

Use draft releases when:
- Release notes need review.
- Assets are still being prepared.
- Final testing is incomplete.

## 5. Changelogs

A changelog is a document that records important changes made to a project.

It helps users and contributors understand how the project has evolved.

A common filename is:

```text
CHANGELOG.md
```

### 5.1 Example changelog

```markdown
# Changelog

All notable changes to this project are documented here.

## [1.0.0] - 2026-11-17

### Added
- Initial project release.
- Git fundamentals documentation.
- Git branching examples.

### Changed
- Improved documentation structure.

### Fixed
- Corrected navigation links.
```

### 5.2 Common changelog categories

| Category | Purpose |
|---|---|
| Added | New features |
| Changed | Changes to existing functionality |
| Deprecated | Features that may be removed later |
| Removed | Removed features |
| Fixed | Bug fixes |
| Security | Security-related improvements |

### 5.3 Changelog best practices

- Use clear descriptions.
- Organize entries by version.
- Include release dates.
- Highlight breaking changes.
- Mention important security fixes.
- Avoid documenting every insignificant internal change.

## 6. Open-Source Software

Open-source software makes its source code available under a license that grants users specified permissions.

Depending on the license, users may be allowed to:
- Use the software.
- Study the source code.
- Modify the software.
- Distribute copies.
- Distribute modified versions.

Open source does not mean that software has no copyright or no rules.

### 6.1 Why licenses matter

A license explains the legal permissions and obligations associated with using and distributing software.

Without an explicit license, users generally do not automatically receive permission to reuse copyrighted code.

### 6.2 Common open-source licenses

| License | Main characteristics |
|---|---|
| MIT | Permissive, simple, allows broad reuse with copyright and license notice |
| Apache 2.0 | Permissive and includes explicit patent-related provisions |
| GPL 3.0 | Copyleft license requiring covered distributed derivative works to follow GPL terms |

### 6.3 Choosing a license

Consider:
- Whether commercial use is allowed.
- Whether modified versions can be distributed.
- Whether source code must be shared.
- Whether patent provisions are important.
- Whether the license is compatible with project dependencies.

Always read the actual license terms before selecting one.

### 6.4 Adding a license to GitHub

1. Open the repository.
2. Select **Add file**.
3. Choose **Create new file**.
4. Name the file `LICENSE`.
5. Use GitHub's license template option if available.
6. Review the license.
7. Commit the file.

## 7. Repository Security

Repository security protects source code, credentials, dependencies, and project users.

Security is important for personal projects, college projects, professional repositories, and open-source projects.

### 7.1 Never commit secrets

Never store sensitive information directly in a public repository.

Examples include:
- Passwords.
- API keys.
- Access tokens.
- Private SSH keys.
- Database credentials.
- Cloud service credentials.
- Private certificates.

### 7.2 Use environment variables

Instead of hardcoding credentials, load them from environment variables.

Example in Python:

```python
import os

api_key = os.getenv("API_KEY")
```

Example `.env` file:

```text
API_KEY=your-secret-key
```

Add sensitive environment files to `.gitignore`:

```gitignore
.env
.env.*
!.env.example
```

An `.env.example` file can contain placeholder values without exposing real credentials.

### 7.3 What if a secret is committed?

If a secret is accidentally pushed:

1. Revoke or rotate the exposed credential immediately.
2. Generate a replacement credential.
3. Update applications that use it.
4. Remove the secret from the repository.
5. Review repository history and access logs.
6. Follow the provider's instructions for handling exposed credentials.

Important: Deleting a secret in a later commit does not remove it from earlier Git history.

History rewriting may be necessary, but it does not replace revoking the exposed credential.

## 8. SECURITY.md

A `SECURITY.md` file explains how people can responsibly report security vulnerabilities.

It gives security researchers a clear reporting process.

### 8.1 Creating SECURITY.md

Create a file named:

```text
SECURITY.md
```

Example:

```markdown
# Security Policy

## Supported Versions

Security updates are provided for the latest maintained version.

## Reporting a Vulnerability

If you discover a security vulnerability, please report it
privately to the repository maintainers.

Do not publish sensitive vulnerability details in a public issue.

## Response Process

The maintainers will review the report and determine
the appropriate response.

Please provide:
- A description of the vulnerability.
- Steps to reproduce the issue.
- Potential impact.
- Any relevant evidence.

Thank you for helping keep this project secure.
```

### 8.2 Benefits of SECURITY.md

- Provides a responsible reporting process.
- Helps researchers contact maintainers.
- Avoids exposing vulnerability details publicly.
- Communicates supported versions.
- Improves repository security practices.

## 9. GitHub Security Features

GitHub provides security tools that help identify and manage vulnerabilities.

Availability can depend on repository visibility, plan, and organization settings.

### 9.1 Dependabot

Dependabot helps identify outdated or vulnerable dependencies.

It can:
- Check dependency versions.
- Detect known vulnerabilities.
- Suggest dependency updates.
- Create pull requests for supported updates.

### 9.2 Dependabot alerts

Dependabot alerts notify maintainers when dependencies have known security vulnerabilities.

Maintainers should:
1. Review the affected dependency.
2. Understand the vulnerability.
3. Check the suggested update.
4. Test the application.
5. Merge a suitable fix.

### 9.3 Secret scanning

Secret scanning helps detect exposed credentials in repositories.

It can identify supported secret patterns and alert maintainers.

Some supported integrations can also help prevent secrets from being pushed.

### 9.4 Code scanning and CodeQL

Code scanning analyzes source code for potential security vulnerabilities and coding errors.

CodeQL is one technology GitHub supports for code analysis.

It can help identify issues such as:
- SQL injection.
- Cross-site scripting.
- Unsafe data handling.
- Certain authentication and authorization weaknesses.

### 9.5 Security advisories

GitHub Security Advisories provide a way to privately discuss, coordinate, and disclose vulnerabilities affecting a project.

They support responsible vulnerability management.

## 10. Repository Security Settings

GitHub repositories provide settings that help maintainers control access and collaboration.

Important security practices include:

- Enable two-factor authentication on your GitHub account.
- Use strong, unique passwords.
- Review collaborator permissions.
- Protect important branches.
- Require pull request reviews where appropriate.
- Use appropriate workflow permissions.
- Store workflow credentials in GitHub Secrets.
- Review third-party GitHub Actions before using them.
- Keep dependencies updated.
- Remove access that is no longer needed.

### 10.1 Branch protection

Branch protection rules help prevent unsafe changes to important branches.

Depending on the repository and available features, rules can require:
- Pull requests before merging.
- Approval from reviewers.
- Passing status checks.
- Conversation resolution.
- Restrictions on direct pushes.

For a team project, protecting `main` helps maintain code quality.

## 11. Contributing to Open Source

Contributing to open-source projects is an important way to develop practical development and collaboration skills.

Contributions can include:
- Fixing bugs.
- Improving documentation.
- Adding features.
- Writing tests.
- Reporting issues.
- Reviewing pull requests.
- Improving accessibility.

### 11.1 Find a suitable repository

Choose a project that:
- Matches your interests.
- Has understandable documentation.
- Has contribution guidelines.
- Has issues suitable for beginners.
- Is actively maintained.

Read the repository's `README.md`, `CONTRIBUTING.md`, and `CODE_OF_CONDUCT.md` when available.

### 11.2 Fork a repository

A fork creates your own GitHub copy of another user's repository.

Steps:

1. Open the original GitHub repository.
2. Click **Fork**.
3. Select your GitHub account.
4. Wait for GitHub to create the fork.

### 11.3 Clone your fork

Clone the fork to your computer:

```bash
git clone https://github.com/YOUR-USERNAME/REPOSITORY.git
```

Move into the project directory:

```bash
cd REPOSITORY
```

### 11.4 Add the original repository as upstream

The `origin` remote normally points to your fork.

Add the original repository as `upstream`:

```bash
git remote add upstream https://github.com/ORIGINAL-OWNER/REPOSITORY.git
```

Check the remotes:

```bash
git remote -v
```

### 11.5 Synchronize your fork

Fetch changes from the original repository:

```bash
git fetch upstream
```

Update your local main branch:

```bash
git switch main
git merge upstream/main
```

Push the updated branch to your fork:

```bash
git push origin main
```

The exact default branch name may differ between repositories.

### 11.6 Create a feature branch

Never make an unrelated contribution directly on your main branch.

Create a descriptive branch:

```bash
git switch -c docs/improve-installation-guide
```

### 11.7 Make and commit your changes

Check your changes:

```bash
git status
git diff
```

Stage the intended files:

```bash
git add README.md
```

Commit:

```bash
git commit -m "docs: improve installation instructions"
```

### 11.8 Push your branch

```bash
git push -u origin docs/improve-installation-guide
```

### 11.9 Create a pull request

1. Open your fork on GitHub.
2. Select **Compare & pull request**.
3. Confirm the original repository is the base repository.
4. Confirm your fork is the head repository.
5. Write a clear title and description.
6. Explain what changed and why.
7. Submit the pull request.

The project maintainers may review your changes and request modifications.

## 12. Responsible Collaboration

Open-source collaboration requires communication, respect, and attention to project guidelines.

### 12.1 Before contributing

- Read the project documentation.
- Check existing issues and pull requests.
- Avoid duplicating existing work.
- Understand the contribution guidelines.
- Ask questions when requirements are unclear.

### 12.2 During contribution

- Keep changes focused.
- Write meaningful commit messages.
- Test your changes.
- Update documentation when necessary.
- Respond respectfully to review feedback.

### 12.3 After submitting a pull request

- Monitor review comments.
- Make requested improvements.
- Push additional commits when required.
- Avoid unnecessary force pushes.
- Wait for maintainer approval.
- Follow the project's merge process.

A successful contribution is not measured only by the number of commits. Quality, usefulness, and collaboration matter.

## 13. Practical Exercise: Create a Release

Practice creating a version tag and understanding the release process.

Use a test repository or a suitable project where you have permission to publish a release.

### Step 1: Check the current repository state

```bash
git status
git log --oneline -5
```

Make sure your working tree is clean.

### Step 2: Create an annotated tag

```bash
git tag -a v1.0.0 -m "First stable release"
```

### Step 3: Verify the tag

```bash
git tag
git show v1.0.0
```

### Step 4: Push the tag

```bash
git push origin v1.0.0
```

### Step 5: Publish a GitHub Release

1. Open the repository on GitHub.
2. Navigate to **Releases**.
3. Select **Create a new release**.
4. Select the `v1.0.0` tag.
5. Add a release title.
6. Write release notes.
7. Publish the release.

### Expected outcome

You should understand how Git tags and GitHub Releases work together.

Do not create a version tag on your learning repository unless you intend to maintain that release.

## 14. Practical Exercise: Add Security Documentation

Practice adding security documentation to a repository you control.

### Step 1: Create SECURITY.md

```bash
touch SECURITY.md
```

### Step 2: Add a security policy

Include:
- Supported versions.
- Vulnerability reporting instructions.
- Responsible disclosure guidance.
- Expected response process.

### Step 3: Review the file

```bash
cat SECURITY.md
```

### Step 4: Stage and commit

```bash
git add SECURITY.md
git commit -m "docs: add repository security policy"
```

### Step 5: Push the changes

```bash
git push origin main
```

### Expected outcome

Your repository should contain a documented process for reporting security vulnerabilities.

## 15. Common Mistakes

| Mistake | Why it is a problem | Better practice |
|---|---|---|
| Using only lightweight tags for official releases | Provides limited tag metadata | Prefer annotated tags |
| Forgetting to push tags | Release tags may remain local | Push the required tags |
| Using inconsistent version numbers | Makes releases difficult to understand | Follow Semantic Versioning where appropriate |
| Publishing without release notes | Users cannot understand changes | Write clear release notes |
| Committing API keys | Exposes sensitive credentials | Use environment variables and secrets |
| Assuming deletion removes a leaked secret | The secret may remain in Git history | Revoke or rotate it immediately |
| Ignoring dependency alerts | Vulnerable dependencies may remain | Review and update affected dependencies |
| Contributing without reading guidelines | Changes may not follow project requirements | Read contribution documentation |
| Opening unclear pull requests | Review becomes difficult | Explain changes and testing clearly |
| Making unrelated changes in one PR | Increases review complexity | Keep contributions focused |

## 16. Best Practices

- Use annotated tags for official releases.
- Follow a consistent versioning strategy.
- Write meaningful release notes.
- Maintain a changelog for projects that benefit from one.
- Choose an appropriate open-source license.
- Never commit passwords, tokens, or private keys.
- Rotate exposed credentials immediately.
- Maintain a `SECURITY.md` file when appropriate.
- Review dependency vulnerabilities.
- Enable suitable GitHub security features.
- Protect important branches.
- Review repository permissions regularly.
- Read contribution guidelines before contributing.
- Keep pull requests small and focused.
- Communicate respectfully with maintainers.
- Test changes before submitting them.

## 17. Revision Questions

1. What is a Git tag?
2. What is the difference between lightweight and annotated tags?
3. How do you create an annotated tag?
4. How do you push a tag to GitHub?
5. What is Semantic Versioning?
6. What do MAJOR, MINOR, and PATCH represent?
7. What is a pre-release version?
8. What is a GitHub Release?
9. Why are release notes important?
10. What is a changelog?
11. Why does a project need an open-source license?
12. What is the difference between MIT, Apache 2.0, and GPL 3.0?
13. Why should secrets never be committed?
14. What should you do if an API key is exposed?
15. What is the purpose of `SECURITY.md`?
16. What is Dependabot?
17. What is secret scanning?
18. What is CodeQL?
19. What is the purpose of a GitHub Security Advisory?
20. What is the difference between `origin` and `upstream`?
21. What is a fork?
22. Why should contributors create feature branches?
23. What information should a good pull request contain?
24. Why should contribution guidelines be followed?

## 18. Key Takeaways

- Git tags identify important commits and project milestones.
- Annotated tags are useful for official releases.
- Semantic Versioning provides a consistent version-numbering convention.
- GitHub Releases help distribute and explain project versions.
- Changelogs document important project changes.
- Open-source licenses define permissions and obligations.
- Repository security protects code and sensitive information.
- Exposed credentials must be revoked or rotated immediately.
- `SECURITY.md` documents vulnerability reporting procedures.
- GitHub provides tools to help detect vulnerabilities and manage dependencies.
- Forks and pull requests support open-source collaboration.
- Responsible contributions require testing, communication, and respect.

---

## Navigation

**Previous Guide:** [GitHub Actions and CI/CD](../14-github-actions-and-ci-cd/README.md)

**Next Guide:** [Professional Git Workflow](../16-professional-git-workflow/README.md)

**Back to Main README:** [Git & GitHub Mastery](../README.md)
