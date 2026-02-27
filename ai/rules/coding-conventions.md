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

**Philosophy:** Code should be self-documenting. Only add comments when code cannot express intent.

### Do Comment
- Non-obvious business logic
- Workarounds (with issue link)
- Complex algorithms
- Public API (JSDoc)

### Don't Comment
- Obvious code (`// increment counter`, `// return result`)
- What the code does (use clear naming instead)
- Commented-out code
- TODO without context

---

## Code Reuse

**DRY Principle:** Don't Repeat Yourself

```typescript
// ✅ Extract common logic
function validateEmail(email: string): boolean {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

// ❌ Duplicate validation logic
if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(user.email)) {}
if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(admin.email)) {}
```

**When to extract:**
- Logic used in 2+ places
- Complex conditions (>3 operators)
- Business rules that may change

---

## Documentation Policy

**When to create markdown files:**
- User explicitly requests documentation
- New architectural pattern introduced
- Complex domain logic needs explanation
- Setup/deployment procedures

**When NOT to create markdown:**
- Simple bug fixes
- Code refactoring
- Adding tests
- Minor feature changes

**Ask first** if documentation need is unclear.
