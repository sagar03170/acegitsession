# Git Command Reference — Instructor Copy
### Git & CI/CD Workshop | 2-Day Session

---

## ⚙️ SETUP (Run Once)

```bash
# Set your identity
git config --global user.name  "Your Name"
git config --global user.email "you@example.com"

# Verify config
git config --list

# Set VS Code as default editor
git config --global core.editor "code --wait"

# Set default branch to main
git config --global init.defaultBranch main
```

---

## 📁 CREATING / CLONING A REPO

```bash
# Initialize a new repo
git init my-project
cd my-project

# Clone an existing remote repo
git clone https://github.com/user/repo.git

# Clone into a specific folder name
git clone https://github.com/user/repo.git my-folder
```

---

## 📊 STATUS & INSPECTION

```bash
# Check current state of working directory
git status

# Short status view
git status -s

# View commit history
git log

# Pretty one-line log
git log --oneline

# Log with branch graph
git log --oneline --graph --all

# See what changed in a file (unstaged diff)
git diff

# See what's staged vs last commit
git diff --staged

# See a specific commit
git show <commit-hash>
```

---

## ➕ STAGING & COMMITTING

```bash
# Stage a single file
git add filename.py

# Stage all changes
git add .

# Stage specific parts of a file interactively
git add -p

# Commit with a message
git commit -m "feat: add user login endpoint"

# Stage all tracked files AND commit in one step
git commit -am "fix: correct off-by-one error"

# Amend the last commit (before pushing)
git commit --amend -m "corrected commit message"
```

### ✅ Conventional Commits Format
```
feat:     new feature
fix:      bug fix
docs:     documentation only
style:    formatting, no logic change
refactor: code change without new feature/fix
test:     adding or fixing tests
chore:    build tasks, dependencies
```

---

## 🌿 BRANCHING

```bash
# List all local branches
git branch

# List all branches (local + remote)
git branch -a

# Create a new branch
git branch feature/login

# Switch to a branch
git checkout feature/login
# or (modern syntax):
git switch feature/login

# Create AND switch in one command
git checkout -b feature/login
# or:
git switch -c feature/login

# Delete a branch (after merging)
git branch -d feature/login

# Force delete (even if unmerged)
git branch -D feature/login

# Rename current branch
git branch -m new-name
```

---

## 🔀 MERGING

```bash
# Merge a branch into current branch
git checkout main
git merge feature/login

# Merge with no fast-forward (keeps merge commit)
git merge --no-ff feature/login

# Abort a merge in progress
git merge --abort
```

### 🔥 Resolving Merge Conflicts (Step-by-Step)

```bash
# 1. See which files have conflicts
git status

# 2. Open the file — look for these markers:
# <<<<<<< HEAD
# your changes
# =======
# their changes
# >>>>>>> feature/login

# 3. Edit the file: keep what's correct, remove ALL markers

# 4. Mark as resolved
git add conflicted-file.py

# 5. Complete the merge
git commit
```

---

## ☁️ REMOTE REPOS

```bash
# View remote connections
git remote -v

# Add a remote
git remote add origin https://github.com/user/repo.git

# Push to remote (first time — set upstream)
git push -u origin main

# Push after upstream is set
git push

# Push a new branch
git push origin feature/login

# Pull latest changes
git pull

# Fetch without merging
git fetch origin

# Pull with rebase (cleaner history)
git pull --rebase
```

---

## ↩️ UNDOING THINGS

```bash
# Unstage a file (keep changes)
git restore --staged filename.py

# Discard changes in working directory (PERMANENT)
git restore filename.py

# Undo last commit, keep changes staged
git reset --soft HEAD~1

# Undo last commit, keep changes unstaged
git reset --mixed HEAD~1

# Undo last commit AND discard all changes (DANGEROUS)
git reset --hard HEAD~1

# Create a new commit that undoes a previous commit (safe for shared branches)
git revert <commit-hash>
```

---

## 🏷️ STASHING

```bash
# Save uncommitted work temporarily
git stash

# Stash with a message
git stash push -m "WIP: working on auth"

# List all stashes
git stash list

# Apply most recent stash (keep it in stash list)
git stash apply

# Apply and remove from stash list
git stash pop

# Drop a specific stash
git stash drop stash@{0}
```

---

## 🔖 TAGS

```bash
# Create a lightweight tag
git tag v1.0.0

# Create an annotated tag (recommended for releases)
git tag -a v1.0.0 -m "Release version 1.0.0"

# List tags
git tag

# Push tags to remote
git push origin --tags
```

