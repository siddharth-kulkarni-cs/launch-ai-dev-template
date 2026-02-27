# GitHub Copilot Code Review Instructions

> Custom instructions for GitHub Copilot code review in this template repository.
> These instructions help Copilot understand the project standards and review PRs effectively.

---

## Repository Overview

This is a **template repository** for TypeScript/Node.js projects with built-in AI development support. It provides a standardized structure for AI-assisted development with skills, rules, and commands that can be customized for specific projects.

**Key Characteristics:**
- Template placeholders: `{Repository Name}`, `{module-name}`, `{One-line description}`
- AI development layer in `/ai` directory
- Skills-based documentation pattern
- Cross-IDE compatibility (Cursor, Claude, GitHub Copilot)

---

## Review Focus Areas

### 1. Testing Standards (CRITICAL)

**Requirements:**
- Every code change requires tests (happy path + error path)
- Minimum 90% code coverage on new/modified files
- Test-driven development (TDD) is the default approach
- Bug fixes must include regression tests

**Test Naming Convention:**
```
FeatureName > should {behavior} when {condition}
```

**What to Check:**
- [ ] Tests exist for new/modified code
- [ ] Happy path AND error path covered
- [ ] Test names follow convention
- [ ] No test interdependencies
- [ ] No flaky tests (time/order dependent)
- [ ] Mocks are at the right level (services, not DB drivers)

**Red Flags:**
- Missing tests for new functions/methods
- Tests that depend on execution order
- Tests with `console.log` or debugging artifacts
- Using `any` type in test code

---

### 2. Code Style (REQUIRED)

**File Naming:**
- Use `kebab-case.ts` for all TypeScript files
- No camelCase or PascalCase file names

**Code Conventions:**
- Classes: `PascalCase`
- Functions/variables: `camelCase`
- Constants: `UPPER_SNAKE_CASE`
- Named exports ONLY (no default exports)

**TypeScript:**
- Strict mode enabled
- No `any` type (use `unknown` and narrow)
- No `console.log` (use project logger)

**Import Organization:**
```typescript
// Node.js built-ins
import { readFile } from 'fs/promises';

// External dependencies
import { Injectable } from '@nestjs/common';

// Internal modules
import { UserService } from '../user/user.service';
```

**What to Check:**
- [ ] File names are kebab-case
- [ ] Only named exports used
- [ ] No `any` types introduced
- [ ] No `console.log` statements
- [ ] Imports properly grouped and sorted
- [ ] Functions under 50 lines
- [ ] No magic numbers or strings

---

### 3. Security (CRITICAL)

**Mandatory Checks:**
- [ ] No hardcoded secrets, tokens, or API keys
- [ ] Input validation on all external data
- [ ] No SQL/NoSQL injection vectors
- [ ] Auth/authz checks on protected endpoints
- [ ] No sensitive data in logs (PII, tokens)
- [ ] No `eval()` or dynamic code execution
- [ ] Dependencies checked for known vulnerabilities

**Red Flags:**
- Raw SQL queries with string concatenation
- Missing input validation on API endpoints
- Secrets in code or comments
- Logging sensitive user data
- Missing authentication checks

---

### 4. Commit Format (REQUIRED)

**Format:**
```
<type>(<scope>): <subject>

[optional body]
```

**Valid Types:**
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation only
- `style` - Code style (formatting, no logic change)
- `refactor` - Code restructuring (no behavior change)
- `perf` - Performance improvement
- `test` - Adding or updating tests
- `chore` - Maintenance tasks

**What to Check:**
- [ ] Commits follow conventional format
- [ ] Each commit is atomic (one logical change)
- [ ] Commit messages explain "why" not just "what"
- [ ] No WIP or fixup commits in final PR
- [ ] Subject line is concise (<50 chars)

---

### 5. Architecture & Layer Boundaries

**Standard Layers:**
```
Controller → Service → Repository
```

**Rules:**
- Controllers handle HTTP, delegate to services
- Services contain business logic, NO HTTP types
- Repositories handle data access
- Dependencies flow inward
- No circular imports

**What to Check:**
- [ ] Layer boundaries respected
- [ ] No business logic in controllers
- [ ] No HTTP types (Request, Response) in services
- [ ] No circular dependencies
- [ ] Shared code in `/shared` directory

**Common Violations:**
```typescript
// ❌ BAD: Business logic in controller
@Controller('users')
export class UserController {
  @Post()
  async create(req: Request) {
    // Validation logic here (should be in service)
    const user = await this.db.save(req.body);
  }
}

// ✅ GOOD: Delegate to service
@Controller('users')
export class UserController {
  constructor(private userService: UserService) {}
  
  @Post()
  async create(@Body() createDto: CreateUserDto) {
    return this.userService.create(createDto);
  }
}
```

---

### 6. Error Handling

**Requirements:**
- Use custom error classes (subclass of `AppError`)
- Never swallow errors (empty `catch {}`)
- Log errors with context before re-throwing
- Include error cause chain

**What to Check:**
- [ ] Custom error classes used
- [ ] No empty catch blocks
- [ ] Errors logged with context
- [ ] Error cause chain preserved

