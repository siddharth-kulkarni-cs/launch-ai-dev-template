# Development Guidelines — {Repository Name}

> Quick reference for AI agents. See `/ai/skills/` for detailed guides.

## Standards

**Testing:** Every change needs tests (happy path + error path). 90% coverage minimum.
→ [Testing Skill](/ai/skills/testing-skill.md)

**Code Style:** `kebab-case.ts` files, `PascalCase` classes, named exports only.
→ [Code Style Skill](/ai/skills/code-style-skill.md)

**Commits:** `<type>(<scope>): <subject>` — conventional commits format.
→ [Commit Format Skill](/ai/skills/commit-format-skill.md)

**Security:** No hardcoded secrets, validate all input, use custom errors.
→ [Security Skill](/ai/skills/security-skill.md)

**Reviews:** Follow checklist before submitting PR.
→ [PR Review Skill](/ai/skills/pr-review-skill.md)

## Architecture

{One-line description of the system architecture}

**Layers:** Controller → Service → Repository

**Key flows:**
- {Add application-specific flows here}

## Validation Commands

```bash
npm run lint          # Lint check
npx tsc --noEmit      # Type check
npm run test:unit     # Unit tests
npm run validate      # Full validation
```

## Finding Skills

```bash
ls ai/skills/
grep -r "description:.*keyword" ai/skills/*-skill.md
cat ai/skills/{name}-skill.md
```

**Categories:**
- **Dev:** testing, code-style, commit-format, pr-review, security, creating-skills
- **Setup:** {local-setup, docker-dev — add if created}
- **Logic:** {request-flow, auth-flow — add if created}

## Critical Files

Do not modify without review:
- `package.json` — dependency changes
- `migrations/` — database migrations
- `.env.example` — environment config
- `Dockerfile` — container config

## Quick Links

| Resource | Path |
|----------|------|
| Context | `/ai/context/` |
| Rules | `/ai/rules/` |
| Commands | `/ai/commands/` |
| Skills | `/ai/skills/` |
| Manifest | `/ai/index.json` |
