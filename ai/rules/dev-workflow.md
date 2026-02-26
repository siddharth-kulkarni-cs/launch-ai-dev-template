# Development Workflow Rules

> These rules are automatically loaded by Cursor and apply to all AI-assisted development
> in this repository. Follow them for every code change.

---

## 1. Code Change Lifecycle

Every code change must follow this sequence:

1. **Understand** — Read the relevant skill file(s) before writing code.
2. **Implement** — Write code following the patterns in `.skills/` folder.
3. **Test** — Write or update unit tests for every change (see Testing Rules below).
4. **Validate** — Run the validation commands after code generation.
5. **Review** — Self-review against the code review skill checklist in `skills/code-review` skill.

---

## 2. Skill File References

Before working on any module, read the relevant skill files:

| Scope | Location | When to Read |
|---|---|---|
| Repo-wide patterns | `skills/skill.md` | Always — before any code change |
| Module-specific | `skills/{module}-skill/skill.md` | When working in that module |
| Code review | `skills/code-review/skill.md` | Follow the code style guidelines in this skill while making changes to the codebase |

**If a skill file exists for the area you're working in, you must follow it.**

---

## 3. Testing Rules

### Unit Tests
- **Every new function/method** gets at least one happy-path and one error-path test.
- **Every bug fix** gets a regression test that would have caught the bug.
- **Test files** are colocated: `{entity}.{layer}.spec.ts`
  (e.g., {example test file}).
- **No test** should depend on another test's execution or state.

### Test Naming
**ALWAYS prefix test descriptions with the feature name** for traceability and easy filtering.

**Format:** `FeatureName > description of what is being tested`

**Getting the Feature Name:**
1. If provided in the task/story (e.g., JIRA ticket), use it: `CMA-1234 >` or `EmptyBin >`
2. If not provided, **ASK THE USER** before writing tests:
   > "What feature name should I use for test prefixes?"
3. Use descriptive names like: `EmptyBin`, `EntryRestore`, `ContentTypeVersioning`

```javascript
// ✅ CORRECT - Feature-prefixed test names
describe('EmptyBin > checkEmptyBinPlan', () => {
  it('EmptyBin > should reject with 422 error when retentionPeriod is not set', () => {
    // Test implementation
  });
  
  it('EmptyBin > should resolve when emptyBin feature is enabled', () => {
    // Test implementation
  });
});

// ❌ INCORRECT - No feature prefix
describe('checkEmptyBinPlan', () => {
  it('should reject with 422 error', () => {
    // Hard to trace back to feature
  });
});
```

### Running Tests
```bash
# Unit tests (fast, no external deps)
npm run test:unit

# Integration tests (requires Docker for MongoDB/Redis)
npm run test:integration

# Single file
npm run test -- --testPathPattern={filename}

# With coverage
npm run test:coverage

# With featurename
npm run test: Featurename
```

### Coverage Requirements
- Minimum **90% line coverage** on all new/modified files.
- CI will block merges that drop below threshold.

---

## 4. Naming Conventions

| Item | Convention | Example |
|---|---|---|
| Files | `kebab-case.ts` | `order-service.ts` |
| Classes | `PascalCase` | `OrderService` |
| Interfaces | `PascalCase` (no `I` prefix) | `OrderRepository` |
| Types | `PascalCase` | `CreateOrderInput` |
| Functions / methods | `camelCase` | `calculateTotal()` |
| Constants | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| Env variables | `UPPER_SNAKE_CASE` | `DATABASE_URL` |
| DB collections | `plural_snake_case` | `order_items` |
| Redis keys | `{service}:{entity}:{id}` | `orders:cache:abc123` |
| Test files | `{entity}.{layer}.test.ts` | `order.service.test.ts` |
| Skill directories | `{name}-skill/` | `payments-skill/` |

---

## 5. Validation Commands

Run these before every commit:

```bash
# Lint
npm run lint

# Type check
npx tsc --noEmit

# Unit tests
npm run test:unit

# Full validation (CI-equivalent)
npm run validate
```

**If any command fails, fix it before proceeding.**

---

## 6. Commit & PR Rules

### Commit Messages
Follow [Conventional Commits](https://www.conventionalcommits.org/):
```
feat(orders): add bulk order cancellation endpoint
fix(payments): handle race condition in refund processing
test(orders): add integration tests for order status transitions
docs(skills): update order module skill with new caching pattern
```

### PR Requirements
- [ ] All validation commands pass
- [ ] Unit tests written/updated for changes
- [ ] Relevant skill files updated if patterns changed
- [ ] PR description explains **what** and **why**
- [ ] No `console.log` statements left in production code
- [ ] No `any` types introduced (use `unknown` and narrow)

---

## 7. Skill Maintenance

### When to Update Skills
- You introduce a **new pattern or convention** → update `skills/skill.md`
- You add a **new module** → create `skills/{module}-skill/skill.md`
- You discover a **new gotcha** → add to the relevant `common-pitfalls.md` to references in skills and update `skills/skill.md`
- You change **architecture** (new layer, new dependency) → update relevant skill files

### When to Create a New Skill
- A module has **3+ developers** working on it regularly
- The module has **domain-specific patterns** not covered by the repo-wide skill
- You find yourself **repeatedly explaining** the same thing to AI or new devs

### Skill File Quality Bar
- Under **300 lines** (link to references/ for detail)
- Contains **code snippets** (5-15 lines) not just prose
- Has a **Common Tasks** section with step-by-step recipes
- Has a **Pitfalls** section with negative examples ("Do NOT do X")

### When a new Skill is added
 - update the reference to the skill in `Readme.md` and `skills/skills.md` with proper description.