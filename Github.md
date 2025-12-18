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
```

**Use Cases:**

```bash
# Stash current changes
git stash
git stash save "Work in progress"

# Stash with untracked files
git stash -u
git stash --include-untracked

# Stash everything including ignored files
git stash -a
git stash --all

# List all stashes
git stash list

# Show stash content
git stash show
git stash show -p stash@{0}

# Apply latest stash (keep in stash list)
git stash apply

# Apply specific stash
git stash apply stash@{2}

# Apply and remove latest stash
git stash pop

# Apply and remove specific stash
git stash pop stash@{1}

# Create branch from stash
git stash branch new-branch-name

# Remove specific stash
git stash drop stash@{1}

# Clear all stashes
git stash clear

# Stash only specific files
git stash push -m "message" file.txt

# Stash interactively
git stash -p
```

**Flags:**
- `save "message"` - Stash with message
- `-u` or `--include-untracked` - Include untracked files
- `-a` or `--all` - Include ignored files
- `list` - List all stashes
- `show` - Show stash contents
- `apply` - Apply stash (keep it)
- `pop` - Apply and remove stash
- `drop` - Remove stash
- `clear` - Remove all stashes
- `branch` - Create branch from stash
- `-p` or `--patch` - Interactive stashing

**Common Scenarios:**

```bash
# Scenario 1: Quick context switch
git stash
git checkout main
# ... do work ...
git checkout feature
git stash pop

# Scenario 2: Save work with message
git stash save "Half-finished login feature"

# Scenario 3: Apply stash to different branch
git stash
git checkout other-branch
git stash pop
```

---

## 🔄 Rewriting History

### `git commit --amend`

**Purpose:** Modify the most recent commit.

**Visual Flow:**
```
Before:           After:
●──●──●          ●──●──◆
      old              new
      commit           commit
                       (replaces old)
```

**Use Cases:**

```bash
# Change last commit message
git commit --amend

# Add files to last commit
git add forgotten-file.txt
git commit --amend --no-edit

# Change author of last commit
git commit --amend --author="Name <email@example.com>"

# Change date of last commit
git commit --amend --date="2024-12-01 10:00:00"
```

---

### `git rebase -i` (Interactive Rebase)

**Purpose:** Edit, reorder, squash, or delete commits.

**Use Cases:**

```bash
# Interactive rebase last 5 commits
git rebase -i HEAD~5

# Rebase onto main interactively
git rebase -i main

# Edit specific commit
git rebase -i abc1234^

# Autosquash fixup commits
git rebase -i --autosquash HEAD~10
```

**Interactive Commands:**
```
pick    = use commit
reword  = change commit message
edit    = stop for amending
squash  = combine with previous commit
fixup   = like squash but discard message
drop    = remove commit
```

---

### `git filter-branch`

**Purpose:** Rewrite branches (complex history rewriting).

**⚠️ Warning:** Use `git filter-repo` instead (modern alternative).

**Use Cases:**

```bash
# Remove file from all history
git filter-branch --tree-filter 'rm -f passwords.txt' HEAD

# Change author in all commits
git filter-branch --env-filter '
if [ "$GIT_AUTHOR_EMAIL" = "old@email.com" ]; then
    export GIT_AUTHOR_EMAIL="new@email.com"
fi' HEAD
```

**Better Alternative: git filter-repo**
```bash
# Install first: pip install git-filter-repo

# Remove file from history
git filter-repo --path passwords.txt --invert-paths

# Change author
git filter-repo --mailmap mailmap.txt
```

---

### `git reflog`

**Purpose:** Reference logs - track where HEAD has been.

**Use Cases:**

```bash
# Show reflog
git reflog

# Show reflog with dates
git reflog --date=relative

# Recover lost commit
git reflog
git checkout abc1234  # or git reset --hard abc1234

# Show reflog for specific branch
git reflog show main

# Expire old reflog entries
git reflog expire --expire=30.days.ago --all
```

**Common Scenario - Recover Lost Commits:**
```bash
# 1. Made a mistake with reset
git reset --hard HEAD~3

# 2. Find lost commits
git reflog

# 3. Recover
git reset --hard abc1234  # hash from reflog
```

---

## 🏷️ Tagging

### `git tag`

**Purpose:** Create, list, delete, or verify tags.

**Visual Concept:**
```
●──●──●──●──●
      ↑      ↑
    v1.0   v2.0
    (tag)  (tag)