---

## 🌐 GITHUB WORKFLOW (Pull Request Flow)

```bash
# Step 1: Create a feature branch
git switch -c feature/user-profile

# Step 2: Make changes, commit them
git add .
git commit -m "feat: add user profile page"

# Step 3: Push the branch to GitHub
git push -u origin feature/user-profile

# Step 4: Go to GitHub → Open a Pull Request
#   - Compare: feature/user-profile → main
#   - Write a description
#   - Request review

# Step 5: After PR is merged, clean up
git switch main
git pull
git branch -d feature/user-profile
```

---

## 🔍 USEFUL SHORTCUTS & ALIASES

```bash
# Pretty log alias
git config --global alias.lg "log --oneline --graph --all"

# Quick status
git config --global alias.st "status -s"

# Shortcut to push current branch
git config --global alias.pushup "push -u origin HEAD"

# Undo last commit
git config --global alias.undo "reset --soft HEAD~1"

# Usage after setting aliases
git lg
git st
git pushup
git undo
```

---

## 🛠️ DEMO LAB WALKTHROUGH

### Demo 1: First Repo from Scratch

```bash
mkdir demo-project && cd demo-project
git init
echo "# My Project" > README.md
git add README.md
git commit -m "chore: initial project setup"

# Create a Python file
echo 'print("Hello, Git!")' > app.py
git add app.py
git commit -m "feat: add app entry point"

git log --oneline
```

### Demo 2: Feature Branch Workflow

```bash
# Create feature branch
git switch -c feature/greeting

# Edit app.py — change to: print("Hello from feature branch!")
git add app.py
git commit -m "feat: update greeting message"

# Switch back to main
git switch main

# See that main still has the old version
cat app.py

# Merge the feature
git merge feature/greeting
cat app.py   # Now has the update

# Clean up
git branch -d feature/greeting
```

### Demo 3: Simulating a Merge Conflict

```bash
# Terminal A — on main
git switch main
echo 'name = "Alice"' > config.py
git add config.py && git commit -m "chore: set name to Alice"

# Create a branch and change config
git switch -c feature/config
echo 'name = "Bob"' > config.py
git add config.py && git commit -m "chore: set name to Bob"

# Go back to main and make a conflicting change
git switch main
echo 'name = "Charlie"' > config.py
git add config.py && git commit -m "chore: set name to Charlie"

# Now merge — CONFLICT!
git merge feature/config

# Resolve manually, then:
git add config.py
git commit -m "merge: resolve config name conflict"
```

---

## 📄 .gitignore — Patterns to Ignore

```gitignore
# Python
__pycache__/
*.pyc
*.pyo
.env
venv/
*.egg-info/

# Node.js
node_modules/
dist/
.env.local

# General
.DS_Store
*.log
*.tmp
.idea/
.vscode/settings.json

# Secrets — NEVER commit these
*.pem
*.key
secrets.yaml
config.secret.json
```

---

## 🚀 GITHUB ACTIONS — CI/CD REFERENCE

### Basic CI Workflow

```yaml
# .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install pytest flake8

      - name: Lint with flake8
        run: flake8 . --max-line-length=100

      - name: Run tests
        run: pytest tests/ -v
```

### With Deploy Stage

```yaml
  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to server
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_SSH_KEY }}
        run: |
          echo "Deploying to production..."
          # Add your deploy script here
```

### Using Environment Variables & Secrets

```yaml
      - name: Use secret
        env:
          DATABASE_URL: ${{ secrets.DATABASE_URL }}
          API_KEY: ${{ secrets.API_KEY }}
        run: python deploy.py
```

---

## 📋 QUICK CHEAT SHEET

| Action | Command |
|---|---|
| Initialize repo | `git init` |
| Clone repo | `git clone <url>` |
| Check status | `git status` |
| Stage all | `git add .` |
| Commit | `git commit -m "message"` |
| Push | `git push` |
| Pull | `git pull` |
| New branch | `git switch -c <name>` |
| Switch branch | `git switch <name>` |
| Merge branch | `git merge <name>` |
| View log | `git log --oneline` |
| Stash | `git stash` |
| Unstash | `git stash pop` |
| Undo last commit | `git reset --soft HEAD~1` |
| Discard changes | `git restore <file>` |

---

*Prepared for the Git & CI/CD Workshop — BSCIT Session*
