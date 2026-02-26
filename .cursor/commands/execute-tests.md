# Test Execution Command

> Trigger: `/execute-tests`
> Runs the appropriate test suite and reports results.

---

## Instructions

When the user runs `/execute-tests`, determine the scope and execute tests:

### Step 1: Determine Scope

Based on context, decide what to run:

| Context | Action |
|---|---|
| User has a specific file open | Run tests for that file/module |
| User mentions a module name | Run tests for that module |
| User says "all" or no specific context | Run the full unit test suite |
| User mentions "integration" | Run integration tests |
| User mentions "featurename" | Run feature tests |

### Step 2: Execute

```bash
# Single file (infer test path from source file)
# Source: src/modules/orders/order.service.ts
# Test:   src/modules/orders/__tests__/order.service.test.ts
npm run test -- --testPathPattern="order.service.test" --verbose

# Module-level
npm run test -- --testPathPattern="modules/orders" --verbose

# Full unit suite
npm run test:unit --verbose

# Integration tests
npm run test:integration --verbose

# Feature tests
npm run test: "FeatureName >"

# With coverage for the changed module
npm run test -- --testPathPattern="modules/orders" --coverage --collectCoverageFrom="src/modules/orders/**/*.ts"
```

### Step 3: Analyze Results

After execution, report:

1. **Pass/Fail summary** — total tests, passed, failed, skipped.
2. **Failed tests** — show the test name, expected vs received, and the relevant source line.
3. **Coverage** — if coverage was run, note any files below 90% threshold.
4. **Suggestions** — if tests fail, suggest specific fixes based on the error messages.

### Step 4: Missing Tests Check

If the user has made code changes, check if corresponding tests exist:
- New functions/methods → should have test coverage
- Modified logic → existing tests should be updated
- Bug fixes → should have a regression test

Report any gaps:
```
## Missing Test Coverage
- `src/modules/orders/order.service.ts:calculateTotal()` — no test found
- `src/modules/orders/order.service.ts:applyDiscount()` — test exists but doesn't cover the new error path
```