```

**Use Cases:**

```bash
# List all tags
git tag

# List tags matching pattern
git tag -l "v1.*"
git tag --list "v2.*"

# Create lightweight tag
git tag v1.0.0

# Create annotated tag (recommended)
git tag -a v1.0.0 -m "Release version 1.0.0"

# Tag specific commit
git tag -a v0.9.0 abc1234 -m "Beta release"

# Show tag information
git show v1.0.0

# Push tag to remote
git push origin v1.0.0

# Push all tags
git push --tags
git push origin --tags

# Delete local tag
git tag -d v1.0.0
git tag --delete v1.0.0

# Delete remote tag
git push origin --delete v1.0.0
git push origin :refs/tags/v1.0.0

# Checkout tag (detached HEAD)
git checkout v1.0.0

# Create branch from tag
git checkout -b version1-hotfix v1.0.0

# List tags with commit messages
git tag -n

# Verify signed tag
git tag -v v1.0.0
```

**Flags:**
- `-a` - Create annotated tag
- `-m` - Tag message
- `-l` or `--list` - List tags
- `-d` or `--delete` - Delete tag
- `-n` - Show tag with commit message
- `-v` - Verify signed tag
- `-s` - Create signed tag

**Tag Naming Convention:**
```
v1.0.0        - Major release
v1.1.0        - Minor release
v1.1.1        - Patch release
v2.0.0-rc.1   - Release candidate
v1.0.0-beta   - Beta version
```

---

## 🚀 Advanced Operations

### `git cherry-pick`

**Purpose:** Apply changes from specific commits.

**Visual Flow:**
```
main    ●──●──●
           │
feature    └──●──●──●
              specific
              commit  →  cherry-pick  →  main  ●──●──●──◆
```

**Use Cases:**

```bash
# Cherry-pick single commit
git cherry-pick abc1234

# Cherry-pick multiple commits
git cherry-pick abc1234 def5678

# Cherry-pick commit range
git cherry-pick abc1234..def5678

# Cherry-pick without committing
git cherry-pick -n abc1234
git cherry-pick --no-commit abc1234

# Continue after resolving conflicts
git cherry-pick --continue

# Abort cherry-pick
git cherry-pick --abort

# Cherry-pick and edit message
git cherry-pick -e abc1234
git cherry-pick --edit abc1234
```

---

### `git bisect`

**Purpose:** Use binary search to find commit that introduced a bug.

**Visual Process:**
```
good ●──●──●──●──●──● bad
         ↓
    Testing middle commit
         ↓
    Narrow down range
         ↓
    Find buggy commit
```

**Use Cases:**

```bash
# Start bisecting
git bisect start

# Mark current commit as bad
git bisect bad

# Mark last known good commit
git bisect good abc1234

# Git will checkout middle commit
# Test the code, then mark as good or bad
git bisect good   # if working
git bisect bad    # if broken

# Continue until bug is found
# Git will show you the bad commit

# End bisecting
git bisect reset

# Automate bisecting with script
git bisect start
git bisect bad
git bisect good abc1234
git bisect run npm test  # or any test command

# Skip commit if it can't be tested
git bisect skip
```

---

### `git worktree`

**Purpose:** Manage multiple working trees attached to same repository.

**Use Cases:**

```bash
# List worktrees
git worktree list

# Add new worktree
git worktree add ../feature-worktree feature-branch

# Add worktree with new branch
git worktree add -b new-feature ../new-feature-worktree

# Remove worktree
git worktree remove ../feature-worktree

# Prune worktree information
git worktree prune
```

---

### `git submodule`

**Purpose:** Manage repositories inside repositories.

**Use Cases:**

```bash
# Add submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Initialize submodules
git submodule init

# Update submodules
git submodule update

# Clone with submodules
git clone --recurse-submodules https://github.com/user/repo.git

# Update all submodules to latest
git submodule update --remote

# Execute command in all submodules
git submodule foreach 'git pull origin main'

# Remove submodule
git submodule deinit path/to/submodule
git rm path/to/submodule
rm -rf .git/modules/path/to/submodule
```

---

### `git archive`

**Purpose:** Create archive of files from named tree.

**Use Cases:**

```bash
# Create zip archive
git archive --format=zip HEAD > project.zip

# Create tar archive
git archive --format=tar HEAD > project.tar

