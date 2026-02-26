# Code Review Skill

## Purpose

Automated code review for pull requests using specialized review patterns. Analyzes code for quality, security, performance, and best practices. Use when reviewing code changes, PRs, or doing code audits.

---

## Review Process

### 1. Context Gathering

Before reviewing, always load:
- `.cursor/skills/skill.md` — repo-wide patterns (layering, error handling, conventions)
- `Skills/{affected-module}-skill/skill.md` — module-specific patterns (if exists)
- This file's `references/common-pitfalls.md` — known anti-patterns
- This file's `references/review-checklist.md` — structured checklist

### 2. Review Categories

Review every change against these categories in order of severity:

#### 🔴 Security (Critical)
- No hardcoded secrets, tokens, API keys, or credentials. Secrets should be fetched from environment/configuration files.
- Input validation present on all external data (request bodies, query params, headers)
- No NoSQL injection vectors — always use parameterized queries with Mongoose
- Auth/authz checks on protected endpoints
- No sensitive data in logs (PII, tokens, passwords)
- No `eval()`, `Function()`, or dynamic code execution
- Dependencies don't have known critical CVEs
- No XSS (cross-site scripting) risks
- Unsafe deserialization
- Path traversal vulnerabilities
- CSRF protection
- Input validation gaps
- Insecure cryptography
- Dependency vulnerabilities
- 

#### 🔴 Correctness (Critical)
- Async operations properly awaited (no floating promises)
- Error cases handled, not swallowed (no empty `catch {}`)
- Null/undefined checks for optional data
- Race conditions addressed (especially Redis lock patterns)
- Edge cases: empty arrays, zero values, boundary conditions
- MongoDB ObjectId validated before query
- Proper use of transactions where data consistency matters

#### 🟡 Architecture (Important)
- Follows controller → service → repository layering
- No business logic in controllers
- No HTTP types (Request, Response) in services
- Dependencies flow in the right direction (no circular imports)
- New modules follow the established structure
- Errors use `AppError` subclasses, not raw `Error`
- API responses follow the standard envelope

#### 🟡 Performance (Important)
- No N+1 query patterns (DB calls inside loops)
- Queries use appropriate indexes
- `.lean()` used for read-only MongoDB queries
- Redis cache used where expected (hot paths, expensive computations)
- Pagination on list endpoints (no unbounded queries)
- No synchronous blocking operations in async hot paths
- Bulk operations used instead of sequential single-item operations

#### 🟢 Testing (Standard)
- Unit tests present for new/modified logic
- Happy path and error path covered
- Test names follow: `should {behavior} when {condition}`
- No test interdependencies or shared mutable state
- Bug fixes include regression tests
- Mocks are at the right level (mock repositories, not mongoose directly)

#### 🟢 Conventions (Standard)
- File naming: `kebab-case.ts`
- Named exports only (no default exports)
- No `any` type (use `unknown` and narrow)
- No `console.log` (use project logger)
- Import groups separated (node → external → internal)
- Constants in `UPPER_SNAKE_CASE`
- Zod schemas for request validation

---

## 3. Output Format

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
{1-2 sentence overall assessment: approve, request changes, or needs discussion}
```

---

## 4. Common Review Patterns

### Pattern: Check for floating promises
```typescript
// ❌ Floating promise — fire-and-forget can hide errors
someAsyncFunction();

// ✅ Awaited
await someAsyncFunction();

// ✅ Intentional fire-and-forget (explicit void)
void someAsyncFunction().catch(logger.error);
```

### Pattern: Check for proper error propagation
```typescript
// ❌ Swallowed error
try { ... } catch (e) { }

// ❌ Generic error
throw new Error('Something went wrong');

// ✅ Domain error with context
throw new OrderNotFoundError(orderId);
```

### Pattern: Check for N+1 queries
```typescript
// ❌ N+1: one query per item
for (const id of orderIds) {
  const order = await orderRepo.findById(id);
}

// ✅ Batch query
const orders = await orderRepo.findByIds(orderIds);
```

### Pattern: Check for missing `.lean()`
```typescript
// ❌ Returns full Mongoose document (heavy, with change tracking)
const orders = await Order.find({ userId });

// ✅ Returns plain JS object (faster, lower memory)
const orders = await Order.find({ userId }).lean();
```