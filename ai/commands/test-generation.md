# Test Generation Command

## Goal
Generate comprehensive tests for a given function, class, or module.

## Inputs
- Target file path
- Function/class name (optional, generates for all if not specified)
- Feature name for test prefixes

## Steps

1. **Read Target Code**
   - Parse the file to understand structure
   - Identify public methods/functions
   - Map dependencies and imports

2. **Analyze Patterns**
   - Read `/ai/skills/testing-skill.md` for test patterns
   - Check existing tests in `__tests__/` for conventions
   - Identify mock requirements

3. **Generate Test Cases**
   For each function/method:
   - Happy path (valid input → expected output)
   - Error path (invalid input → expected error)
   - Edge cases (null, empty, boundary values)
   - Async behavior (if applicable)

4. **Create Test File**
   - Follow naming convention: `{name}.test.ts`
   - Use AAA pattern (Arrange, Act, Assert)
   - Include proper mocks and fixtures

## Constraints
- Follow test naming: `FeatureName > should {behavior} when {condition}`
- Mock at service/repository level, not database level
- No test interdependencies
- Each test should be independently runnable

## Output Schema

```typescript
import { describe, it, expect, beforeEach, jest } from '@jest/globals';
import { TargetClass } from './target';

describe('FeatureName > TargetClass', () => {
  let target: TargetClass;
  let mockDependency: jest.Mocked<Dependency>;

  beforeEach(() => {
    mockDependency = {
      method: jest.fn(),
    };
    target = new TargetClass(mockDependency);
  });

  describe('methodName', () => {
    it('FeatureName > should return result when valid input', async () => {
      // Arrange
      const input = { /* test data */ };
      mockDependency.method.mockResolvedValue(expected);

      // Act
      const result = await target.methodName(input);

      // Assert
      expect(result).toEqual(expected);
    });

    it('FeatureName > should throw error when invalid input', async () => {
      // Arrange
      const invalidInput = { /* invalid data */ };

      // Act & Assert
      await expect(target.methodName(invalidInput))
        .rejects.toThrow(ExpectedError);
    });
  });
});
```
