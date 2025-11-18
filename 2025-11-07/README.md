# Git & GitHub: DevOps Notes

## 1\. Introduction to Source Code Management (SCM)

**SCM** stands for **Source Code Management**. It is a system that helps teams collaborate on code safely and track every change forever.

### Types of SCM

| Type | Where Code Lives | Description | Example |
| :--- | :--- | :--- | :--- |
| **Local SCM** | Only on your computer | No collaboration; only accessible to one user/machine. | RCS |
| **Remote SCM** | Central server | Everyone connects to a central server. Dependent on server uptime. | Subversion (SVN) |
| **Distributed SCM** | Every developer's machine | Each user has a **full copy** of the history/repository. Works offline. | **Git** |

**Why Distributed (Git) is preferred:**

  * **Collaboration:** Full collaboration where every user has a complete copy.
  * **Availability:** Works offline; highly available since every clone acts as a backup.
  * **Performance:** Very fast because operations are local.

-----

## 2\. Installation & Configuration

### Installing Git

  * **CentOS/RHEL:** `sudo su` $\rightarrow$ `yum update` $\rightarrow$ `yum install git`.
  * **Ubuntu:** `apt update -y` $\rightarrow$ `apt install git`.
  * **Verify:** `git --version`.

### Initial Configuration (The Identity Scroll)

Before committing, you must configure your identity, which signs every commit.

```bash
git config --global user.name "Your Name"    
git config --global user.email "email@ex.com" 
[cite_start]git config --global core.editor "vim"
```

### SSH Setup (Connecting to GitHub)

To share code securely without entering passwords every time:

1.  **Generate Keys:** `ssh-keygen -t rsa -b 4096 -C "email@ex.com"`.
2.  **Add to GitHub:** Copy the public key and add it to GitHub account settings.

-----

## 3\. The Four Kingdoms (Core Areas of Git)

Data in Git moves through four specific areas.

1.  **Working Tree (Workspace):** The actual files you are currently editing.
2.  **Staging Area (Index/Cache):** A preview area where you prepare specific files for the next commit.
3.  **Local Repository (.git):** The database on your machine storing commits and history.
4.  **Remote Repository (Origin):** The server-hosted copy (e.g., GitHub) for sharing.

-----

## 4\. Basic Workflow Commands

### Starting a Project

  * `mkdir demo` & `cd demo`.
  * **Initialize:** `git init` (Creates the hidden `.git` folder).
  * **Clone:** `git clone <url>` (Downloads an existing repo including history).

### Tracking and Committing

  * **Check Status:** `git status` (Shows untracked/modified files).
  * **Stage Files:**
      * `git add filename.txt` (Moves file to Staging Area).
      * `git add .` or `git add -A` (Stages all changes).
  * **Commit:** `git commit -m "Message"` (Saves snapshot to Local Repo).
      * `git commit --amend` (Modify the last commit/message).

### Synchronization (Remote)

  * **Link Remote:** `git remote add origin <url>`.
  * **Push:** `git push origin main` (Uploads local commits to remote).
  * **Fetch:** `git fetch` (Downloads updates but does not merge).
  * **Pull:** `git pull` (Fetch + Merge).

-----

## 5\. Inspecting and Comparing

  * **Diff Working Tree vs. Last Commit:** `git diff`.
  * **Diff Staging Area vs. Last Commit:** `git diff --cached` (or `--staged`).
  * **Commit History:**
      * `git log` (Linear history).
      * `git log --oneline --graph --decorate --all` (Visual graph).
      * `git blame <file>` (See who changed each line and when).

-----

## 6\. Branching and Merging

### Managing Branches

  * **List Branches:** `git branch` (local) or `git branch -a` (all).
  * **Create Branch:** `git branch <name>`.
  * **Switch Branch:**
      * Old way: `git checkout <name>`.
      * New way: `git switch <name>`.
  * **Create & Switch:** `git switch -c <name>`.
  * **Delete Branch:** `git branch -d <name>` (Safe) or `-D` (Force).

### Merging & Rebasing

  * **Merge:** `git merge <branch>` (Combines history, creates merge commit).
  * **Rebase:** `git rebase master` (Moves current branch commits on top of master for linear history).
      * *Interactive Rebase:* `git rebase -i HEAD~5` (Squash/edit last 5 commits).

-----

## 7\. Advanced Operations

### Stashing (Temporary Storage)

Useful when you want to switch branches but have unfinished work.

  * **Save Stash:** `git stash` or `git stash push -m "msg"`.
  * **List Stashes:** `git stash list`.
  * **Apply & Keep:** `git stash apply stash@{0}`.
  * **Apply & Drop:** `git stash pop`.

### Undoing Changes

  * **Revert:** `git revert <commit>` (Creates a *new* commit that undoes changes; safe for history).
  * **Reset (Move HEAD back):**
      * `--soft`: Keep changes staged.
      * `--mixed`: Keep changes in working tree (unstaged).
      * `--hard`: Discard all changes (Destructive).

### Searching

  * `git grep "text"`: Search text in files[cite: 375].
  * `git log -S"string"`: Find commits that added/removed a string.
  * `git bisect`: Binary search to find a "bad" commit that introduced a bug.

### Ignoring Files

Create a `.gitignore` file to exclude files (like logs or node\_modules) from tracking.

  * `git check-ignore -v path/file`: Checks which rule is ignoring a file.

-----

## 8\. Best Practices & Workflows

### Feature Branch Workflow

1.  Create branch: `git checkout -b feature/awesome`.
2.  Make changes, add, and commit.
3.  Push upstream: `git push -u origin feature/awesome`.
4.  Open Pull Request on remote.

### Safety Commands

  * `git remote show origin`: See tracking info.
  * `git fetch --all --prune`: Update remotes and remove deleted branches locally.
  * `git branch --merged`: Check which branches are already merged before deleting.
