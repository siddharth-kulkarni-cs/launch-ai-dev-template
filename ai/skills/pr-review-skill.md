---
name: pr-review
description: PR review checklist, feedback format, and review process. Use when reviewing PRs, preparing code for review, or understanding review standards.
---

# PR Review Skill

## When to Use
- Reviewing pull requests
- Preparing your code for review
- Understanding review standards
- Writing review feedback
- Self-reviewing before submitting PR

## Overview

Code review ensures quality, security, and maintainability. Every PR must pass review before merging.

## Review Process

### 1. Context Gathering
Before reviewing, load:
- [Testing Skill](/ai/skills/testing-skill.md) - Test patterns
- [Code Style Skill](/ai/skills/code-style-skill.md) - Style conventions
- [Security Skill](/ai/skills/security-skill.md) - Security patterns
- [Commit Format Skill](/ai/skills/commit-format-skill.md) - Commit standards

### 2. Review in Order of Severity

## Test Checklist

- [ ] Unit tests present for new/modified logic
- [ ] Happy path and error path covered
- [ ] Test names follow: `FeatureName > should {behavior} when {condition}`
- [ ] No test interdependencies or shared mutable state
- [ ] Bug fixes include regression tests
- [ ] Mocks are at the right level (mock services, not DB drivers)
- [ ] No flaky tests (time-dependent, order-dependent)

## Code Style Checklist

- [ ] File names: `kebab-case.ts`
- [ ] Named exports only (no default exports)
- [ ] No `any` type (use `unknown` and narrow)
- [ ] No `console.log` (use project logger)
- [ ] Import groups separated (node → external → internal)
- [ ] Constants in `UPPER_SNAKE_CASE`
- [ ] Functions under 50 lines
- [ ] No magic numbers/strings

## Security Checklist

- [ ] No hardcoded secrets, tokens, API keys
- [ ] Input validation on all external data
- [ ] No SQL/NoSQL injection vectors
- [ ] Auth/authz checks on protected endpoints
- [ ] No sensitive data in logs (PII, tokens)
- [ ] No `eval()` or dynamic code execution
- [ ] Dependencies checked for CVEs

## Commit Checklist

- [ ] Follows conventional commit format
- [ ] Each commit is atomic (one logical change)
- [ ] Commit messages explain "why" not just "what"
- [ ] No WIP or fixup commits in final PR
- [ ] Branch name follows convention

## Functional Review

- [ ] Code does what the PR description says
- [ ] Edge cases handled (null, empty, boundary)
- [ ] Error cases handled properly
- [ ] No breaking changes without documentation
- [ ] Performance considerations addressed

## Feedback Format

### Output Template
```markdown
## PR Review: {PR title}

### 🔴 Critical — Must Fix
- **[file.ts:L42]** {Description}
  ```typescript
  // Current (problematic)
  ...
  // Suggested fix
  ...
  ```

### 🟡 Important — Should Fix
- **[file.ts:L15]** {Description}

### 🟢 Nits — Optional
- **[file.ts:L88]** {Minor suggestion}

### ✅ Highlights
- {Positive notes on well-structured code}

### Summary
{1-2 sentence overall assessment}

**Verdict:** Approve / Request Changes / Needs Discussion
```

### Severity Levels

| Level | Icon | Meaning | Action |
|-------|------|---------|--------|
| Critical | 🔴 | Security, correctness, data loss | Must fix before merge |
| Important | 🟡 | Architecture, performance, maintainability | Should fix |
| Nit | 🟢 | Style, minor improvements | Optional |

## Common Review Patterns

### Check for Floating Promises
```typescript
// ❌ Fire-and-forget hides errors
someAsyncFunction();

// ✅ Awaited
await someAsyncFunction();

// ✅ Intentional fire-and-forget
void someAsyncFunction().catch(logger.error);
```

### Check for Error Swallowing
```typescript
// ❌ Swallowed error
try { ... } catch (e) { }

// ✅ Handled or re-thrown
try { ... } catch (e) {
  logger.error('Operation failed', { error: e });
  throw new OperationError('Failed', { cause: e });
}
```

### Check for N+1 Queries
```typescript
// ❌ N+1: one query per item
for (const id of orderIds) {
  const order = await orderRepo.findById(id);
}

// ✅ Batch query
const orders = await orderRepo.findByIds(orderIds);
```

### Check for Missing Validation
```typescript
// ❌ No validation
app.post('/orders', (req, res) => {
  const order = req.body; // Untrusted!
});

// ✅ Validated
app.post('/orders', (req, res) => {
  const order = OrderSchema.parse(req.body);
});
```

## Common Pitfalls

❌ **Problem:** Reviewing only the diff, not the context
✅ **Solution:** Read surrounding code to understand impact

❌ **Problem:** Nitpicking style over substance
✅ **Solution:** Focus on correctness and security first

❌ **Problem:** Vague feedback ("this looks wrong")
✅ **Solution:** Be specific with line numbers and suggestions

❌ **Problem:** Not testing the changes locally
✅ **Solution:** Pull the branch and run tests for complex changes

❌ **Problem:** Rubber-stamping PRs
✅ **Solution:** Take time to understand the changes

## Related Skills
- [Testing](/ai/skills/testing-skill.md) - Test patterns
- [Code Style](/ai/skills/code-style-skill.md) - Style conventions
- [Security](/ai/skills/security-skill.md) - Security patterns
- [Commit Format](/ai/skills/commit-format-skill.md) - Commit standards

## Troubleshooting

**Issue:** PR is too large to review effectively
**Fix:** Ask author to split into smaller PRs

**Issue:** Tests pass but code has issues
**Fix:** Check test quality - are edge cases covered?

**Issue:** Disagreement on approach
**Fix:** Discuss in PR comments, escalate to team if needed
