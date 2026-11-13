# Git Tags

Git tags are used to mark specific points in a repository's history. They are commonly used to identify software releases, milestones, and important commits.

Unlike branches, tags generally remain fixed at the commit they reference and do not move forward automatically.

This guide explains Git tags, their types, useful commands, practical examples, and exercises.

## Table of Contents

1. [Introduction to Git Tags](#1-introduction-to-git-tags)
2. [Understanding Types of Tags](#2-understanding-types-of-tags)
3. [Creating Lightweight Tags](#3-creating-lightweight-tags)
4. [Creating Annotated Tags](#4-creating-annotated-tags)
5. [Listing and Inspecting Tags](#5-listing-and-inspecting-tags)
6. [Managing Tags](#6-managing-tags)
7. [Sharing Tags with Remote Repositories](#7-sharing-tags-with-remote-repositories)
8. [Semantic Versioning](#8-semantic-versioning)
9. [Common Git Tag Commands](#9-common-git-tag-commands)
10. [Best Practices](#10-best-practices)
11. [Practice Exercises](#11-practice-exercises)

---

## 1. Introduction to Git Tags

A Git tag is a reference used to identify a specific commit in the repository history.

Tags are especially useful for marking:

- Software releases.
- Stable versions of a project.
- Important development milestones.
- Production-ready code.
- Specific commits that need to be identified later.

For example:

```bash
git tag v1.0.0
```

This creates a tag named `v1.0.0` pointing to the current commit.

### 1.1 Tags vs Branches

| Feature | Branch | Tag |
|---|---|---|
| Purpose | Ongoing development | Mark a specific point |
| Moves automatically | Yes, when new commits are added to it | No |
| New commits | Can be added | Not normally added |
| Common use | Feature development | Releases and milestones |

## 2. Understanding Types of Tags

Git provides two main types of tags:

1. Lightweight tags
2. Annotated tags

### 2.1 Lightweight Tags

A lightweight tag is a simple reference to a commit.

It does not create a separate tag object containing tagger information or a message.

Example:

```bash
git tag v1.0.0
```

Characteristics:

- Simple and quick to create.
- Points directly to a commit.
- Does not contain a tag message.
- Useful for temporary or local labels.

### 2.2 Annotated Tags

An annotated tag is a full Git object containing additional information.

It includes:

- Tag name.
- Tagger information.
- Creation date.
- Tag message.
- Reference to the target commit.

Example:

```bash
git tag -a v1.0.0 -m "First stable release"
```

Characteristics:

- Stores additional metadata.
- Includes a descriptive message.
- Suitable for release management.
- Can be signed using a configured signing key.

### 2.3 Lightweight vs Annotated Tags

| Feature | Lightweight | Annotated |
|---|---|---|
| Tag object | No | Yes |
| Tag message | No | Yes |
| Tagger information | No | Yes |
| Creation date in tag metadata | No | Yes |
| Best suited for | Temporary labels | Official releases |

**Recommendation:** Use annotated tags for important project releases because they provide additional information about the release.

## 3. Creating Lightweight Tags

A lightweight tag can be created using the following command:

```bash
git tag <tag-name>
```

Example:

```bash
git tag practice-lightweight-v1
```

This creates a lightweight tag pointing to the current commit.

### 3.1 Tagging a Specific Commit

You can also create a tag for an older commit using its hash:

```bash
git tag <tag-name> <commit-hash>
```

Example:

```bash
git tag practice-lightweight-v1 1bf4fd5
```

This creates a tag pointing to commit `1bf4fd5`, even if that commit is not the current HEAD.

## 4. Creating Annotated Tags

Use the `-a` option to create an annotated tag.

### 4.1 Basic Syntax

```bash
git tag -a <tag-name> -m "Tag message"
```

Example:

```bash
git tag -a v1.0.0 -m "First stable release"
```

### 4.2 Tagging a Specific Commit

To create an annotated tag for a specific commit:

```bash
git tag -a <tag-name> <commit-hash> -m "Tag message"
```

Example:

```bash
git tag -a practice-annotated-v1 c875011 -m "Annotated tag practice"
```

This creates an annotated tag pointing to commit `c875011`.

## 5. Listing and Inspecting Tags

Git provides several commands to view existing tags.

### 5.1 List All Tags

```bash
git tag
```

This displays all tags in the repository.

### 5.2 Display Tag Messages

```bash
git tag -n
```

This displays tag names along with annotation messages when available. For lightweight tags, Git displays the associated commit subject.

### 5.3 Inspect a Specific Tag

```bash
git show <tag-name>
```

Example:

```bash
git show practice-lightweight-v1
```

For a lightweight tag, Git displays the referenced commit and its details.

For an annotated tag:

```bash
git show practice-annotated-v1
```

Git displays the tag metadata, tag message, referenced commit, and commit details.

### 5.4 Display Tag References

```bash
git show-ref --tags
```

This displays the references and object IDs associated with tags.

**Important:** For annotated tags, the displayed object ID identifies the tag object, not directly the commit it references.

To inspect the commit referenced by a tag:

```bash
git rev-parse <tag-name>^{commit}
```

## 6. Managing Tags

Git also allows developers to delete or replace existing tags.

### 6.1 Delete a Local Tag

```bash
git tag -d <tag-name>
```

Example:

```bash
git tag -d practice-lightweight-v1
```

This deletes the local tag without deleting the commit.

### 6.2 Replace an Existing Tag

To force-update an existing local tag:

```bash
git tag -f <tag-name> <commit-hash>
```

Use this carefully because changing an existing tag can create confusion when other developers already use it.

For official releases, creating a new version tag is generally safer.

### 6.3 Search Tags by Pattern

```bash
git tag -l "v1.*"
```

This lists tags matching the specified pattern.

## 7. Sharing Tags with Remote Repositories

Creating a tag locally does not automatically upload it to GitHub or another remote repository.

Tags must be pushed separately.

### 7.1 Push a Specific Tag

```bash
git push origin <tag-name>
```

Example:

```bash
git push origin v1.0.0
```

This uploads the specified tag to the remote repository.

### 7.2 Push All Tags

```bash
git push origin --tags
```

This pushes all local tags that are not already present on the remote.

### 7.3 Delete a Remote Tag

```bash
git push origin --delete <tag-name>
```

Example:

```bash
git push origin --delete v1.0.0
```

This deletes the specified tag from the remote repository.

**Note:** Deleting a remote tag does not automatically delete the corresponding local tag.

## 8. Semantic Versioning

Semantic Versioning is a commonly used version-numbering system for software releases.

Its standard format is:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
v1.2.3
```

Each part represents:

| Component | Meaning | Example |
|---|---|---|
| MAJOR | Incompatible or breaking changes | v2.0.0 |
| MINOR | New backward-compatible functionality | v1.3.0 |
| PATCH | Backward-compatible bug fixes | v1.2.4 |

### 8.1 Versioning Examples

- `v1.0.0` — First stable release.
- `v1.1.0` — New backward-compatible feature.
- `v1.1.1` — Bug-fix release.
- `v2.0.0` — Release containing breaking changes.

Semantic versioning helps developers and users understand the significance of each release.

## 9. Common Git Tag Commands

| Command | Description |
|---|---|
| `git tag` | List all tags |
| `git tag <name>` | Create a lightweight tag |
| `git tag -a <name> -m "message"` | Create an annotated tag |
| `git tag <name> <hash>` | Tag a specific commit |
| `git tag -a <name> <hash> -m "message"` | Annotate a specific commit |
| `git tag -n` | List tags with descriptions |
| `git show <name>` | Inspect a tag |
| `git show-ref --tags` | Display tag references |
| `git tag -d <name>` | Delete a local tag |
| `git tag -l "pattern"` | Search tags by pattern |
| `git push origin <name>` | Push a specific tag |
| `git push origin --tags` | Push all local tags |
| `git push origin --delete <name>` | Delete a remote tag |

## 10. Best Practices

- Use meaningful and consistent tag names.
- Prefer annotated tags for official releases.
- Follow semantic versioning when appropriate.
- Verify the target commit before creating a tag.
- Inspect tags before pushing them to remote repositories.
- Avoid changing published release tags.
- Keep release tag names consistent across projects.
- Document important release changes.
- Remember that local tags are not automatically pushed to remote repositories.

## 11. Practice Exercises

Complete these exercises in a separate practice repository or dedicated practice branches.

### Exercise 1: Create a Lightweight Tag

- Create a new commit.
- Create a lightweight tag for that commit.
- List all tags.
- Inspect the tag using `git show`.

### Exercise 2: Create an Annotated Tag

- Create an annotated tag.
- Add a meaningful tag message.
- Inspect the tag using `git show`.
- Compare it with a lightweight tag.

### Exercise 3: Tag an Older Commit

- Find an older commit using `git log`.
- Create a tag pointing to that commit.
- Verify the referenced commit.

### Exercise 4: Inspect Tag References

- Run `git show-ref --tags`.
- Compare the object IDs of lightweight and annotated tags.
- Identify the difference between a tag object and its referenced commit.

### Exercise 5: Delete a Local Tag

- Create a temporary tag.
- Delete it using `git tag -d`.
- Verify that it no longer appears in the tag list.

### Exercise 6: Push Tags to GitHub

- Create a new annotated tag.
- Push the specific tag to GitHub.
- Verify that it appears in the remote repository.
- Practise deleting a remote tag in a test repository.

---

## Conclusion

Git tags provide a simple and reliable way to identify important commits in a repository.

Lightweight tags are useful for simple references, while annotated tags provide additional metadata and are preferred for official releases.

Understanding tag creation, inspection, management, and remote sharing helps developers maintain organized project versions and manage software releases professionally.

---

**Learning through practice. Improving through consistency.**

**Previous Guide:** [Git Cherry-pick and Rebase](../07-git-cherry-pick-and-rebase/README.md)