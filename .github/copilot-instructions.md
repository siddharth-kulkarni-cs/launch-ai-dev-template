# GitHub Copilot Code Review Instructions

> Custom instructions for GitHub Copilot code review in this template repository.
> **Single Source of Truth:** All detailed standards live in `/ai/` directory.

---

## About This Repository

This is a **template repository** for TypeScript/Node.js projects with built-in AI development support.

**Key characteristics:**
- Template with placeholders: `{Repository Name}`, `{module-name}`, etc.
- AI development layer in `/ai/` directory (works across Cursor, Claude, Copilot)
- Skills-based documentation pattern for cross-IDE compatibility

**Before reviewing:** Read `/ai/README.md` for complete AI layer overview.

---

## Core Review Philosophy

### Review Context Sources

**ALWAYS reference these as source of truth:**

1. **Context Files** (understand the system):
   - `/ai/context/architecture.md` — system overview, boundaries, data flow
   - `/ai/context/domain.md` — business concepts and domain rules
   - `/ai/context/glossary.md` — terminology
   - `/ai/index.json` — machine-readable manifest

2. **Rules** (mandatory constraints):
   - `/ai/rules/dev-workflow.md` — code change lifecycle, validation commands
   - `/ai/rules/coding-conventions.md` — naming, formatting, structure

3. **Skills** (detailed implementation patterns):
   - `/ai/skills/testing-skill.md` — test patterns and requirements
   - `/ai/skills/tdd-skill.md` — test-driven development approach
   - `/ai/skills/code-style-skill.md` — style conventions with examples
   - `/ai/skills/security-skill.md` — security patterns and anti-patterns
   - `/ai/skills/commit-format-skill.md` — commit message standards
   - `/ai/skills/pr-review-skill.md` — comprehensive review checklist

### Review Priority

Review in this order of severity:

1. **🔴 Critical** — Security, data loss, breaking changes, missing critical tests
2. **🟡 Important** — Architecture violations, performance, error handling, style violations
3. **🟢 Nit** — Minor improvements, refactoring suggestions

---

## Quick Review Checklist

Use this as a fast reference. For detailed patterns and examples, consult the skill files.

### 1. Testing (CRITICAL)

**Requirements:**
- [ ] Every code change has tests (happy path + error path)
- [ ] Minimum 90% coverage on new/modified files
- [ ] Test names follow: `FeatureName > should {behavior} when {condition}`
- [ ] No test interdependencies
- [ ] Bug fixes include regression tests

**Reference:** `/ai/skills/testing-skill.md` for detailed test patterns

**Red Flags:**
- Missing tests for new functions/methods
- Tests with `console.log` or debugging artifacts
- Tests using `any` type
- Order-dependent or time-dependent tests

---

### 2. Code Style (REQUIRED)

**File & Code Naming:**
- [ ] Files: `kebab-case.ts`
- [ ] Classes/Interfaces: `PascalCase`
- [ ] Functions/variables: `camelCase`
- [ ] Constants: `UPPER_SNAKE_CASE`
- [ ] Named exports ONLY (no default exports)

**TypeScript:**
- [ ] No `any` type (use `unknown` and narrow)
- [ ] No `console.log` (use project logger)
- [ ] Imports properly grouped: node → external → internal

**Code Quality:**
- [ ] Functions under 50 lines
- [ ] No magic numbers or strings
- [ ] No unnecessary comments (code should be self-documenting)
- [ ] No code duplication (shared logic extracted)

**Reference:** `/ai/rules/coding-conventions.md` and `/ai/skills/code-style-skill.md`

---

### 3. Security (CRITICAL)

**Mandatory Checks:**
- [ ] No hardcoded secrets, tokens, or API keys
- [ ] Input validation on all external data
- [ ] No SQL/NoSQL injection vectors
- [ ] Auth/authz checks on protected endpoints
- [ ] No sensitive data in logs
- [ ] No `eval()` or dynamic code execution

**Reference:** `/ai/skills/security-skill.md` for patterns and anti-patterns

---

### 4. Commit Format (REQUIRED)

