---
name: using-feature-branches
description: Use when starting feature work that needs isolation - creates feature branches with clean baseline verification and structured workflow
---

# Using Feature Branches

## Overview

Use standard git branches for feature isolation. This provides sufficient isolation with a simpler workflow.

**Core principle:** Standard branches provide isolation for feature development.

**Announce at start:** "I'm using the using-feature-branches skill to set up a feature branch."

## Branch Workflow

### Step 1: Ensure Clean State

```bash
# Check for uncommitted changes
git status

# Stash if needed
git stash push -m "WIP before feature branch"
```

### Step 2: Update Base Branch

```bash
git checkout main
git pull origin main
```

### Step 3: Create Feature Branch

```bash
git checkout -b feature/<feature-name>
```

**Branch naming conventions:**
- `feature/<name>` - New features
- `fix/<name>` - Bug fixes
- `refactor/<name>` - Code refactoring

### Step 4: Verify Clean Baseline

Run tests to ensure you start clean:

```bash
# Use project-appropriate command
npm test        # Node.js
pytest          # Python
cargo test      # Rust
go test ./...   # Go
```

**If tests fail:** Report failures, ask whether to proceed or investigate.

**If tests pass:** Report ready.

### Step 5: Report Ready

```
Feature branch ready: feature/<feature-name>
Base: main (up to date)
Tests passing (<N> tests, 0 failures)
Ready to implement <feature-name>
```

## Quick Reference

| Situation | Action |
|-----------|--------|
| Starting new feature | Create branch from updated main |
| Uncommitted changes exist | Stash or commit before branching |
| Tests fail on clean branch | Investigate before proceeding |
| Need to switch context | Commit/stash, checkout other branch |
| Main has updates | Rebase or merge origin/main |

## Context Switching

When you need to work on something else:

```bash
# Option 1: Commit current work
git add -A && git commit -m "WIP: description"
git checkout other-branch

# Option 2: Stash current work
git stash push -m "WIP: feature-name"
git checkout other-branch

# Return later
git checkout feature/<feature-name>
git stash pop  # if stashed
```

## Keeping Branch Updated

Periodically sync with main to avoid large merge conflicts:

```bash
git fetch origin
git rebase origin/main
# OR
git merge origin/main
```

## Common Mistakes

**Starting from outdated main**
- **Problem:** Feature branch diverges significantly, painful merge later
- **Fix:** Always pull main before creating feature branch

**Not verifying clean baseline**
- **Problem:** Can't distinguish new bugs from pre-existing issues
- **Fix:** Run tests before starting work

**Large uncommitted changes when switching**
- **Problem:** Risk losing work, messy stash history
- **Fix:** Commit WIP or use meaningful stash messages

## Integration

**Called by:**
- **brainstorming** (Phase 4) - When design is approved and implementation follows
- Any skill needing isolated workspace

**Pairs with:**
- **finishing-a-development-branch** - For completing work
- **executing-plans** or **subagent-driven-development** - Work happens on this branch
