# Git & GitHub — Practical Guide & Cheat Sheet

Git is a **distributed version control system** used to track changes in source code and collaborate with other developers.

This README covers the most commonly used Git commands, from creating a repository and working with branches to committing, pushing, merging, stashing, rebasing, and resolving common problems.

---

## 📚 Table of Contents

1. [Git Basics](#-git-basics)
2. [Git Workflow](#-git-workflow)
3. [Setup & Configuration](#-setup--configuration)
4. [Initialize & Clone](#-initialize--clone)
5. [Check Repository Status](#-check-repository-status)
6. [Stage Changes](#-stage-changes)
7. [Commit Changes](#-commit-changes)
8. [Branches](#-branches)
9. [Switch Branches](#-switch-branches)
10. [Remote Repositories](#-remote-repositories)
11. [Fetch, Pull & Push](#-fetch-pull--push)
12. [Merge](#-merge)
13. [Git Diff](#-git-diff)
14. [Git Log](#-git-log)
15. [Stash](#-git-stash)
16. [Rebase](#-git-rebase)
17. [Reset](#-git-reset)
18. [Undo Changes](#-undo-changes)
19. [Rename & Delete Files](#-rename--delete-files)
20. [.gitignore](#-gitignore)
21. [Typical Developer Workflow](#-typical-developer-workflow)
22. [Git Branch + PR Workflow](#-git-branch--pr-workflow)
23. [Useful Commands](#-useful-commands)
24. [Important Git Concepts](#-important-git-concepts)

---

# 🔰 Git Basics

### What is Git?

Git tracks changes made to files over time.

Instead of having:

```text
project-final
project-final-2
project-final-latest
project-final-latest-new
```

Git keeps a history of changes:

```text
Commit 1 → Commit 2 → Commit 3 → Commit 4
```

You can move between versions, compare changes, create separate branches, and collaborate with other developers.

### Git vs GitHub

| Git                    | GitHub                         |
| ---------------------- | ------------------------------ |
| Version control system | Cloud platform                 |
| Runs locally           | Hosted online                  |
| Tracks code history    | Hosts Git repositories         |
| Works without internet | Usually used for collaboration |
| Command-line/tool      | Web + Git hosting service      |

**Git is the tool. GitHub is one platform that hosts Git repositories.**

---

# 🔄 Git Workflow

The basic Git workflow is:

```text
Working Directory
       ↓
     git add
       ↓
Staging Area
       ↓
   git commit
       ↓
 Local Repository
       ↓
    git push
       ↓
Remote Repository
```

For example:

```bash
git status

git add .

git commit -m "Add login validation"

git push origin feature/login-validation
```

---

# ⚙️ Setup & Configuration

Git needs your identity so commits can be associated with you.

## Set username

```bash
git config --global user.name "Your Name"
```

Example:

```bash
git config --global user.name "Jayesh"
```

## Set email

```bash
git config --global user.email "your-email@example.com"
```

## Check configuration

```bash
git config --list
```

## Set automatic colors

```bash
git config --global color.ui auto
```

### Important

The email configured in Git should generally match the email associated with your GitHub account if you want your commits to be properly associated with that account.

---

# 📁 Initialize & Clone

## `git init`

Initializes an existing folder as a Git repository.

```bash
git init
```

Example:

```bash
mkdir my-project
cd my-project
git init
```

Git creates a hidden `.git` directory that contains repository information.

---

## `git clone`

Downloads an existing remote repository to your computer.

```bash
git clone <repository-url>
```

Example:

```bash
git clone https://github.com/company/project.git
```

Then:

```bash
cd project
```

### When to use

Use `git clone` when a repository already exists on GitHub/GitLab/Bitbucket and you want a local copy.

---

# 🔍 Check Repository Status

## `git status`

One of the most important Git commands.

```bash
git status
```

It tells you:

* Which branch you're currently on
* Which files are modified
* Which files are staged
* Which files are untracked
* Whether your branch is ahead/behind the remote

Example:

```text
On branch feature/login

Changes not staged for commit:
  modified: login.java

Untracked files:
  test.java
```

---

# 📦 Stage Changes

Before committing, changes must be added to the staging area.

## Stage one file

```bash
git add filename
```

Example:

```bash
git add Login.java
```

## Stage multiple files

```bash
git add file1.java file2.java
```

## Stage all changes

```bash
git add .
```

### Important

`git add .` stages changes in the current directory and its subdirectories.

Always check:

```bash
git status
```

before committing.

---

# ↩️ Unstage Changes

## `git reset <file>`

Removes a file from the staging area while keeping your changes in the working directory.

```bash
git reset Login.java
```

Your code changes are **not deleted**.

Only the staging status changes.

---

# 💾 Commit Changes

## `git commit`

Creates a snapshot of staged changes.

```bash
git commit -m "Add login validation"
```

A good commit message should describe what was changed.

### Good

```bash
git commit -m "Add account onboarding validation"
```

### Bad

```bash
git commit -m "changes"
```

```bash
git commit -m "done"
```

```bash
git commit -m "final"
```

---

# 🌿 Branches

Branches allow developers to work on features independently without directly changing the main branch.

Typical structure:

```text
main
 │
 ├── feature/login
 ├── feature/payment
 └── bugfix/account-validation
```

---

## List branches

```bash
git branch
```

The `*` indicates the current branch.

Example:

```text
  main
* feature/login
  bugfix/payment
```

---

## Create a branch

```bash
git branch feature/login
```

This creates the branch but does **not** switch to it.

---

## Create and switch to a branch

Modern Git:

```bash
git switch -c feature/login
```

Older/common syntax:

```bash
git checkout -b feature/login
```

This is usually what you want when starting new work.

---

# 🔀 Switching Branches

Modern command:

```bash
git switch main
```

or:

```bash
git switch feature/login
```

Older command:

```bash
git checkout main
```

### Recommendation

For new Git usage, prefer:

```bash
git switch
```

for changing branches.

---

# 🌐 Remote Repositories

A remote repository is the online repository associated with your local repository.

Common remote names:

```text
origin
upstream
```

---

## Add a remote

```bash
git remote add origin <repository-url>
```

Example:

```bash
git remote add origin https://github.com/company/project.git
```

---

## Check remote

```bash
git remote -v
```

Example:

```text
origin  https://github.com/company/project.git (fetch)
origin  https://github.com/company/project.git (push)
```

---

# ⬇️ Fetch, Pull & Push

These three commands are extremely important.

---

## `git fetch`

Downloads information about changes from the remote repository without modifying your current working branch.

```bash
git fetch origin
```

Think:

```text
Remote Repository
       ↓
     fetch
       ↓
Local knowledge of remote changes
```

Your current files are not automatically changed.

---

## `git pull`

Downloads remote changes and integrates them into your current branch.

```bash
git pull
```

Or:

```bash
git pull origin main
```

Conceptually:

```text
git pull ≈ git fetch + integration
```

The integration may be performed using merge or rebase depending on configuration/options.

---

## `git push`

Uploads your local commits to the remote repository.

```bash
git push origin feature/login
```

For the first push of a new branch:

```bash
git push -u origin feature/login
```

The `-u` sets the upstream tracking relationship.

After that, you can often simply use:

```bash
git push
```

---

# 🔀 Merge

`git merge` combines changes from another branch into your current branch.

Suppose:

```text
main
  \
   feature/login
```

You finished your feature and want to merge it into `main`.

First:

```bash
git switch main
```

Then:

```bash
git merge feature/login
```

Now the feature branch's changes are integrated into `main`.

### Important

The branch you are currently on receives the merge.

Example:

```bash
git switch main
git merge feature/login
```

means:

> Merge `feature/login` INTO `main`.

---

# 🔎 Git Diff

## See unstaged changes

```bash
git diff
```

Shows changes that have been made but haven't been staged.

---

## See staged changes

```bash
git diff --staged
```

Shows changes that are currently staged and ready for commit.

---

## Compare two branches

```bash
git diff branchB...branchA
```

Shows differences between the branch histories from their common ancestor.

---

# 📜 Git Log

## View commit history

```bash
git log
```

Useful compact version:

```bash
git log --oneline
```

Example:

```text
a81f32c Add login validation
b72e123 Fix account API
c83d912 Update README
```

---

## Show commits unique to a branch

```bash
git log branchB..branchA
```

This shows commits reachable from `branchA` that are not reachable from `branchB`.

---

## Track file history

```bash
git log --follow filename
```

Useful when a file has been renamed.

Example:

```bash
git log --follow LoginService.java
```

---

# 🧳 Git Stash

Sometimes you have unfinished changes but need to switch branches.

Instead of committing incomplete work, you can temporarily store it.

## Save changes

```bash
git stash
```

Your working directory becomes clean.

You can now switch branches.

---

## View stashes

```bash
git stash list
```

Example:

```text
stash@{0}: WIP on feature/login
stash@{1}: WIP on bugfix/payment
```

---

## Restore latest stash

```bash
git stash pop
```

This restores the most recent stash and removes it from the stash list if successfully applied.

---

## Remove a stash

```bash
git stash drop
```

Be careful because this discards the selected stash.

---

# 🔄 Git Rebase

Rebase moves/replays your branch commits on top of another branch.

Example:

```text
Before:

main:     A---B---C
               \
feature:        D---E
```

After:

```text
main:     A---B---C
                   \
feature:            D'---E'
```

Command:

```bash
git switch feature
git rebase main
```

### Why use rebase?

It can create a cleaner, more linear project history.

### Important warning

Avoid rebasing commits that other developers are already depending on unless your team has agreed on the workflow.

---

# ↩️ Git Reset

Reset moves your current branch pointer and can change the staging area and working directory depending on the option.

---

## Soft reset

```bash
git reset --soft HEAD~1
```

Removes the latest commit but keeps the changes staged.

Useful when you want to modify the previous commit.

---

## Mixed reset

```bash
git reset HEAD~1
```

or:

```bash
git reset --mixed HEAD~1
```

Removes the commit and unstages the changes, while keeping the files modified.

---

## Hard reset

```bash
git reset --hard HEAD~1
```

Removes the commit and discards corresponding working-tree changes.

### ⚠️ WARNING

`git reset --hard` can permanently discard uncommitted work.

Use it carefully.

---

# ↩️ Undo Changes

## Discard changes to a file

Modern Git:

```bash
git restore filename
```

Example:

```bash
git restore Login.java
```

This discards unstaged changes to that file.

---

## Unstage a file

```bash
git restore --staged Login.java
```

The file remains modified but is removed from staging.

---

# 🗑️ Delete a File

```bash
git rm filename
```

This:

1. Deletes the file
2. Stages the deletion

Example:

```bash
git rm oldfile.java
```

---

# ✏️ Rename / Move a File

```bash
git mv old-name.java new-name.java
```

Git stages the rename.

---

## View rename history

```bash
git log --stat -M
```

---

# 🔍 Git Show

Show information about a particular commit/object.

```bash
git show <commit-SHA>
```

Example:

```bash
git show a81f32c
```

A commit SHA is the identifier associated with a Git commit.

---

# 🚫 .gitignore

`.gitignore` tells Git which files/folders should normally not be tracked.

Example `.gitignore`:

```gitignore
logs/
*.notes
target/
*.class
.env
node_modules/
```

Common things to ignore:

* Build files
* Logs
* Environment files
* IDE-generated files
* Dependencies
* Temporary files
* Secrets

### Example

```text
project/
│
├── src/
├── target/          ← ignored
├── logs/            ← ignored
├── .env             ← ignored
└── .gitignore
```

---

# 🧠 Git Areas You Must Understand

Git becomes much easier once you understand these four concepts:

```text
Working Directory
       ↓
    git add
       ↓
Staging Area
       ↓
   git commit
       ↓
Local Repository
       ↓
    git push
       ↓
Remote Repository
```

### Working Directory

Your actual project files.

### Staging Area

Files selected for the next commit.

### Local Repository

Commits stored on your computer.

### Remote Repository

Repository hosted on GitHub, GitLab, Bitbucket, etc.

---

# 👨‍💻 Typical Developer Workflow

Suppose you're assigned a new story.

## Step 1 — Get the latest code

```bash
git switch main
git pull origin main
```

---

## Step 2 — Create a feature branch

```bash
git switch -c feature/account-onboarding
```

Example naming:

```text
feature/account-onboarding
bugfix/open-date
hotfix/payment-error
```

---

## Step 3 — Make your code changes

Modify the required files.

Then check:

```bash
git status
```

---

## Step 4 — Review changes

```bash
git diff
```

Make sure you haven't accidentally modified unrelated code.

---

## Step 5 — Stage changes

```bash
git add .
```

Then:

```bash
git status
```

---

## Step 6 — Review staged changes

```bash
git diff --staged
```

---

## Step 7 — Commit

```bash
git commit -m "Add custom open date for account onboarding"
```

---

## Step 8 — Push

```bash
git push -u origin feature/account-onboarding
```

---

## Step 9 — Create Pull Request

Go to your Git hosting platform and create a Pull Request.

Typical flow:

```text
feature/account-onboarding
          ↓
        Push
          ↓
Remote feature branch
          ↓
      Pull Request
          ↓
        Review
          ↓
       Approval
          ↓
        Merge
          ↓
         main
```

---

# 🔥 Git Branch + PR Workflow

A common company workflow looks like this:

```text
main
 │
 └── feature/my-story
          │
          ├── code changes
          ├── git add
          ├── git commit
          └── git push
                    │
                    ↓
              Pull Request
                    │
              Code Review
                    │
                 Approval
                    │
                  Merge
```

### Example

```bash
git switch main
git pull origin main

git switch -c feature/JTMF-1234

# Make changes

git status
git diff

git add .
git diff --staged

git commit -m "Implement JTMF-1234"

git push -u origin feature/JTMF-1234
```

Then raise the PR from:

```text
feature/JTMF-1234
```

to:

```text
main
```

**Always follow your company's specific branching and PR rules**, because the target branch may be something other than `main`.

---

# 🔄 Keeping Your Feature Branch Updated

Suppose your feature branch is old and `main` has received new changes.

One common approach:

```bash
git switch main
git pull origin main

git switch feature/my-story
git merge main
```

Or, if your team uses rebase:

```bash
git switch main
git pull origin main

git switch feature/my-story
git rebase main
```

Ask your team which approach they follow.

---

# 🧩 Common Problems

## Problem: "I have changes but need to switch branches"

Use:

```bash
git stash
git switch another-branch
```

Later:

```bash
git switch my-branch
git stash pop
```

---

## Problem: "I accidentally staged a file"

```bash
git restore --staged filename
```

---

## Problem: "I want to see what I changed"

```bash
git diff
```

---

## Problem: "I want to see what is ready for commit"

```bash
git diff --staged
```

---

## Problem: "I committed but haven't pushed"

```bash
git push
```

---

## Problem: "I need the latest remote changes"

```bash
git pull
```

Or, more explicitly:

```bash
git pull origin main
```

---

# ⚠️ Commands to Use Carefully

These commands can cause loss of work or complications if used incorrectly:

```bash
git reset --hard
```

```bash
git clean
```

```bash
git push --force
```

```bash
git rebase
```

Before using destructive commands, make sure you understand exactly what they will change.

---

# ⭐ Most Important Commands to Memorize

If you're a beginner, don't try to memorize every Git command.

Start with these:

```bash
git status
git add .
git commit -m "message"
git pull
git push
git branch
git switch -c branch-name
git switch branch-name
git merge branch-name
git log --oneline
git diff
git stash
```

---

# 📌 Quick Cheat Sheet

| Task                     | Command                       |
| ------------------------ | ----------------------------- |
| Check status             | `git status`                  |
| Initialize repo          | `git init`                    |
| Clone repo               | `git clone <url>`             |
| Stage file               | `git add <file>`              |
| Stage everything         | `git add .`                   |
| Unstage file             | `git restore --staged <file>` |
| Commit                   | `git commit -m "message"`     |
| View branches            | `git branch`                  |
| Create branch            | `git branch <name>`           |
| Create + switch          | `git switch -c <name>`        |
| Switch branch            | `git switch <name>`           |
| Delete local branch      | `git branch -d <name>`        |
| Add remote               | `git remote add origin <url>` |
| View remote              | `git remote -v`               |
| Download remote changes  | `git fetch`                   |
| Fetch + integrate        | `git pull`                    |
| Upload commits           | `git push`                    |
| Merge branch             | `git merge <branch>`          |
| View changes             | `git diff`                    |
| View staged changes      | `git diff --staged`           |
| View commits             | `git log`                     |
| Compact history          | `git log --oneline`           |
| Temporarily save changes | `git stash`                   |
| View stashes             | `git stash list`              |
| Restore stash            | `git stash pop`               |
| Rebase                   | `git rebase <branch>`         |
| Rename file              | `git mv old new`              |
| Delete file              | `git rm <file>`               |
| Show commit              | `git show <SHA>`              |

---

# 🧭 Recommended Learning Order

Learn Git in this order:

```text
1. git status
       ↓
2. git add
       ↓
3. git commit
       ↓
4. git log
       ↓
5. git branch
       ↓
6. git switch
       ↓
7. git pull
       ↓
8. git push
       ↓
9. git fetch
       ↓
10. git merge
       ↓
11. git diff
       ↓
12. git stash
       ↓
13. git restore
       ↓
14. git rebase
       ↓
15. git reset
```

Master the first 10–12 commands before worrying about advanced Git operations.

---

# 💡 Golden Rules

### 1. Always check your status

```bash
git status
```

### 2. Review before committing

```bash
git diff
```

### 3. Review staged changes

```bash
git diff --staged
```

### 4. Write meaningful commit messages

```bash
git commit -m "Fix account onboarding validation"
```

### 5. Pull before starting new work

```bash
git pull
```

### 6. Work on a feature branch

Avoid making feature changes directly on `main` unless your team's workflow explicitly allows it.

### 7. Be careful with destructive commands

Especially:

```bash
git reset --hard
git push --force
```

### 8. Understand before using force push

Never force-push to shared branches unless your team explicitly permits it.

---

# 🚀 The 8 Commands You Will Use Most at Work

In day-to-day development, you'll frequently use:

```bash
git status
```

```bash
git pull
```

```bash
git switch -c feature/my-story
```

```bash
git add .
```

```bash
git commit -m "Implement my story"
```

```bash
git push
```

```bash
git diff
```

```bash
git stash
```

A typical work session may therefore look like:

```bash
git pull origin main

git switch -c feature/JTMF-1234

# Make code changes

git status
git diff

git add .
git diff --staged

git commit -m "Implement JTMF-1234"

git push -u origin feature/JTMF-1234
```

Then raise your **Pull Request**.

---

# 🏁 Final Mental Model

Remember this simple flow:

```text
             YOU WRITE CODE
                    │
                    ↓
            Working Directory
                    │
                git add
                    ↓
             Staging Area
                    │
              git commit
                    ↓
           Local Repository
                    │
                git push
                    ↓
          Remote Repository
                    │
                    ↓
              Pull Request
                    │
                    ↓
               Code Review
                    │
                    ↓
                 Merge
```

If you understand this flow clearly, you already understand the foundation of Git.

---

## 📖 Useful References


* Git book: https://git-scm.com/book/en/v2
* GitHub documentation: https://docs.github.com/
