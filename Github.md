# 🚀 Complete Git & GitHub Commands Reference Guide

> A comprehensive guide covering all Git and GitHub commands from basic to advanced level with use cases, flags, and visual explanations.

---

## 📑 Table of Contents

1. [Configuration Commands](#-configuration-commands)
2. [Repository Initialization](#-repository-initialization)
3. [Basic Workflow Commands](#-basic-workflow-commands)
4. [Branching & Merging](#-branching--merging)
5. [Remote Repository Operations](#-remote-repository-operations)
6. [Inspection & Comparison](#-inspection--comparison)
7. [Undoing Changes](#-undoing-changes)
8. [Stashing](#-stashing)
9. [Rewriting History](#-rewriting-history)
10. [Tagging](#-tagging)
11. [Advanced Operations](#-advanced-operations)
12. [GitHub Specific Commands](#-github-specific-commands)

---

## ⚙️ Configuration Commands

### `git config`

**Purpose:** Configure Git settings at global, local, or system level.

**Common Use Cases:**

```bash
# Set your identity (Required for first-time setup)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default branch name
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "vim"          # Vim

# Enable colorful output
git config --global color.ui auto

# View all configurations
git config --list

# View specific configuration
git config user.name

# Configure line endings (Windows)
git config --global core.autocrlf true

# Configure line endings (Mac/Linux)
git config --global core.autocrlf input

# Set up aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.st status
git config --global alias.last 'log -1 HEAD'
```

**Flags:**
- `--global` - User-level configuration (stored in ~/.gitconfig)
- `--local` - Repository-level configuration (default)
- `--system` - System-wide configuration
- `--list` - List all configuration settings
- `--unset` - Remove a configuration setting

---

## 📦 Repository Initialization

### `git init`

**Purpose:** Initialize a new Git repository in the current directory.

**Visual Flow:**
```
📁 Empty Directory  →  📁 Git Repository (.git folder created)
```

**Use Cases:**

```bash
# Initialize repository in current directory
git init

# Initialize with specific branch name
git init --initial-branch=main
git init -b main

# Initialize a bare repository (for servers)
git init --bare
```

**Flags:**
- `-b <branch-name>` or `--initial-branch=<name>` - Set initial branch name
- `--bare` - Create a bare repository (no working directory)
- `--quiet` or `-q` - Only print error and warning messages

---

### `git clone`

**Purpose:** Clone an existing repository from a remote source.

**Visual Flow:**
```
☁️ Remote Repository  →  📥 Download  →  💻 Local Repository
```

**Use Cases:**

```bash
# Clone a repository
git clone https://github.com/username/repo.git

# Clone with a different folder name
git clone https://github.com/username/repo.git my-project

# Clone specific branch
git clone -b develop https://github.com/username/repo.git

# Clone with limited history (shallow clone)
git clone --depth 1 https://github.com/username/repo.git

# Clone only specific branch with shallow history
git clone --single-branch --branch main --depth 1 https://github.com/username/repo.git

# Clone using SSH
git clone git@github.com:username/repo.git
```

**Flags:**
- `-b <branch>` - Clone specific branch
- `--depth <depth>` - Create shallow clone with limited history
- `--single-branch` - Clone only one branch
- `--recurse-submodules` - Clone with submodules
- `--mirror` - Create mirror clone (for backup)

---

## 🔄 Basic Workflow Commands

### `git status`

**Purpose:** Show the working tree status and staging area.

**Visual Representation:**
```
📝 Working Directory  →  📋 Staging Area  →  📦 Repository

Untracked files:  🔴
Modified files:   🟡
Staged files:     🟢
Committed files:  ✅
```

**Use Cases:**

```bash
# Show full status
git status

# Show short status
git status -s
git status --short

# Show branch information
git status -b
git status --branch
```

**Output Interpretation:**
- `??` - Untracked files
- `M` - Modified files (red = not staged, green = staged)
- `A` - Added files
- `D` - Deleted files
- `R` - Renamed files

**Flags:**
- `-s` or `--short` - Concise output
- `-b` or `--branch` - Show branch info
- `--ignored` - Show ignored files

---

### `git add`

**Purpose:** Add file contents to the staging area (index).

**Visual Animation Concept:**
```
📝 Working Directory          📋 Staging Area          📦 Repository
     (Modified)        →      (Ready to commit)   →   (Committed)
        
     file.txt          →      file.txt (staged)   →   (after commit)
        🔴             →           🟢              →        ✅
```

**Use Cases:**

```bash
# Add specific file
git add filename.txt

# Add multiple files
git add file1.txt file2.txt file3.txt

# Add all files in current directory
git add .

# Add all files in repository
git add -A
git add --all

# Add all modified and deleted files (not untracked)
git add -u
git add --update

# Add files interactively (choose hunks)
git add -i
git add --interactive

# Add parts of a file (patch mode)
git add -p
git add --patch

# Add all .js files
git add *.js

# Add all files in a directory
git add src/

# Dry run (see what would be added)
git add -n .
git add --dry-run .
```

**Flags:**
- `-A` or `--all` - Add all changes (new, modified, deleted)
- `-u` or `--update` - Add modified and deleted (not new)
- `-p` or `--patch` - Interactively choose hunks
- `-i` or `--interactive` - Interactive mode
- `-n` or `--dry-run` - Don't actually add files
- `-f` or `--force` - Add ignored files

---

### `git commit`

**Purpose:** Record changes to the repository.

**Visual Flow:**
```
📋 Staging Area  →  💾 Commit  →  📦 Local Repository

Multiple files   →  Single      →  Permanent
in staging          snapshot        record
```

**Use Cases:**

```bash
# Commit with message
git commit -m "Add user authentication feature"

# Commit with detailed message (opens editor)
git commit

# Add and commit tracked files in one command
git commit -am "Update documentation"
git commit -a -m "Fix bug in login"

# Amend the last commit (change message or add files)
git commit --amend

# Amend without changing message
git commit --amend --no-edit

# Commit with specific author
git commit --author="John Doe <john@example.com>" -m "Message"

# Create empty commit (useful for triggering CI)
git commit --allow-empty -m "Trigger CI build"

# Commit with verbose diff
git commit -v
git commit --verbose

# Sign commit with GPG
git commit -S -m "Signed commit"
git commit --gpg-sign -m "Signed commit"
```

**Flags:**
- `-m <message>` - Commit message
- `-a` or `--all` - Stage all modified/deleted files and commit
- `--amend` - Modify the last commit
- `--no-edit` - Use the previous commit message
- `-v` or `--verbose` - Show diff in commit message editor
- `--allow-empty` - Create commit without changes
- `-S` or `--gpg-sign` - Sign commit with GPG

**Best Practices for Commit Messages:**
```
# Format: <type>(<scope>): <subject>

feat(auth): add JWT token authentication
fix(api): resolve null pointer exception in user service
docs(readme): update installation instructions
style(css): format code according to style guide
refactor(database): optimize query performance
test(user): add unit tests for user registration
chore(deps): update dependencies to latest version
```

---

### `git diff`

**Purpose:** Show changes between commits, commit and working tree, etc.

**Visual Concept:**
```
📝 Working Directory  ↔️  📋 Staging Area  ↔️  📦 Repository

        A              ↔️         B         ↔️        C
     Changes not      Show       Changes      Show    Previous
     yet staged       diff       staged       diff    commits
```

**Use Cases:**

```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged
git diff --cached

# Show changes between commits
git diff commit1 commit2
git diff HEAD~2 HEAD

# Show changes in specific file
git diff filename.txt

# Show changes between branches
git diff main feature-branch

# Show only file names that changed
git diff --name-only

# Show file names with status
git diff --name-status

# Show word-level diff
git diff --word-diff

# Show statistics
git diff --stat

# Show changes since specific commit
git diff abc123

# Compare with remote branch
git diff main origin/main

# Show changes in specific commit
git show commit-hash

# Colorful, side-by-side diff
git diff --color-words
```

**Flags:**
- `--staged` or `--cached` - Show staged changes
- `--name-only` - Show only file names
- `--name-status` - Show file names with status (M, A, D)
- `--stat` - Show statistics
- `--word-diff` - Word-by-word diff
- `--color-words` - Highlighted word diff
- `-w` or `--ignore-all-space` - Ignore whitespace

---

## 🌿 Branching & Merging

### `git branch`

**Purpose:** List, create, or delete branches.

**Visual Branch Structure:**
```
        main    ──●──●──●──●──●──
                     │
                     └──●──●  feature-branch
                           │
                           └──●  bugfix-branch
```

**Use Cases:**

```bash
# List all local branches
git branch

# List all branches (local and remote)
git branch -a
git branch --all

# List remote branches
git branch -r
git branch --remotes

# Create new branch
git branch feature-login

# Create branch from specific commit
git branch bugfix abc1234

# Delete branch
git branch -d feature-completed
git branch --delete feature-completed

# Force delete unmerged branch
git branch -D feature-abandoned
git branch --delete --force feature-abandoned

# Rename current branch
git branch -m new-branch-name
git branch --move new-branch-name

# Rename another branch
git branch -m old-name new-name

# Show last commit on each branch
git branch -v
git branch --verbose

# Show merged branches
git branch --merged

# Show unmerged branches
git branch --no-merged

# Set upstream branch
git branch --set-upstream-to=origin/main main
git branch -u origin/main
```

**Flags:**
- `-a` or `--all` - List all branches
- `-r` or `--remotes` - List remote branches
- `-d` or `--delete` - Delete branch (safe)
- `-D` - Force delete branch
- `-m` or `--move` - Rename branch
- `-v` or `--verbose` - Show last commit
- `--merged` - Show merged branches
- `--no-merged` - Show unmerged branches
- `-u` or `--set-upstream-to` - Set upstream

---

### `git checkout`

**Purpose:** Switch branches or restore working tree files.

**Visual Flow:**
```
main branch    ●──●──●  ←  You are here
                  │
                  └──●──●  feature branch  →  Switch to this
```

**Use Cases:**

```bash
# Switch to existing branch
git checkout feature-branch

# Create and switch to new branch
git checkout -b new-feature
git checkout -b bugfix/issue-123

# Switch to previous branch
git checkout -

# Checkout specific commit (detached HEAD)
git checkout abc1234

# Checkout specific file from another branch
git checkout main -- file.txt

# Checkout specific file from commit
git checkout abc1234 -- file.txt

# Discard changes in file (restore from last commit)
git checkout -- file.txt

# Discard all local changes
git checkout -- .

# Checkout remote branch
git checkout -b feature origin/feature

# Checkout tag
git checkout v1.0.0
```

**Flags:**
- `-b` - Create and switch to new branch
- `-B` - Create/reset and switch to branch
- `-` - Switch to previous branch
- `--` - Separate file paths from branch names
- `-f` or `--force` - Force checkout, discard local changes

**Note:** `git switch` and `git restore` are newer alternatives:
```bash
# Switch branch (clearer than checkout)
git switch feature-branch
git switch -c new-branch

# Restore files (clearer than checkout)
git restore file.txt
git restore --staged file.txt
```

---

### `git merge`

**Purpose:** Join two or more development histories together.

**Visual Merge Types:**

**Fast-Forward Merge:**
```
Before:                After:
main    ●──●           main    ●──●──●──●
           │                              
           └──●──●                        
              feature                     
```

**Three-Way Merge:**
```
Before:                After:
main    ●──●──●        main    ●──●──●──◆
           │                        │  /
           └──●──●                  └──●──●
              feature                  feature
```

**Use Cases:**

```bash
# Merge branch into current branch
git merge feature-branch

# Merge with commit message
git merge feature-branch -m "Merge feature into main"

# Merge without fast-forward (create merge commit)
git merge --no-ff feature-branch

# Merge with fast-forward only (abort if not possible)
git merge --ff-only feature-branch

# Abort merge during conflicts
git merge --abort

# Continue merge after resolving conflicts
git merge --continue

# Use theirs strategy for conflicts
git merge -X theirs feature-branch

# Use ours strategy for conflicts
git merge -X ours feature-branch

# Squash merge (combine all commits into one)
git merge --squash feature-branch

# View merged branches
git branch --merged
```

**Flags:**
- `--no-ff` - Create merge commit even if fast-forward is possible
- `--ff-only` - Only fast-forward, abort otherwise
- `--squash` - Squash commits into one
- `--abort` - Abort merge
- `--continue` - Continue after resolving conflicts
- `-X <strategy>` - Use merge strategy (ours, theirs)
- `-m <message>` - Merge commit message

**Resolving Merge Conflicts:**
```bash
# 1. Identify conflicts
git status

# 2. Open conflicted files and resolve
# Look for conflict markers:
# <<<<<<< HEAD
# Your changes
# =======
# Their changes
# >>>>>>> feature-branch

# 3. Stage resolved files
git add resolved-file.txt

# 4. Complete merge
git commit
```

---

### `git rebase`

**Purpose:** Reapply commits on top of another base tip.

**Visual Rebase:**
```
Before:                After:
main    ●──●──●        main    ●──●──●
           │                           \
           └──●──●                      ●──●
              feature                    feature
                                         (rebased)
```

**Use Cases:**

```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase (edit, squash, reorder commits)
git rebase -i HEAD~3
git rebase --interactive HEAD~5

# Rebase onto specific commit
git rebase abc1234

# Continue rebase after resolving conflicts
git rebase --continue

# Skip current commit during rebase
git rebase --skip

# Abort rebase
git rebase --abort

# Preserve merge commits
git rebase --preserve-merges main

# Rebase with autosquash (works with commit messages)
git rebase -i --autosquash main

# Pull with rebase instead of merge
git pull --rebase origin main
```

**Interactive Rebase Commands:**
```bash
# pick = use commit
# reword = use commit, but edit message
# edit = use commit, but stop for amending
# squash = use commit, but meld into previous commit
# fixup = like squash, but discard commit message
# drop = remove commit

# Example:
pick abc1234 Add feature A
squash def5678 Fix typo
reword ghi9012 Update documentation
drop jkl3456 Wrong implementation
```

**Flags:**
- `-i` or `--interactive` - Interactive rebase
- `--continue` - Continue after resolving conflicts
- `--abort` - Cancel rebase
- `--skip` - Skip current commit
- `--onto` - Rebase onto different branch
- `--preserve-merges` - Preserve merge commits
- `--autosquash` - Automatically squash fixup commits

**⚠️ Warning:** Never rebase commits that have been pushed to shared branches!

---

## 🌐 Remote Repository Operations

### `git remote`

**Purpose:** Manage set of tracked repositories.

**Visual Concept:**
```
💻 Local Repository  ↔️  ☁️ Remote Repository
                          (origin)
```

**Use Cases:**

```bash
# List remote repositories
git remote

# List with URLs
git remote -v
git remote --verbose

# Add remote repository
git remote add origin https://github.com/username/repo.git
git remote add upstream https://github.com/original/repo.git

# Remove remote
git remote remove origin
git remote rm origin

# Rename remote
git remote rename origin new-origin

# Change remote URL
git remote set-url origin https://github.com/username/new-repo.git

# Show remote information
git remote show origin

# Prune deleted remote branches
git remote prune origin

# Update remote tracking branches
git remote update
```

**Flags:**
- `-v` or `--verbose` - Show URLs
- `add` - Add new remote
- `remove` or `rm` - Remove remote
- `rename` - Rename remote
- `set-url` - Change remote URL
- `show` - Show remote details
- `prune` - Delete stale remote-tracking branches

---

### `git fetch`

**Purpose:** Download objects and refs from remote repository.

**Visual Flow:**
```
☁️ Remote Repository  →  📥 Download  →  💻 Local Repository
                                          (origin/main updated)
                                          (main unchanged)
```

**Use Cases:**

```bash
# Fetch from default remote (origin)
git fetch

# Fetch from specific remote
git fetch upstream

# Fetch specific branch
git fetch origin main

# Fetch all remotes
git fetch --all

# Fetch and prune deleted branches
git fetch --prune
git fetch -p

# Fetch tags
git fetch --tags

# Fetch with verbose output
git fetch --verbose

# Dry run (see what would be fetched)
git fetch --dry-run
```

**Flags:**
- `--all` - Fetch all remotes
- `-p` or `--prune` - Remove deleted remote branches
- `--tags` - Fetch all tags
- `--verbose` - Detailed output
- `--dry-run` - Show what would be done

---

### `git pull`

**Purpose:** Fetch from and integrate with another repository or branch.

**Visual Flow:**
```
☁️ Remote  →  📥 Fetch  →  🔀 Merge  →  💻 Local
   origin       download     combine       updated
```

**Use Cases:**

```bash
# Pull from default remote and branch
git pull

# Pull from specific remote and branch
git pull origin main

# Pull with rebase instead of merge
git pull --rebase
git pull --rebase origin main

# Pull all branches
git pull --all

# Pull with fast-forward only
git pull --ff-only

# Pull without commit (stage changes only)
git pull --no-commit

# Pull and automatically stash/unstash changes
git pull --autostash

# Pull specific branch
git pull origin feature-branch
```

**Flags:**
- `--rebase` - Rebase instead of merge
- `--ff-only` - Only fast-forward
- `--no-commit` - Don't create merge commit
- `--all` - Fetch all remotes
- `--autostash` - Stash local changes before pull

**Note:** `git pull` = `git fetch` + `git merge`

---

### `git push`

**Purpose:** Update remote refs along with associated objects.

**Visual Flow:**
```
💻 Local Repository  →  📤 Upload  →  ☁️ Remote Repository
   (committed changes)    push          (updated)
```

**Use Cases:**

```bash
# Push to default remote and branch
git push

# Push to specific remote and branch
git push origin main

# Push and set upstream
git push -u origin feature-branch
git push --set-upstream origin feature-branch

# Push all branches
git push --all

# Push tags
git push --tags

# Push specific tag
git push origin v1.0.0

# Force push (overwrite remote)
git push --force
git push -f

# Safer force push (only if no one else pushed)
git push --force-with-lease

# Delete remote branch
git push origin --delete feature-branch
git push origin :feature-branch

# Push to multiple remotes
git push origin main
git push backup main

# Dry run (see what would be pushed)
git push --dry-run
```

**Flags:**
- `-u` or `--set-upstream` - Set upstream for tracking
- `--all` - Push all branches
- `--tags` - Push all tags
- `-f` or `--force` - Force push (dangerous!)
- `--force-with-lease` - Safer force push
- `--delete` - Delete remote branch
- `--dry-run` - Show what would be pushed

**⚠️ Warning:** Be careful with `--force` as it can overwrite others' work!

---

## 🔍 Inspection & Comparison

### `git log`

**Purpose:** Show commit logs.

**Visual Timeline:**
```
◆ HEAD → main (latest)
│
● Commit 3: Add new feature
│
● Commit 2: Fix bug
│
● Commit 1: Initial commit
```

**Use Cases:**

```bash
# Show commit history
git log

# Show with one line per commit
git log --oneline

# Show last n commits
git log -n 5
git log -5

# Show with file changes
git log --stat

# Show with full diff
git log -p
git log --patch

# Show graph of branches
git log --graph --oneline --all

# Beautiful graph view
git log --graph --oneline --decorate --all

# Show commits by author
git log --author="John Doe"

# Show commits with message matching pattern
git log --grep="fix"

# Show commits in date range
git log --since="2 weeks ago"
git log --after="2024-01-01" --before="2024-12-31"

# Show commits that modified specific file
git log -- path/to/file.txt

# Show commits between branches
git log main..feature-branch

# Show commits with specific string in diff
git log -S "function_name"

# Show commits that added or removed file
git log --diff-filter=A  # Added
git log --diff-filter=D  # Deleted

# Pretty format
git log --pretty=format:"%h - %an, %ar : %s"

# Show commits with files
git log --name-only

# Show commits with status
git log --name-status
```

**Flags:**
- `--oneline` - Condensed output
- `-n <number>` - Limit number of commits
- `--stat` - Show file statistics
- `-p` or `--patch` - Show diff
- `--graph` - Show branch graph
- `--all` - Show all branches
- `--author="name"` - Filter by author
- `--grep="pattern"` - Filter by message
- `--since` / `--after` - Date filter
- `--until` / `--before` - Date filter
- `-S "string"` - Find commits with string in diff

**Pretty Format Options:**
- `%h` - Short commit hash
- `%H` - Full commit hash
- `%an` - Author name
- `%ae` - Author email
- `%ad` - Author date
- `%ar` - Author date, relative
- `%s` - Subject (commit message)

---

### `git show`

**Purpose:** Show various types of objects (commits, tags, etc).

**Use Cases:**

```bash
# Show latest commit
git show
git show HEAD

# Show specific commit
git show abc1234

# Show commit with specific file
git show abc1234:path/to/file.txt

# Show file from another branch
git show main:README.md

# Show tag
git show v1.0.0

# Show commit statistics
git show --stat

# Show commit names only
git show --name-only

# Show commit with status
git show --name-status
```

---

### `git blame`

**Purpose:** Show what revision and author last modified each line of a file.

**Use Cases:**

```bash
# Show who modified each line
git blame file.txt

# Show with commit hashes
git blame -l file.txt

# Show line numbers
git blame -n file.txt

# Show specific lines
git blame -L 10,20 file.txt

# Show with email
git blame -e file.txt

# Ignore whitespace changes
git blame -w file.txt
```

---

### `git grep`

**Purpose:** Print lines matching a pattern in tracked files.

**Use Cases:**

```bash
# Search for pattern
git grep "function_name"

# Search with line numbers
git grep -n "TODO"

# Search case-insensitive
git grep -i "error"

# Show count of matches
git grep -c "import"

# Search in specific commit
git grep "pattern" abc1234
```

---

## ↩️ Undoing Changes

### `git reset`

**Purpose:** Reset current HEAD to specified state.

**Visual Representation:**

**Types of Reset:**
```
Soft Reset (--soft):
  Working Directory: ✅ Unchanged
  Staging Area:      ✅ Unchanged  
  Commits:           ❌ Moved

Mixed Reset (default):
  Working Directory: ✅ Unchanged
  Staging Area:      ❌ Cleared
  Commits:           ❌ Moved

Hard Reset (--hard):
  Working Directory: ❌ Cleared
  Staging Area:      ❌ Cleared
  Commits:           ❌ Moved
```

**Use Cases:**

```bash
# Unstage file (keep changes)
git reset file.txt
git reset HEAD file.txt

# Unstage all files
git reset

# Soft reset - move HEAD, keep changes staged
git reset --soft HEAD~1
git reset --soft abc1234

# Mixed reset (default) - move HEAD, unstage changes
git reset HEAD~1
git reset --mixed HEAD~2

# Hard reset - discard all changes ⚠️
git reset --hard HEAD~1
git reset --hard origin/main

# Reset to specific commit
git reset --hard abc1234

# Reset single file to specific commit
git reset abc1234 -- file.txt

# Undo last commit but keep changes
git reset --soft HEAD~1

# Undo last 3 commits
git reset --hard HEAD~3
```

**Flags:**
- `--soft` - Keep changes in staging area
- `--mixed` (default) - Keep changes in working directory
- `--hard` - Discard all changes (⚠️ dangerous!)
- `HEAD~n` - Go back n commits
- `<commit>` - Reset to specific commit

**Common Scenarios:**

```bash
# Scenario 1: Undo last commit, keep changes staged
git reset --soft HEAD~1

# Scenario 2: Undo last commit, keep changes unstaged
git reset HEAD~1

# Scenario 3: Completely remove last commit
git reset --hard HEAD~1

# Scenario 4: Unstage file
git reset HEAD file.txt

# Scenario 5: Reset to remote state
git reset --hard origin/main
```

---

### `git revert`

**Purpose:** Create new commit that undoes changes from previous commit.

**Visual Flow:**
```
Before:                 After:
●──●──●──●             ●──●──●──●──◆
       bad commit             new commit
       (kept)                 (reverses changes)
```

**Use Cases:**

```bash
# Revert last commit
git revert HEAD

# Revert specific commit
git revert abc1234

# Revert without creating commit
git revert --no-commit abc1234
git revert -n abc1234

# Revert multiple commits
git revert HEAD~3..HEAD

# Revert merge commit
git revert -m 1 merge-commit-hash

# Continue revert after resolving conflicts
git revert --continue

# Abort revert
git revert --abort
```

**Flags:**
- `-n` or `--no-commit` - Don't create commit automatically
- `-m <parent-number>` - Revert merge commit
- `--continue` - Continue after resolving conflicts
- `--abort` - Cancel revert operation

**Reset vs Revert:**
- `reset` - Moves branch pointer backward (rewrites history)
- `revert` - Creates new commit (preserves history)
- Use `revert` for shared/public branches
- Use `reset` for local/private branches

---

### `git restore`

**Purpose:** Restore working tree files (modern alternative to checkout).

**Use Cases:**

```bash
# Discard changes in file
git restore file.txt

# Restore multiple files
git restore file1.txt file2.txt

# Restore all files
git restore .

# Unstage file
git restore --staged file.txt

# Restore file from specific commit
git restore --source=abc1234 file.txt

# Restore file from another branch
git restore --source=main file.txt

# Restore and unstage
git restore --staged --worktree file.txt
```

**Flags:**
- `--staged` - Restore in staging area (unstage)
- `--worktree` - Restore in working directory
- `--source=<tree>` - Restore from specific commit/branch

---

### `git clean`

**Purpose:** Remove untracked files from working tree.

**Use Cases:**

```bash
# Dry run (see what would be deleted)
git clean -n
git clean --dry-run

# Remove untracked files
git clean -f
git clean --force

# Remove untracked files and directories
git clean -fd

# Remove ignored files too
git clean -fx

# Remove all untracked and ignored files
git clean -fdx

# Interactive mode
git clean -i
```

**Flags:**
- `-n` or `--dry-run` - Show what would be deleted
- `-f` or `--force` - Actually delete
- `-d` - Remove directories
- `-x` - Remove ignored files too
- `-X` - Remove only ignored files
- `-i` - Interactive mode

---

## 📦 Stashing

### `git stash`

**Purpose:** Temporarily store modified tracked files.

**Visual Concept:**
```
Working Directory  →  📦 Stash Stack  →  Working Directory
   (changes)           (stored)            (restored)
