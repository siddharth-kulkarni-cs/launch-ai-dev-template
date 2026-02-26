---
name: code-style
description: Naming conventions, file organization, and coding standards. Use when writing new code, refactoring, or reviewing code style.
---

# Code Style Skill

## When to Use
- Writing new code (functions, classes, files)
- Refactoring existing code
- Reviewing code for style consistency
- Naming new entities (variables, functions, files)
- Organizing imports and file structure

## Overview

This repository follows consistent naming and organization patterns to ensure readability and maintainability.

## Naming Conventions

### Files
| Type | Convention | Example |
|------|------------|---------|
| Source files | `kebab-case.ts` | `order-service.ts` |
| Test files | `{name}.test.ts` | `order-service.test.ts` |
| Type files | `{name}.types.ts` | `order.types.ts` |
| Constants | `{name}.constants.ts` | `order.constants.ts` |

### Code Elements
| Element | Convention | Example |
|---------|------------|---------|
| Classes | `PascalCase` | `OrderService` |
| Interfaces | `PascalCase` (no `I` prefix) | `OrderRepository` |
| Types | `PascalCase` | `CreateOrderInput` |
| Functions | `camelCase` | `calculateTotal()` |
| Methods | `camelCase` | `getOrderById()` |
| Variables | `camelCase` | `orderCount` |
| Constants | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| Env vars | `UPPER_SNAKE_CASE` | `DATABASE_URL` |
| Private fields | `camelCase` (no underscore) | `orderCache` |

### Database
| Element | Convention | Example |
|---------|------------|---------|
| Collections | `plural_snake_case` | `order_items` |
| Fields | `camelCase` | `createdAt` |
| Indexes | `idx_{collection}_{fields}` | `idx_orders_userId` |

### Redis Keys
```
{service}:{entity}:{identifier}
```
Examples: `orders:cache:abc123`, `users:session:xyz789`

## File Organization

### Import Order
```typescript
// 1. Node.js built-ins
import { readFile } from 'fs/promises';
import path from 'path';

// 2. External packages
import { Injectable } from '@nestjs/common';
import { z } from 'zod';

// 3. Internal modules (absolute paths)
import { AppError } from '@/shared/errors';
import { Logger } from '@/shared/logger';

// 4. Relative imports
import { OrderRepository } from './order.repository';
import { Order } from './order.types';
```

### File Structure
```typescript
// 1. Imports (grouped as above)

// 2. Constants
const MAX_ITEMS = 100;

// 3. Types/Interfaces
interface OrderInput {
  items: Item[];
}

// 4. Main export (class or function)
export class OrderService {
  // ...
}

// 5. Helper functions (if needed)
function validateOrder(order: Order): boolean {
  // ...
}
```

## Code Patterns

### Exports
```typescript
// ✅ Named exports only
export class OrderService {}
export function calculateTotal() {}
export const ORDER_STATUS = {};

// ❌ No default exports
export default class OrderService {} // Don't do this
```

### Type Safety
```typescript
// ✅ Use unknown and narrow
function processData(data: unknown): Order {
  if (!isOrder(data)) {
    throw new ValidationError('Invalid order data');
  }
  return data;
}

// ❌ Don't use any
function processData(data: any): Order { // Avoid!
  return data;
}
```

### Error Handling
```typescript
// ✅ Use custom error classes
throw new OrderNotFoundError(orderId);

// ❌ Don't use generic Error
throw new Error('Order not found'); // Avoid!
```

### Logging
```typescript
// ✅ Use project logger
import { logger } from '@/shared/logger';
logger.info('Order created', { orderId });

// ❌ Don't use console
console.log('Order created'); // Avoid!
```

## Comments Policy

### When to Comment
- Non-obvious business logic
- Workarounds with context (link to issue)
- Complex algorithms
- Public API documentation (JSDoc)

### When NOT to Comment
```typescript
// ❌ Obvious comments
const total = price * quantity; // Calculate total

// ❌ Commented-out code
// const oldImplementation = ...

// ❌ TODO without context
// TODO: fix this
```

### Good Comment Examples
```typescript
// Retry with exponential backoff per RFC 7231 section 7.1.3
// Max 5 retries with 2^n * 100ms delay

// HACK: MongoDB 6.0 has a bug with $lookup on sharded collections
// Remove after upgrading to 6.1 (see JIRA-1234)
```

## Common Pitfalls

❌ **Problem:** Inconsistent naming
```typescript
// Bad: Mixed conventions
const user_name = 'John';  // Should be camelCase
class order_service {}     // Should be PascalCase
```
✅ **Solution:** Follow the naming table above consistently

❌ **Problem:** Barrel files with circular imports
```typescript
// Bad: index.ts re-exports everything
export * from './order.service';
export * from './order.repository'; // May cause circular deps
```
✅ **Solution:** Import directly from specific files when needed

❌ **Problem:** Magic numbers/strings
```typescript
// Bad
if (retries > 5) { ... }
if (status === 'pending') { ... }
```
✅ **Solution:** Use named constants
```typescript
const MAX_RETRIES = 5;
const OrderStatus = { PENDING: 'pending' } as const;
```

❌ **Problem:** Long functions (>50 lines)
✅ **Solution:** Extract into smaller, focused functions

## Related Skills
- [Testing](/ai/skills/testing-skill.md) - Test file naming
- [Commit Format](/ai/skills/commit-format-skill.md) - Commit message style
- [PR Review](/ai/skills/pr-review-skill.md) - Style review checklist

## Troubleshooting

**Issue:** ESLint errors on naming
**Fix:** Check `.eslintrc` for project-specific overrides

**Issue:** Import path errors
**Fix:** Verify `tsconfig.json` paths configuration

**Issue:** Type errors with `unknown`
**Fix:** Use type guards or assertion functions to narrow types
