---
name: testing
description: Test patterns, frameworks, and coverage requirements. Use when writing tests, debugging test failures, or reviewing test coverage.
---

# Testing Skill

## When to Use
- Writing new unit or integration tests
- Debugging failing tests
- Reviewing test coverage
- Setting up test fixtures or mocks
- Understanding test patterns in this repo

## Overview

This repository uses {Jest/Mocha/Vitest} for testing with {describe the testing philosophy}.

## File Naming

| Type | Pattern | Example |
|------|---------|---------|
| Unit tests | `{name}.test.ts` | `order.service.test.ts` |
| Integration tests | `{name}.integration.test.ts` | `order.integration.test.ts` |
| E2E tests | `{name}.e2e.test.ts` | `order.e2e.test.ts` |
| Test fixtures | `fixtures/{name}.fixture.ts` | `fixtures/order.fixture.ts` |

## Test Pattern

```typescript
describe('FeatureName > ComponentName', () => {
  // Setup
  let service: OrderService;
  let mockRepository: jest.Mocked<OrderRepository>;

  beforeEach(() => {
    mockRepository = createMockRepository();
    service = new OrderService(mockRepository);
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  describe('methodName', () => {
    it('FeatureName > should return expected result when valid input', async () => {
      // Arrange
      const input = createTestOrder();
      mockRepository.findById.mockResolvedValue(input);

      // Act
      const result = await service.getOrder(input.id);

      // Assert
      expect(result).toEqual(input);
      expect(mockRepository.findById).toHaveBeenCalledWith(input.id);
    });

    it('FeatureName > should throw error when order not found', async () => {
      // Arrange
      mockRepository.findById.mockResolvedValue(null);

      // Act & Assert
      await expect(service.getOrder('invalid-id'))
        .rejects.toThrow(OrderNotFoundError);
    });
  });
});
```

## Writing Tests

### Test Naming Convention
**Format:** `FeatureName > should {behavior} when {condition}`

- Get feature name from JIRA ticket or ask user
- Describe expected behavior, not implementation
- Include the condition that triggers the behavior

### Test Structure (AAA Pattern)
1. **Arrange:** Set up test data and mocks
2. **Act:** Execute the code under test
3. **Assert:** Verify the expected outcome

### Coverage Requirements
- **Minimum:** 90% line coverage on new/modified files
- **Required paths:** Happy path + at least one error path
- **Bug fixes:** Must include regression test

## Mocking

### Mock at the Right Level
```typescript
// ✅ Mock the repository (correct level)
const mockRepo = {
  findById: jest.fn(),
  save: jest.fn(),
};

// ❌ Don't mock the database driver directly
// This couples tests to implementation details
```

### Mock Factories
```typescript
// Create reusable mock factories
function createMockOrder(overrides?: Partial<Order>): Order {
  return {
    id: 'test-order-id',
    status: 'pending',
    items: [],
    createdAt: new Date(),
    ...overrides,
  };
}
```

## Running Tests

```bash
# Unit tests (fast, no external deps)
npm run test:unit

# Integration tests (requires Docker)
npm run test:integration

# Single file
npm run test -- --testPathPattern=order.service

# With coverage
npm run test:coverage

# Watch mode
npm run test:watch

# Filter by feature name
npm run test -- --testNamePattern="FeatureName"
```

## Common Pitfalls

❌ **Problem:** Tests depend on execution order
```typescript
// Bad: Shared mutable state
let counter = 0;
it('test 1', () => { counter++; });
it('test 2', () => { expect(counter).toBe(1); }); // Fragile!
```
✅ **Solution:** Reset state in beforeEach, use isolated test data

❌ **Problem:** Testing implementation instead of behavior
```typescript
// Bad: Testing internal method calls
expect(service._internalMethod).toHaveBeenCalled();
```
✅ **Solution:** Test observable behavior and outputs

❌ **Problem:** Floating promises in tests
```typescript
// Bad: Promise not awaited
it('should work', () => {
  service.asyncMethod(); // No await!
  expect(something).toBe(true);
});
```
✅ **Solution:** Always await async operations

❌ **Problem:** No error path testing
✅ **Solution:** Every happy path needs a corresponding error path test

## Related Skills
- [Code Style](/ai/skills/code-style-skill.md) - Naming conventions
- [PR Review](/ai/skills/pr-review-skill.md) - Test review checklist

## Troubleshooting

**Issue:** Tests pass locally but fail in CI
**Fix:** Check for time-dependent tests, ensure mocks are properly reset

**Issue:** "Cannot find module" in tests
**Fix:** Verify tsconfig paths and jest moduleNameMapper match

**Issue:** Tests timeout
**Fix:** Check for unresolved promises, increase timeout for integration tests
