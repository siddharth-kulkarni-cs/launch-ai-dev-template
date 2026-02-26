# PR Review Checklist

> Quick-reference checklist for code reviews. Used by `Skills/code-review-skill/skill.md`.

---

## Before Approving, Verify:

### Security
- [ ] No hardcoded secrets or credentials
- [ ] Input validation on all external data
- [ ] No NoSQL injection vectors
- [ ] Auth checks on protected routes
- [ ] No sensitive data in logs

### Correctness
- [ ] All async operations awaited
- [ ] Errors handled, not swallowed
- [ ] Null/undefined edge cases covered
- [ ] Race conditions addressed

### Architecture
- [ ] Controller → Service → Repository layering respected
- [ ] No business logic in controllers
- [ ] No HTTP types in services
- [ ] Custom `AppError` used (not raw `Error`)
- [ ] API response uses standard envelope

### Performance
- [ ] No N+1 queries (DB calls in loops)
- [ ] `.lean()` on read-only Mongoose queries
- [ ] Pagination on list endpoints
- [ ] Redis cache for hot paths

### Testing
- [ ] Unit tests for new/changed logic
- [ ] Happy path + error path covered
- [ ] Bug fix includes regression test
- [ ] No test interdependencies

### Conventions
- [ ] File names: `kebab-case.ts`
- [ ] Named exports (no defaults)
- [ ] No `any` types
- [ ] No `console.log` (use logger)
- [ ] Conventional commit message format