# Archive specific branch
git archive --format=zip main > main-branch.zip

# Archive with prefix
git archive --prefix=project/ HEAD > project.tar.gz

# Archive specific directory
git archive HEAD:src/ > src.zip
```

---

### `git shortlog`

**Purpose:** Summarize git log output.

**Use Cases:**

```bash
# Show commit count by author
git shortlog -s

# Show with number of commits
git shortlog -n

# Show with email
git shortlog -e

# Summary since specific date
git shortlog --since="2024-01-01"
```

---

## 🐙 GitHub Specific Commands

### GitHub CLI (`gh`)

**Installation:**
```bash
# macOS
brew install gh

# Windows
winget install GitHub.cli

# Linux
sudo apt install gh
```

**Authentication:**
```bash
# Login to GitHub
gh auth login

# Check auth status
gh auth status
```

**Repository Operations:**

```bash
# Create new repository
gh repo create my-project --public
gh repo create my-project --private

# Clone repository
gh repo clone username/repo

# Fork repository
gh repo fork username/repo

# View repository
gh repo view
gh repo view username/repo --web

# List repositories
gh repo list
gh repo list username

# Delete repository
gh repo delete username/repo
```

**Pull Request Operations:**

```bash
# Create pull request
gh pr create
gh pr create --title "Add feature" --body "Description"

# List pull requests
gh pr list

# View pull request
gh pr view 123
gh pr view 123 --web

# Checkout pull request
gh pr checkout 123

# Review pull request
gh pr review 123 --approve
gh pr review 123 --request-changes
gh pr review 123 --comment -b "Looks good!"

# Merge pull request
gh pr merge 123
gh pr merge 123 --squash
gh pr merge 123 --rebase

# Close pull request
gh pr close 123

# Reopen pull request
gh pr reopen 123
```

**Issue Operations:**

```bash
# Create issue
gh issue create
gh issue create --title "Bug report" --body "Description"

# List issues
gh issue list

# View issue
gh issue view 456

# Close issue
gh issue close 456

# Reopen issue
gh issue reopen 456

# Comment on issue
gh issue comment 456 --body "This is fixed"
```

**GitHub Actions:**

```bash
# List workflows
gh workflow list

# View workflow runs
gh run list

# View specific run
gh run view 789

# Rerun workflow
gh run rerun 789

# Watch workflow run
gh run watch
```

**Release Operations:**

```bash
# Create release
gh release create v1.0.0
gh release create v1.0.0 --notes "Release notes"

# List releases
gh release list

# View release
gh release view v1.0.0

# Download release assets
gh release download v1.0.0

# Delete release
gh release delete v1.0.0
```

**Gist Operations:**

```bash
# Create gist
gh gist create file.txt
gh gist create file.txt --public

# List gists
gh gist list

# View gist
gh gist view gist-id

# Edit gist
gh gist edit gist-id
```

---

## 📝 Useful Git Configurations

### Aliases

```bash
# Short commands
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

# Pretty log
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# Show last commit
git config --global alias.last 'log -1 HEAD'

# Undo last commit
git config --global alias.undo 'reset HEAD~1 --mixed'

# Amend without edit
git config --global alias.amend 'commit --amend --no-edit'

# List aliases
git config --global alias.aliases 'config --get-regexp alias'
```

### Global Gitignore

```bash
# Create global gitignore
git config --global core.excludesfile ~/.gitignore_global

# Add common patterns
echo ".DS_Store" >> ~/.gitignore_global
echo "node_modules/" >> ~/.gitignore_global
echo ".env" >> ~/.gitignore_global
echo "*.log" >> ~/.gitignore_global
```

### Useful Settings

```bash
# Set default editor
git config --global core.editor "code --wait"

# Enable colors
git config --global color.ui auto

# Set default branch name
git config --global init.defaultBranch main

# Auto-correct typos
git config --global help.autocorrect 1

# Prune on fetch
git config --global fetch.prune true

# Rebase on pull
git config --global pull.rebase true

# Use ssh instead of https
git config --global url."git@github.com:".insteadOf "https://github.com/"
```

---

## 🔧 Troubleshooting Commands

### Common Issues

```bash
# Discard all local changes
git reset --hard HEAD
git clean -fd

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Fix "detached HEAD"
git checkout main  # or any branch

# Recover deleted branch
git reflog
git checkout -b recovered-branch abc1234

