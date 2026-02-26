# {Repository Name}

> {2-4 sentences: what this repo does, who it serves, where it fits in the product.}

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Language | TypeScript 5.x (strict mode) |
| Runtime | Node.js 20 LTS |
| Framework | {NestJS / Express / Fastify} |
| Database | {MongoDB / PostgreSQL} |
| Cache | {Redis} |
| Testing | Jest |
| Linting | ESLint + Prettier |

---

## Project Structure

```
src/
├── config/              # Environment parsing, app config
├── modules/             # Feature modules (one per domain)
│   ├── {module-a}/
│   │   ├── {module-a}.controller.ts
│   │   ├── {module-a}.service.ts
│   │   ├── {module-a}.repository.ts
│   │   └── __tests__/
│   └── {module-b}/
├── shared/              # Cross-cutting: middleware, utils, errors
└── main.ts              # Entry point

ai/                      # AI development layer
├── context/             # Architecture, domain, glossary
├── commands/            # Structured workflows
├── skills/              # Domain knowledge (flat files)
├── rules/               # Development rules
└── index.json           # Machine-readable manifest
```

---

## Getting Started

### Prerequisites
- Node.js 20+
- {Database requirements}
- {Other dependencies}

### Installation

```bash
npm install
```

### Running the App

```bash
# Development (watch mode)
npm run start:dev

# Production
npm run start:prod
```

### Build

```bash
npm run build
```

---

## Testing

```bash
# Unit tests
npm run test

# Integration tests
npm run test:e2e

# Coverage report
npm run test:cov
```

---

## Validation

Run before every commit:

```bash
npm run lint          # Lint check
npx tsc --noEmit      # Type check
npm run test:unit     # Unit tests
npm run validate      # Full validation
```

---

## AI Development Layer

This repository includes an `/ai` directory for AI-assisted development:

| Resource | Path | Purpose |
|----------|------|---------|
| Context | `/ai/context/` | Architecture, domain, glossary |
| Rules | `/ai/rules/` | Development constraints |
| Commands | `/ai/commands/` | Structured workflows |
| Skills | `/ai/skills/` | Domain knowledge |
| Manifest | `/ai/index.json` | Machine-readable index |

### Quick Skill Reference

| Skill | When to Use |
|-------|-------------|
| [Testing](/ai/skills/testing-skill.md) | Writing tests |
| [Code Style](/ai/skills/code-style-skill.md) | Writing code |
| [Commit Format](/ai/skills/commit-format-skill.md) | Making commits |
| [PR Review](/ai/skills/pr-review-skill.md) | Reviewing PRs |
| [Security](/ai/skills/security-skill.md) | Handling auth/secrets |
| [Creating Skills](/ai/skills/creating-skills-skill.md) | Adding documentation |

---

## Contributing

1. Read the relevant skill files before making changes
2. Follow the code style guidelines
3. Write tests for all changes
4. Run validation before committing
5. Use conventional commit format

See [AGENTS.md](AGENTS.md) for AI agent guidelines.
