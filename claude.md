# Claude Code Guidelines

> Quick reference for Claude Code. See `/ai/` for detailed documentation.

## AI Layer

This repository uses the `/ai` directory for AI-assisted development:

- **Context:** `/ai/context/` — architecture, domain, glossary
- **Rules:** `/ai/rules/` — development constraints
- **Commands:** `/ai/commands/` — structured workflows
- **Skills:** `/ai/skills/` — domain knowledge

## Quick Start

1. Read `/ai/context/architecture.md` for system overview
2. Check `/ai/skills/` for relevant patterns
3. Follow `/ai/rules/dev-workflow.md` for all changes

## Standards

- Tests: happy path + error path, 90% coverage. By default, any code change should be done with TDD.
- Style: `kebab-case.ts`, `PascalCase` classes, named exports
- Commits: `<type>(<scope>): <subject>`
- Security: no secrets, validate input

## Validation

```bash
npm run validate
```

See [AGENTS.md](AGENTS.md) for complete guidelines.
