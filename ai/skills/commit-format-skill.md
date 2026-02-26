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

This repository follows [Conventional Commits](https://www.conventionalcommits.org/) for consistent, parseable commit history.

## Commit Message Format

```
<type>(<scope>): <subject>

[optional body]

[optional footer]
```

### Types
| Type | When to Use |
|------|-------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, no code change |
| `refactor` | Code change that neither fixes nor adds |
| `perf` | Performance improvement |
| `test` | Adding or updating tests |
| `chore` | Build, CI, dependencies |
| `revert` | Reverting a previous commit |

### Scope
The scope indicates the module or area affected:
- `orders` - Order module
- `users` - User module
- `auth` - Authentication
- `api` - API layer
- `db` - Database
- `ci` - CI/CD pipeline

### Subject
- Use imperative mood: "add" not "added" or "adds"
- No period at the end
- Max 50 characters
- Lowercase first letter

## Good Commit Examples

```bash
# Feature
feat(orders): add bulk cancellation endpoint

# Bug fix
fix(payments): handle race condition in refund processing

# With body for context
fix(auth): prevent session fixation on login

The session ID was not being regenerated after successful
authentication, allowing potential session fixation attacks.

Closes #1234

# Test
test(orders): add integration tests for status transitions

# Refactor
refactor(users): extract validation logic to separate module

# Breaking change
feat(api)!: change response envelope format

BREAKING CHANGE: API responses now use { data, meta, errors }
format instead of { result, pagination }.
```

## Bad Commit Examples

```bash
# ❌ No type
"fixed the bug"

# ❌ Past tense
"feat(orders): added new endpoint"

# ❌ Too vague
"fix: updates"

# ❌ Too long
"feat(orders): add new endpoint for bulk order cancellation with support for filtering by date range and status"

# ❌ Multiple changes in one commit
"feat(orders): add cancellation and fix validation and update tests"
```

## Get Branch Name

When working on a feature, use this format:
```
<type>/<ticket-id>-<short-description>
```

Examples:
```bash
feat/JIRA-1234-bulk-cancellation
fix/JIRA-5678-refund-race-condition
chore/JIRA-9012-upgrade-dependencies
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
git checkout -b feat/JIRA-1234-new-feature

# 2. Make atomic commits
git add src/orders/order.service.ts
git commit -m "feat(orders): add validation for bulk operations"

git add src/orders/__tests__/order.service.test.ts
git commit -m "test(orders): add tests for bulk validation"

# 3. Push and create PR
git push -u origin feat/JIRA-1234-new-feature
```

### Fixing Commits
```bash
# Amend last commit (before push)
git commit --amend -m "feat(orders): add validation for bulk operations"

# Interactive rebase to squash (before push)
git rebase -i HEAD~3
```

## Common Pitfalls

❌ **Problem:** Committing unrelated changes together
```bash
# Bad: Multiple unrelated changes
git commit -m "feat(orders): add endpoint and fix user bug"
```
✅ **Solution:** Make separate commits for each logical change

❌ **Problem:** Vague commit messages
```bash
# Bad
git commit -m "fix: stuff"
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
