# Development Guidelines — {Repository Name}

> Quick reference for AI agents. See `/ai/skills/` for detailed guides.

## Workflow

**Before any code changes or investigation:**

1. **Load Context:** Read `/ai/context/architecture.md` for system understanding
2. **Check Skills:** Read relevant `/ai/skills/` files for the specific task
3. **Follow Rules:** Apply `/ai/rules/` constraints for all changes
4. **Then Proceed:** Start investigation or implementation

## Standards

**Testing: Every change needs tests (happy path + error path). 90% coverage minimum. By default, any code change should be done with TDD.
→ [Testing Skill](/ai/skills/testing-skill.md)
→ [TDD Skill**](/ai/skills/tdd-skill.md)

**Code Style:** `kebab-case.ts` files, `PascalCase` classes, named exports only. Code should be self-documenting; avoid unnecessary comments.
→ [Code Style Skill](/ai/skills/code-style-skill.md)

**Code Reuse:** Follow DRY principle. Extract common logic into reusable functions.

**Documentation:** Do NOT create markdown files unless explicitly requested. Ask first if unclear.

**Commits:** `<git-branch-name> | <subject>` — conventional commits format.
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

## Finding Skills

> **Note for Cursor Users:** Skills and Rules are natively auto-discovered via the `.cursor/` directory. You do not need to manually search for them.

For other AI agents, use the following commands to find relevant skills:

```bash
ls ai/skills/
grep -r "description:.*keyword" ai/skills/*-skill.md
cat ai/skills/{name}-skill.md
```

**Categories:**

- **Dev:** tdd, testing, code-style, commit-format, pr-review, security, creating-skills
- **Setup:** {local-setup, docker-dev — add if created}
- **Logic:** {request-flow, auth-flow — add if created}

## Critical Files

Do not modify without review:

- `package.json` — dependency changes
- `migrations/` — database migrations
- `.env.example` — environment config
- `Dockerfile` — container config

## Quick Links


| Resource | Path             |
| -------- | ---------------- |
| Context  | `/ai/context/`   |
| Rules    | `/ai/rules/`     |
| Commands | `/ai/commands/`  |
| Skills   | `/ai/skills/`    |
| Manifest | `/ai/index.json` |


