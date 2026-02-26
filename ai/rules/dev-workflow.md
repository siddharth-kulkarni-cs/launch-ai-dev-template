# Development Workflow Rules

> These rules apply to all AI-assisted development in this repository.
> Follow them for every code change.

---

## Code Change Lifecycle

Every code change must follow this sequence:

1. **Understand** — Read relevant skill files before writing code
2. **Implement** — Write code following patterns in `/ai/skills/`
3. **Test** — Write or update tests for every change
4. **Validate** — Run validation commands after code generation
5. **Review** — Self-review against PR review skill checklist

---

## Skill File References

Before working on any area, read the relevant skills:

| Scope | Skill | When to Read |
|-------|-------|--------------|
| Testing | `/ai/skills/testing-skill.md` | Writing or updating tests |
| Code Style | `/ai/skills/code-style-skill.md` | Writing any code |
| Commits | `/ai/skills/commit-format-skill.md` | Making commits |
| Reviews | `/ai/skills/pr-review-skill.md` | Before submitting PR |
| Security | `/ai/skills/security-skill.md` | Handling auth, secrets, input |
| New Skills | `/ai/skills/creating-skills-skill.md` | Adding documentation |

**If a skill file exists for the area you're working in, you must follow it.**

---

## Testing Rules

### Requirements
- **Every new function/method** gets at least one happy-path and one error-path test
- **Every bug fix** gets a regression test that would have caught the bug
- **No test** should depend on another test's execution or state

### Test Naming
**Format:** `FeatureName > should {behavior} when {condition}`

### Coverage
- Minimum **90% line coverage** on new/modified files
- CI will block merges that drop below threshold

---

## Validation Commands

Run these before every commit:

```bash
# Lint
npm run lint

# Type check
npx tsc --noEmit

# Unit tests
npm run test:unit

# Full validation
npm run validate
```

**If any command fails, fix it before proceeding.**

---

## Architectural Constraints

### Layer Boundaries
```
Controller → Service → Repository
```
- Controllers handle HTTP, delegate to services
- Services contain business logic, no HTTP types
- Repositories handle data access

### Dependency Direction
- Dependencies flow inward (controllers depend on services, not vice versa)
- No circular imports
- Shared code goes in `/shared`

### Error Handling
- Use custom `AppError` subclasses, not raw `Error`
- Never swallow errors (empty `catch {}`)
- Log errors with context before re-throwing

---

## Performance Guardrails

- No N+1 queries (database calls inside loops)
- Use `.lean()` for read-only MongoDB queries
- Pagination required on all list endpoints
- Redis cache for hot paths and expensive computations

---

## Files Not to Modify Automatically

These files require manual review before changes:

- `package.json` — dependency changes need security review
- `*.config.js` — configuration changes affect all environments
- `migrations/` — database migrations are irreversible
- `.env.example` — environment changes affect deployment
- `Dockerfile` — container changes affect deployment

---

## Commit & PR Rules

### Commit Format
```
<type>(<scope>): <subject>
```
Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`

### PR Requirements
- [ ] All validation commands pass
- [ ] Tests written/updated for changes
- [ ] Relevant skill files updated if patterns changed
- [ ] PR description explains what and why
- [ ] No `console.log` in production code
- [ ] No `any` types introduced

---

## Skill Maintenance

### When to Update Skills
- New pattern or convention → update relevant skill
- New module → create module skill
- New gotcha discovered → add to Pitfalls section
- Architecture change → update affected skills

### When to Create New Skill
- Module has 3+ developers
- Domain-specific patterns not covered elsewhere
- Repeatedly explaining the same thing
