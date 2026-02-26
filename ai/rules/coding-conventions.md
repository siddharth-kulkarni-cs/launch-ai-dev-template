# Coding Conventions

> Naming, formatting, and structural conventions for this repository.

---

## Naming Conventions

### Files
| Type | Convention | Example |
|------|------------|---------|
| Source files | `kebab-case.ts` | `order-service.ts` |
| Test files | `{name}.test.ts` | `order-service.test.ts` |
| Type files | `{name}.types.ts` | `order.types.ts` |

### Code Elements
| Element | Convention | Example |
|---------|------------|---------|
| Classes | `PascalCase` | `OrderService` |
| Interfaces | `PascalCase` | `OrderRepository` |
| Functions | `camelCase` | `calculateTotal()` |
| Constants | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| Env vars | `UPPER_SNAKE_CASE` | `DATABASE_URL` |

### Database
| Element | Convention | Example |
|---------|------------|---------|
| Collections | `plural_snake_case` | `order_items` |
| Redis keys | `{service}:{entity}:{id}` | `orders:cache:abc123` |

---

## Export Rules

```typescript
// ✅ Named exports only
export class OrderService {}
export function calculateTotal() {}

// ❌ No default exports
export default class OrderService {}
```

---

## Type Safety

```typescript
// ✅ Use unknown and narrow
function process(data: unknown): Order {
  if (!isOrder(data)) throw new ValidationError();
  return data;
}

// ❌ No any types
function process(data: any): Order {}
```

---

## Import Order

```typescript
// 1. Node.js built-ins
import { readFile } from 'fs/promises';

// 2. External packages
import { Injectable } from '@nestjs/common';

// 3. Internal modules
import { AppError } from '@/shared/errors';

// 4. Relative imports
import { OrderRepository } from './order.repository';
```

---

## Comments Policy

### Do Comment
- Non-obvious business logic
- Workarounds (with issue link)
- Complex algorithms
- Public API (JSDoc)

### Don't Comment
- Obvious code
- Commented-out code
- TODO without context
