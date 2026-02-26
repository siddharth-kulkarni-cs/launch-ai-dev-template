# {Module Name} Skill

<!-- TEMPLATE: Copy this directory as Skills/{module-name}-skill/ -->
<!-- Replace all {placeholders} with actual values. -->
<!-- Delete sections that don't apply. -->

---

## 1. Purpose

{What this module does. 2-3 sentences.}

**Owner team:** {team name}
**Key consumers:** {other modules or services that depend on this}

---

## 2. Module Structure

```
src/modules/{module-name}/
├── {module-name}.controller.ts    # REST endpoints
├── {module-name}.service.ts       # Business logic
├── {module-name}.repository.ts    # Data access (Mongoose + Redis)
├── {module-name}.model.ts         # Mongoose schema(s)
├── {module-name}.types.ts         # TypeScript interfaces
├── {module-name}.validator.ts     # Zod request/response schemas
├── {module-name}.module.ts        # NestJS module definition (if NestJS)
├── {module-name}.constants.ts     # Module-specific constants
├── dto/                           # Data transfer objects (if complex)
├── events/                        # Event definitions & handlers
└── __tests__/
    ├── {module-name}.service.test.ts
    ├── {module-name}.controller.test.ts
    └── fixtures/                  # Test data factories
```

---

## 3. Data Model

### Primary Entity: `{EntityName}`

```typescript
interface {EntityName} {
  _id: ObjectId;
  // ... key fields with types and descriptions
  createdAt: Date;
  updatedAt: Date;
}
```

**Collection:** `{collection_name}`
**Key indexes:** `{ field1: 1, field2: 1 }`, `{ createdAt: -1 }`

### Redis Cache Keys

| Key Pattern | TTL | Purpose |
|---|---|---|
| `{service}:{entity}:{id}` | 5 min | Single entity cache |
| `{service}:{entity}:list:{hash}` | 2 min | List query cache |

---

## 4. API Endpoints

| Method | Path | Description | Auth |
|---|---|---|---|
| GET | `/api/{module}/{id}` | Get by ID | Required |
| GET | `/api/{module}` | List (paginated) | Required |
| POST | `/api/{module}` | Create | Required |
| PATCH | `/api/{module}/{id}` | Update | Required |
| DELETE | `/api/{module}/{id}` | Soft delete | Admin |

---

## 5. Business Rules

<!-- List the important domain rules that affect how code is written. -->
<!-- These are the things an AI MUST know to avoid generating wrong logic. -->

1. {Rule 1: e.g., "An order cannot be cancelled after it has been shipped."}
2. {Rule 2: e.g., "Discount codes can only be applied once per user per month."}
3. {Rule 3}

---

## 6. Common Tasks

### Adding a new field to {EntityName}

1. Add the field to the Mongoose schema in `{module-name}.model.ts`.
2. Add the field to the TypeScript interface in `{module-name}.types.ts`.
3. If the field is user-facing, add it to the zod schema in `{module-name}.validator.ts`.
4. If the field needs an index, add it in the schema file.
5. Update relevant tests in `__tests__/`.

### Adding a new business rule

1. Implement the logic in `{module-name}.service.ts`.
2. Add a corresponding `AppError` subclass if the rule can be violated.
3. Write unit tests covering: valid case, violation case, edge cases.
4. Document the rule in this skill file (Section 5).

---

## 7. Dependencies & Interactions

```
{module-name} service
  ├── depends on → {other-module} service (for ...)
  ├── depends on → Redis (for caching)
  ├── publishes → {event-name} event
  └── consumed by → {consumer-module}
```

---

## 8. Module-Specific Pitfalls

- {Pitfall 1: links to pitfalls in references}
- {Pitfall 2}

---

## 9. References

- `references/` — {list key reference files and what they contain}
- `docs/` — {list design docs, ADRs}
- `scripts/` — {list helper scripts and what they do}