**Reference:** `/ai/skills/commit-format-skill.md`

---

### 5. Architecture (REQUIRED)

**Layer Boundaries:**
```
Controller → Service → Repository
```

**Rules:**
- [ ] Controllers handle HTTP, delegate to services
- [ ] Services contain business logic, NO HTTP types
- [ ] Repositories handle data access
- [ ] No circular dependencies
- [ ] Dependencies flow inward

**Reference:** `/ai/context/architecture.md` and `/ai/rules/dev-workflow.md`

---

### 6. Error Handling (REQUIRED)

**Requirements:**
- [ ] Use custom error classes (subclass of `AppError`)
- [ ] Never swallow errors (empty `catch {}`)
- [ ] Log errors with context before re-throwing
- [ ] Error cause chain preserved

**Anti-pattern:**
```typescript
// ❌ Swallowed error
try {
  await riskyOperation();
} catch (e) {}

// ✅ Proper error handling
try {
  await riskyOperation();
} catch (e) {
  logger.error('Operation failed', { error: e, context });
  throw new OperationError('Failed to complete', { cause: e });
}
```

---

### 7. Performance (IMPORTANT)

**Checks:**
- [ ] No N+1 queries (database calls in loops)
- [ ] Pagination on list endpoints
- [ ] Efficient database queries
- [ ] Proper use of caching

**Reference:** `/ai/rules/dev-workflow.md` (Performance Guardrails section)

---

## Documentation and Code Changes

### Critical Rules

**1. Do NOT create or modify markdown documentation files unless specifically requested**
- Documentation updates should be intentional, not automatic
- Before creating ANY `.md` file in a PR, ask if documentation is required
- Exception: Required files like CHANGELOG.md or explicitly requested docs

**2. Avoid unnecessary code comments**
- Comments should only explain "why", not "what"
- Code should be self-documenting through clear naming
- Remove comments that narrate what the code does

**3. Avoid code duplication**
- Check if similar logic exists elsewhere
- Extract shared patterns into reusable functions/modules
- Reference existing utility functions before creating new ones

---

## Critical Files — Extra Scrutiny

Require additional review for changes to:

- `package.json` — dependency changes need security review
- `*.config.js` — affects all environments
- `migrations/` — database migrations are irreversible
- `.env.example` — affects deployment
- `Dockerfile` — affects deployment

**Reference:** `/ai/index.json` for complete list of critical paths

---

## AI Skills System

### Reviewing Skill Files

When PRs add or modify files in `/ai/skills/`:

**File Requirements:**
- [ ] File named `{name}-skill.md` (kebab-case)
- [ ] Includes frontmatter with `name` and `description`
- [ ] Has "When to Use" section
- [ ] Contains concrete examples (not generic)
- [ ] Includes "Related Skills" section
- [ ] Has troubleshooting guidance

**Quality Checks:**
- [ ] Not too generic ("Write good code") ❌
- [ ] Not too specific (single function) ❌
- [ ] Actionable and concrete ✅
- [ ] Shows before/after patterns ✅
- [ ] Cross-referenced with related skills ✅

**Reference:** `/ai/skills/creating-skills-skill.md`

---

## Review Output Format

Structure review comments using this format:

```markdown
## Code Review Summary

### 🔴 Critical Issues
- **[file.ts:L42]** Description with specific fix
  - **Reference:** `/ai/skills/{relevant-skill}.md` (section name)

### 🟡 Important Issues  
- **[file.ts:L15]** Description
  - **Reference:** `/ai/rules/{relevant-rule}.md`

### 🟢 Suggestions
- **[file.ts:L88]** Minor improvement

### ✅ Positive Notes
- Well-structured tests following `/ai/skills/testing-skill.md`
- Good error handling pattern

### Verdict
[Approve / Request Changes / Needs Discussion]
```

---

## Common Pitfalls to Catch

### Unnecessary Documentation
```markdown
❌ Creating README updates or docs without being asked
❌ Adding markdown files automatically in PRs

✅ Only add/modify .md files when explicitly requested
✅ Ask before generating documentation
```