**Anti-patterns:**
```typescript
// ❌ Swallowed error
try {
  await riskyOperation();
} catch (e) {}

// ❌ Lost error context
try {
  await riskyOperation();
} catch (e) {
  throw new Error('Operation failed');
}

// ✅ Proper error handling
try {
  await riskyOperation();
} catch (e) {
  logger.error('Operation failed', { error: e, context });
  throw new OperationError('Failed to complete', { cause: e });
}
```

---

### 7. Performance Considerations

**What to Check:**
- [ ] No N+1 queries (database calls in loops)
- [ ] Pagination on list endpoints
- [ ] Efficient database queries (indexes considered)
- [ ] No unnecessary data fetching
- [ ] Proper use of caching where applicable

**N+1 Query Detection:**
```typescript
// ❌ N+1 query
for (const orderId of orderIds) {
  const order = await orderRepo.findById(orderId);
}

// ✅ Batch query
const orders = await orderRepo.findByIds(orderIds);
```

---

## AI Development Layer

### Skills System

This template includes a `/ai/skills/` directory with reusable skill documentation. When reviewing PRs that add or modify skills:

**Skill File Requirements:**
- [ ] File named `{name}-skill.md` (kebab-case)
- [ ] Includes frontmatter with `name` and `description`
- [ ] Has "When to Use" section
- [ ] Contains concrete examples
- [ ] Includes "Related Skills" section
- [ ] Has troubleshooting guidance

**Skill Quality Checks:**
- [ ] Not too generic ("Write good code")
- [ ] Not too specific (single function)
- [ ] Actionable and concrete
- [ ] Examples show before/after patterns
- [ ] Cross-referenced with related skills

---

**In Code Review Comments:**
- Reference specific validation failures
- Suggest fixes with code examples
- Link to relevant skill files

---

## Critical Files - Extra Scrutiny Required

When reviewing changes to these files, require additional review:

- `package.json` - Dependency changes need security review
- `*.config.js` - Configuration affects all environments
- `migrations/` - Database migrations are irreversible
- `.env.example` - Environment changes affect deployment
- `Dockerfile` - Container changes affect deployment

---

## Review Severity Levels

Use these severity markers in review comments:

### 🔴 Critical - Must Fix Before Merge
- Security vulnerabilities
- Data loss risks
- Breaking changes without migration
- Missing critical tests
- Hardcoded secrets

### 🟡 Important - Should Fix
- Architectural violations
- Performance issues
- Incomplete error handling
- Style violations
- Missing documentation

### 🟢 Nit - Optional
- Minor style improvements
- Refactoring suggestions
- Non-critical optimizations

---

## Review Output Format

Structure review comments using this format:

```markdown
## Code Review Summary

### 🔴 Critical Issues
- **[file.ts:L42]** Description with specific fix

### 🟡 Important Issues  
- **[file.ts:L15]** Description

### 🟢 Suggestions
- **[file.ts:L88]** Minor improvement

### ✅ Positive Notes
- Well-structured tests
- Good error handling

### Verdict
[Approve / Request Changes / Needs Discussion]
```

---

## Common Pitfalls to Catch

### Floating Promises
```typescript
// ❌ Fire-and-forget hides errors
someAsyncFunction();

// ✅ Properly handled
await someAsyncFunction();
// OR
void someAsyncFunction().catch(logger.error);
```

### Missing Input Validation
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

### Template Placeholder Detection

Since this is a template, watch for:
- [ ] Unresolved `{Repository Name}` placeholders
- [ ] `{module-name}` not customized
- [ ] `{One-line description}` still present
- [ ] Generic documentation not customized

---

## Skill File References

For deeper context on standards, refer reviewers to:

- **Testing:** `/ai/skills/testing-skill.md`
- **TDD:** `/ai/skills/tdd-skill.md`
- **Code Style:** `/ai/skills/code-style-skill.md`
- **Security:** `/ai/skills/security-skill.md`
- **Commit Format:** `/ai/skills/commit-format-skill.md`
- **PR Review:** `/ai/skills/pr-review-skill.md`
- **Creating Skills:** `/ai/skills/creating-skills-skill.md`

**Note:** These skill files contain detailed examples and troubleshooting guidance.

---

## Template-Specific Notes

When reviewing PRs in projects using this template:

1. **Check Template Customization:**
   - Has the project replaced placeholder values?
   - Are project-specific skills created?
   - Is architecture.md customized?

2. **Skill System Usage:**
   - Are new patterns documented as skills?
   - Are existing skills referenced in code reviews?
   - Do PR descriptions link to relevant skills?

3. **Cross-IDE Compatibility:**
   - Are instructions IDE-agnostic?
   - No Cursor-specific syntax in shared docs
   - Paths work from repository root

---

## Final Checklist

Before approving ANY PR:

- [ ] All tests pass (including new tests)
- [ ] Code coverage meets 90% threshold
- [ ] No linting or type errors
- [ ] Commit messages follow convention
- [ ] No security vulnerabilities introduced
- [ ] Layer boundaries respected
- [ ] Error handling is proper
- [ ] Documentation updated if needed
- [ ] No hardcoded secrets
- [ ] Performance considerations addressed

---

## References

- [Development Workflow Rules](/ai/rules/dev-workflow.md)
- [Coding Conventions](/ai/rules/coding-conventions.md)
- [PR Review Skill](/ai/skills/pr-review-skill.md)
- [AGENTS.md](/AGENTS.md) - AI agent guidelines
- [CLAUDE.md](/CLAUDE.md) - Claude Code guidelines
