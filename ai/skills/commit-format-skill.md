---
name: commit-format
description: Commit message format, branching workflow, and git practices. Use when committing changes, creating branches, or reviewing commit history.
---

# Commit Format Skill

## When to Use
- Writing commit messages
- Creating feature branches
- Reviewing commit history
- Squashing or rebasing commits
- Preparing PRs for merge

## Overview

This repository uses branch-name-based commit messages for traceability and consistency. The branch name is the Jira ticket ID, while the subject describes what changed.

## Commit Message Format

```
<git-branch-name> | <subject>

[optional body]

[optional footer]
```

### Format Components

**Branch name:** Jira ticket ID (e.g., `CL-1234`)

**Subject:**
- Use imperative mood: "add" not "added" or "adds"
- No period at the end
- Max 72 characters
- Lowercase first letter
- Describe the change clearly
- Include type context if helpful (e.g., "fix:", "feat:", "refactor:")

## Good Commit Examples

```bash
# Feature
CL-1234 | add bulk cancellation endpoint

# Bug fix
CL-5678 | fix race condition in refund processing

# With body for context
CL-9012 | prevent session fixation on login

The session ID was not being regenerated after successful
authentication, allowing potential session fixation attacks.

Closes #9012

# Test
CL-1234 | add integration tests for status transitions

# Refactor
CL-3456 | extract validation logic to separate module

# With type prefix in subject for clarity
CL-7890 | feat: implement user role management
CL-4567 | fix: handle null pointer in payment service
CL-2345 | refactor: simplify order validation logic

# Multiple commits on same branch
CL-1234 | add validation for bulk operations
CL-1234 | implement cancellation service method
CL-1234 | add controller endpoint
```

## Bad Commit Examples

```bash
# ❌ No branch name prefix
"added new endpoint"

# ❌ Past tense
"CL-1234 | added new endpoint"

# ❌ Too vague
"CL-5678 | updates"

# ❌ Too long
"CL-1234 | add new endpoint for bulk order cancellation with support for filtering by date range and status and validation"

# ❌ Multiple unrelated changes in one commit
"CL-1234 | add cancellation and fix validation and update tests"

# ❌ Missing pipe separator
"CL-1234 add endpoint"

# ❌ Wrong ticket format
"jira-1234 | add feature"  # Should be CL-1234
```

## Branch Naming

Branches are named after the Jira ticket ID:
```
<ticket-id>
```

Examples:
```bash
CL-1234
CL-5678
CL-9012
```

## When to Commit

### Do Commit
- After completing a logical unit of work
- Before switching context
- When tests pass for the change
- After fixing a bug (separate from feature work)

### Don't Commit
- Broken code that doesn't compile
- Failing tests (unless intentionally marking as failing)
- Debug statements or console.logs
- Unrelated changes mixed together

## Workflow

### Feature Development
```bash
# 1. Create branch from main
git checkout main
git pull origin main
git checkout -b CL-1234

# 2. Make atomic commits (branch name provides ticket traceability)
git add src/orders/order.service.ts
git commit -m "CL-1234 | add validation for bulk operations"

git add src/orders/__tests__/order.service.test.ts
git commit -m "CL-1234 | add tests for bulk validation"

# 3. Push and create PR
git push -u origin CL-1234
```

### Getting Current Branch Name
```bash
# Use in commit message
git branch --show-current

# Example commit with branch name
BRANCH=$(git branch --show-current)
git commit -m "$BRANCH | add validation logic"
```

### Fixing Commits
```bash
# Amend last commit (before push)
BRANCH=$(git branch --show-current)
git commit --amend -m "$BRANCH | add validation for bulk operations"

# Interactive rebase to squash (before push)
git rebase -i HEAD~3
```

## Common Pitfalls

❌ **Problem:** Forgetting branch name prefix
```bash
# Bad: Missing ticket ID
git commit -m "add validation logic"
```
✅ **Solution:** Always include ticket ID from branch
```bash
git commit -m "CL-1234 | add validation logic"
```

❌ **Problem:** Committing unrelated changes together
```bash
# Bad: Multiple unrelated changes
git commit -m "CL-1234 | add endpoint and fix user bug"
```
✅ **Solution:** Make separate commits for each logical change

❌ **Problem:** Vague commit messages
```bash
# Bad
git commit -m "CL-5678 | stuff"
```
✅ **Solution:** Be specific about what changed and why

❌ **Problem:** Committing generated files
```bash
# Bad: Committing build output
git add dist/
```
✅ **Solution:** Ensure `.gitignore` excludes generated files

❌ **Problem:** Large commits that are hard to review
✅ **Solution:** Break into smaller, focused commits

❌ **Problem:** Force pushing to shared branches
```bash
# Bad
git push --force origin main
```
✅ **Solution:** Only force push to your own feature branches

❌ **Problem:** Using wrong separator
```bash
# Bad: Using colon instead of pipe
git commit -m "CL-1234: add feature"
```
✅ **Solution:** Use pipe separator `|`
```bash
git commit -m "CL-1234 | add feature"
```

❌ **Problem:** Wrong ticket format
```bash
# Bad: Lowercase or wrong prefix
git commit -m "cl-1234 | add feature"
git commit -m "JIRA-1234 | add feature"
```
✅ **Solution:** Use correct format `CL-XXXX`
```bash
git commit -m "CL-1234 | add feature"
```

## Related Skills
- [Code Style](/ai/skills/code-style-skill.md) - Naming conventions
- [PR Review](/ai/skills/pr-review-skill.md) - Commit review checklist
- [Testing](/ai/skills/testing-skill.md) - When to commit tests

## Troubleshooting

**Issue:** Commit rejected by pre-commit hook
**Fix:** Run `npm run lint` and `npm run test` before committing

**Issue:** Need to undo a commit
**Fix:** Use `git revert <sha>` for pushed commits, `git reset` for local

**Issue:** Commit message doesn't match format
**Fix:** Use `git commit --amend` to fix (if not pushed)

**Issue:** Forgot to include ticket ID
**Fix:** Amend the commit with correct format:
```bash
BRANCH=$(git branch --show-current)
git commit --amend -m "$BRANCH | <subject>"
```

**Issue:** Not sure which ticket ID to use
**Fix:** Check your current branch:
```bash
git branch --show-current  # Shows: CL-1234
```