### Code Comments
```typescript
// ❌ Obvious, unnecessary comments
// Import the user service
import { UserService } from './user.service';

// Create a new user
const user = new User();

// ✅ Only comment non-obvious intent
// Using exponential backoff to handle rate limiting
const delay = Math.pow(2, retryCount) * 1000;
```

### Code Duplication
```typescript
// ❌ Duplicated validation logic across functions
function createUser(data) {
  if (!data.email || !data.email.includes('@')) throw new Error('Invalid email');
}
function updateUser(data) {
  if (!data.email || !data.email.includes('@')) throw new Error('Invalid email');
}

// ✅ Extract shared logic
function validateEmail(email: string) {
  if (!email || !email.includes('@')) throw new ValidationError('Invalid email');
}
```

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

Since this is a template, watch for unresolved placeholders:
- [ ] `{Repository Name}` not customized
- [ ] `{module-name}` still present
- [ ] `{One-line description}` not filled
- [ ] Generic documentation not customized for actual project

---

## Template-Specific Notes

When reviewing PRs in projects using this template:

1. **Check Template Customization:**
   - Has the project replaced placeholder values?
   - Are project-specific skills created?
   - Is `/ai/context/architecture.md` customized?

2. **Skill System Usage:**
   - Are new patterns documented as skills?
   - Are existing skills referenced in code reviews?
   - Do PR descriptions link to relevant skills?

3. **Cross-IDE Compatibility:**
   - Are instructions IDE-agnostic?
   - No tool-specific syntax in shared docs
   - Paths work from repository root

---

## Final Checklist

Before approving ANY PR:

- [ ] All tests pass (including new tests)
- [ ] Code coverage meets 90% threshold
- [ ] No linting or type errors
- [ ] Commit messages follow convention
- [ ] No security vulnerabilities introduced
- [ ] Layer boundaries respected (check `/ai/context/architecture.md`)
- [ ] Error handling is proper
- [ ] Documentation updated if needed (only when explicitly required)
- [ ] No hardcoded secrets (check `/ai/skills/security-skill.md`)
- [ ] Performance considerations addressed (check `/ai/rules/dev-workflow.md`)
- [ ] No unnecessary markdown files added without request
- [ ] No obvious/unnecessary code comments
- [ ] No code duplication (shared logic extracted)
- [ ] Validation commands pass (check `/ai/rules/dev-workflow.md`)

---

## Deep Dive References

For comprehensive guidance on each topic, consult:

### Core Skills
- **Testing Patterns:** `/ai/skills/testing-skill.md`
- **TDD Workflow:** `/ai/skills/tdd-skill.md`
- **Code Style:** `/ai/skills/code-style-skill.md`
- **Security Patterns:** `/ai/skills/security-skill.md`
- **Commit Standards:** `/ai/skills/commit-format-skill.md`
- **PR Review Checklist:** `/ai/skills/pr-review-skill.md`
- **Creating Skills:** `/ai/skills/creating-skills-skill.md`

### Rules & Context
- **Development Workflow:** `/ai/rules/dev-workflow.md`
- **Coding Conventions:** `/ai/rules/coding-conventions.md`
- **System Architecture:** `/ai/context/architecture.md`
- **Domain Context:** `/ai/context/domain.md`
- **Terminology:** `/ai/context/glossary.md`

### Machine-Readable
- **Manifest:** `/ai/index.json` — critical paths, commands, validation scripts

### Root Documentation
- **AI Agent Guidelines:** `/AGENTS.md` — quick reference for AI agents
- **Claude Guidelines:** `/CLAUDE.md` — Claude Code specific guidance

---

## Summary

**This file is a quick reference.** All detailed standards, patterns, and examples live in `/ai/` directory.

**Review flow:**
1. Read PR description and load context from `/ai/context/`
2. Review code against checklist above
3. For details, consult specific skill files in `/ai/skills/`
4. Reference exact file/section in review comments
5. Use severity markers (🔴 🟡 🟢) consistently

**Remember:** The `/ai/` directory is the single source of truth. Keep this file as a lightweight index, not a duplicate.
