# {Repository Name} — Repo-Wide Skill

<!-- INSTRUCTIONS: Replace all {placeholders} with your repo's actual values. -->
<!-- Delete sections that don't apply. Add sections specific to your repo. -->

---

## 1. Overview

{2-4 sentences: what this repo does, who it serves, where it fits in the product.}

---

## 2. Tech Stack

| Layer | Technology |
|---|---|
| Language | Ex: TypeScript 5.x (strict mode) |
| Runtime | Ex: Node.js 20 LTS |
| Framework | Ex: {NestJS / Express / Fastify} |
| Database | Ex: MongoDB 7 via Mongoose 8.x |
| Cache | Ex: Redis 7 via ioredis |
| Queue | Ex: {RabbitMQ / SQS / SNS, KAFKA} |
| Testing | Ex: Jest |
| Logging | Ex: {pino / winston} |
| Linting | Ex: ESLint + Prettier |
| Build | Ex: {tsup / tsc / esbuild} |

<!-- For NestJS repos, add: -->
<!-- | DI Container | NestJS built-in (constructor injection) | -->
<!-- | ORM | Mongoose or @contentstack/mongodb | -->

---

## 3. Project Structure

```
src/
├── config/              # Env parsing, app config (zod schemas)
├── modules/             # Feature modules (one per domain)
│   ├── {module-a}/
│   │   ├── {module-a}.controller.ts
│   │   ├── {module-a}.service.ts
│   │   ├── {module-a}.repository.ts
│   │   ├── {module-a}.model.ts          # Mongoose schema
│   │   ├── {module-a}.types.ts          # Interfaces and types
│   │   ├── {module-a}.validator.ts      # Zod request/response schemas
│   │   ├── {module-a}.module.ts         # NestJS module (if using NestJS)
│   │   └── __tests__/
│   └── {module-b}/
├── shared/              # Cross-cutting: middleware, guards, utils, base classes
│   ├── errors/          # Custom error classes
│   ├── middleware/       # Auth, logging, rate-limit middleware
│   ├── guards/          # NestJS guards (if using NestJS)
│   ├── decorators/      # Custom decorators
│   └── utils/           # Pure utility functions
├── infra/               # Infrastructure: DB connections, Redis client, event bus
├── jobs/                # Background job processors (if applicable)
└── main.ts              # Entry point
```

<!-- Customize the tree above to match your actual structure. -->
<!-- Keep it to 2-3 levels deep. Annotate every directory. -->

---
## 4. How to Run the Repository

### Installation

```bash
$ npm install
```

### Running the app

```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode (run built app)
$ npm run start:prod
```

### Build

```bash
$ npm run build
```

### Test

```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```

### Lint and format

```bash
$ npm run lint
$ npm run format
```

### Git Hooks

```bash
# add pre-commit and pre-push githooks (Linux and MacOS)
$ npm run install-githooks
```



## 5. Related Skills

<!-- List other skill files in this repo and related repos -->

| Skill | Location | Purpose |
|---|---|---|
| Code Review | `skills/code-review-skill/skill.md` | PR review automation |
| {Module A} | `skills/{module-a}-skill/skill.md` | {Module A} domain patterns |