# Fix merge conflicts
git merge --abort  # abort merge
# or resolve conflicts then:
git add .
git commit

# Remove untracked files
git clean -fd

# Update remote URL
git remote set-url origin new-url

# Sync fork with upstream
git fetch upstream
git checkout main
git merge upstream/main

# Fix "refusing to merge unrelated histories"
git pull origin main --allow-unrelated-histories

# Large file issues
git filter-repo --strip-blobs-bigger-than 10M
```

---

## 📚 Quick Reference Chart

### File States

| State | Command to Reach | Command to Leave |
|-------|------------------|------------------|
| Untracked | Create new file | `git add` |
| Staged | `git add` | `git reset` |
| Modified | Edit file | `git add` or `git restore` |
| Committed | `git commit` | `git reset` |

### Reset Types

| Type | HEAD | Index (Staging) | Working Directory |
|------|------|-----------------|-------------------|
| `--soft` | ✅ Changed | ❌ Not Changed | ❌ Not Changed |
| `--mixed` | ✅ Changed | ✅ Changed | ❌ Not Changed |
| `--hard` | ✅ Changed | ✅ Changed | ✅ Changed |

### Common Workflows

**Feature Branch Workflow:**
```bash
git checkout -b feature-branch
# ... make changes ...
git add .
git commit -m "Add feature"
git push -u origin feature-branch
# ... create pull request on GitHub ...
git checkout main
git pull origin main
git branch -d feature-branch
```

**Hotfix Workflow:**
```bash
git checkout main
git checkout -b hotfix-branch
# ... fix bug ...
git add .
git commit -m "Fix critical bug"
git checkout main
git merge hotfix-branch
git push origin main
git branch -d hotfix-branch
```

**Sync Fork:**
```bash
git remote add upstream original-repo-url
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## 🎯 Best Practices

### Commit Messages

✅ **Good:**
```
feat(auth): add JWT token authentication
fix(api): resolve null pointer exception in user service
docs(readme): update installation instructions
```

❌ **Bad:**
```
fixed stuff
update
wip
asdfgh
```

### Branching Strategy

```
main (production)
  ├── develop (integration)
  │     ├── feature/user-auth
  │     ├── feature/payment-gateway
  │     └── feature/dashboard
  ├── hotfix/critical-bug
  └── release/v1.2.0
```

### Do's and Don'ts

✅ **DO:**
- Commit often with meaningful messages
- Use branches for features
- Pull before pushing
- Use `.gitignore` properly
- Review changes before committing
- Use `git revert` for public branches

❌ **DON'T:**
- Commit sensitive data (passwords, keys)
- Force push to shared branches
- Rewrite public history
- Commit large binary files
- Have vague commit messages
- Work directly on main/master

---

## 🆘 Emergency Commands

```bash
# "Oh no, I accidentally committed to main!"
git reset --soft HEAD~1
git checkout -b feature-branch
git commit

# "I need to undo everything!"
git reflog  # find safe point
git reset --hard abc1234

# "I deleted a branch by mistake!"
git reflog
git checkout -b recovered-branch abc1234

# "I committed sensitive data!"
git filter-repo --path secrets.txt --invert-paths
git push --force

# "My working directory is a mess!"
git stash
git stash drop  # or pop to recover

# "I want to start over!"
git fetch origin
git reset --hard origin/main
git clean -fd
```

---

## 📖 Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [GitHub Docs](https://docs.github.com)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Learn Git Branching](https://learngitbranching.js.org/)
- [Oh My Git!](https://ohmygit.org/) - Interactive Git Learning
- [Git Explorer](https://gitexplorer.com/) - Find the right command

---

## 🎓 Practice Exercises

1. **Basic Workflow:**
   - Initialize repository
   - Make changes and commit
   - Create branch and merge

2. **Collaboration:**
   - Fork repository
   - Create feature branch
   - Submit pull request

3. **History Manipulation:**
   - Use interactive rebase
   - Squash commits
   - Cherry-pick specific commits

4. **Recovery:**
   - Practice using reflog
   - Recover deleted branches
   - Undo various mistakes

---

**Created with ❤️ for developers and DevOps professionals**

> 💡 **Pro Tip:** Bookmark this guide and refer to it whenever you forget a command. Practice regularly to build muscle memory!

---

**Last Updated:** December 2025  
**Git Version:** 2.43+  
**Compatible with:** Linux, macOS, Windows
