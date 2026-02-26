---
name: security
description: Security patterns, secret management, and vulnerability prevention. Use when handling auth, secrets, user input, or reviewing security.
---

# Security Skill

## When to Use
- Handling authentication/authorization
- Managing secrets and credentials
- Validating user input
- Reviewing code for security issues
- Setting up environment variables
- Logging sensitive operations

## Overview

Security is a critical concern. This skill covers patterns for secure development in this repository.

## Never Commit Secrets

### Types of Secrets to Protect
- API keys and tokens
- Database credentials
- JWT secrets
- OAuth client secrets
- Encryption keys
- Service account credentials
- Webhook secrets

### Detection
```bash
# Check for potential secrets before commit
git diff --cached | grep -iE "(password|secret|key|token|credential)"
```

### If Secrets Are Committed
1. **Immediately** rotate the compromised credential
2. Remove from git history using `git filter-branch` or BFG
3. Report the incident per security policy

## Environment Variables

### Loading Pattern
```typescript
// ✅ Use validated config
import { config } from '@/config';
const dbUrl = config.database.url;

// ❌ Don't access process.env directly
const dbUrl = process.env.DATABASE_URL; // No validation!
```

### Config Validation
```typescript
import { z } from 'zod';

const envSchema = z.object({
  DATABASE_URL: z.string().url(),
  JWT_SECRET: z.string().min(32),
  API_KEY: z.string().min(16),
  NODE_ENV: z.enum(['development', 'production', 'test']),
});

export const config = envSchema.parse(process.env);
```

### .env.example
Always maintain an `.env.example` with placeholder values:
```bash
DATABASE_URL=mongodb://localhost:27017/myapp
JWT_SECRET=your-secret-key-min-32-chars-here
API_KEY=your-api-key-here
NODE_ENV=development
```

## Input Validation

### Always Validate External Data
```typescript
import { z } from 'zod';

// Define schema
const CreateOrderSchema = z.object({
  items: z.array(z.object({
    productId: z.string().uuid(),
    quantity: z.number().int().positive().max(100),
  })).min(1).max(50),
  shippingAddress: z.string().min(10).max(500),
});

// Validate in controller
app.post('/orders', (req, res) => {
  const validatedData = CreateOrderSchema.parse(req.body);
  // Now safe to use
});
```

### Validation Points
| Source | Validation Required |
|--------|---------------------|
| Request body | Always |
| Query params | Always |
| Path params | Always |
| Headers | When used for logic |
| Cookies | Always |
| File uploads | Always (type, size, content) |

### NoSQL Injection Prevention
```typescript
// ❌ Vulnerable to injection
const user = await User.findOne({ email: req.body.email });

// ✅ Validate first
const email = z.string().email().parse(req.body.email);
const user = await User.findOne({ email });

// ✅ Or use parameterized queries
const user = await User.findOne({ email: { $eq: validatedEmail } });
```

## Auth Patterns

### Authentication Check
```typescript
// Middleware pattern
async function requireAuth(req, res, next) {
  const token = req.headers.authorization?.replace('Bearer ', '');
  
  if (!token) {
    throw new UnauthorizedError('Missing token');
  }

  try {
    const payload = await verifyToken(token);
    req.user = payload;
    next();
  } catch (error) {
    throw new UnauthorizedError('Invalid token');
  }
}
```

### Authorization Check
```typescript
// Check permissions after authentication
async function requirePermission(permission: string) {
  return async (req, res, next) => {
    if (!req.user) {
      throw new UnauthorizedError('Not authenticated');
    }
    
    if (!req.user.permissions.includes(permission)) {
      throw new ForbiddenError('Insufficient permissions');
    }
    
    next();
  };
}

// Usage
app.delete('/orders/:id', 
  requireAuth, 
  requirePermission('orders:delete'),
  deleteOrderHandler
);
```

### Session Security
```typescript
// Regenerate session on login
async function login(req, res) {
  // ... validate credentials
  
  // Regenerate session ID to prevent fixation
  req.session.regenerate((err) => {
    req.session.userId = user.id;
    res.json({ success: true });
  });
}
```

## Logging Security

### What NOT to Log
```typescript
// ❌ Never log these
logger.info('User login', { password: user.password });
logger.info('Payment', { cardNumber: payment.card });
logger.info('Request', { headers: req.headers }); // May contain tokens

// ✅ Log safely
logger.info('User login', { userId: user.id, email: user.email });
logger.info('Payment', { paymentId: payment.id, amount: payment.amount });
logger.info('Request', { path: req.path, method: req.method });
```

### Sensitive Data Patterns
| Never Log | Safe to Log |
|-----------|-------------|
| Passwords | User IDs |
| API keys | Request paths |
| Tokens | Timestamps |
| Card numbers | Transaction IDs |
| SSN/PII | Anonymized data |
| Session data | Event types |

## Common Pitfalls

❌ **Problem:** Hardcoded secrets
```typescript
const API_KEY = 'sk_live_abc123'; // In code!
```
✅ **Solution:** Use environment variables

❌ **Problem:** Missing input validation
```typescript
const userId = req.params.id; // Could be anything!
await User.findById(userId);
```
✅ **Solution:** Validate all external input

❌ **Problem:** Exposing stack traces
```typescript
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.stack }); // Leaks internals!
});
```
✅ **Solution:** Return generic errors in production

❌ **Problem:** Using `eval()` or `Function()`
```typescript
eval(userInput); // Remote code execution!
```
✅ **Solution:** Never execute user-provided code

❌ **Problem:** Missing rate limiting
✅ **Solution:** Apply rate limits to auth endpoints

## Related Skills
- [PR Review](/ai/skills/pr-review-skill.md) - Security review checklist
- [Code Style](/ai/skills/code-style-skill.md) - Error handling patterns
- [Testing](/ai/skills/testing-skill.md) - Security test patterns

## Troubleshooting

**Issue:** Secret accidentally committed
**Fix:** Rotate immediately, use BFG to clean history

**Issue:** Validation errors in production
**Fix:** Check schema matches expected input format

**Issue:** Auth middleware not triggering
**Fix:** Verify middleware order in route definition
