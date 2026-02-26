# Refactor Module Command

## Goal
Safely refactor a module while maintaining functionality and test coverage.

## Inputs
- Module path or name
- Refactoring type (extract, rename, restructure, optimize)
- Specific goals (optional)

## Steps

1. **Analyze Current State**
   - Read all files in the module
   - Map dependencies (what depends on this module)
   - Map dependents (what this module depends on)
   - Identify public API surface

2. **Check Test Coverage**
   ```bash
   npm run test:coverage -- --collectCoverageFrom="src/modules/{module}/**"
   ```
   - Ensure adequate coverage before refactoring
   - Note any gaps to address

3. **Plan Refactoring**
   - List specific changes to make
   - Identify breaking changes to public API
   - Plan migration path for dependents

4. **Execute Refactoring**
   - Make changes incrementally
   - Run tests after each change
   - Update imports in dependent files

5. **Validate**
   ```bash
   npm run lint
   npx tsc --noEmit
   npm run test:unit
   ```

6. **Update Documentation**
   - Update module skill file if exists
   - Update related skills if patterns changed

## Constraints
- Never break public API without migration plan
- Maintain or improve test coverage
- Follow existing patterns in `/ai/skills/code-style-skill.md`
- Make atomic commits for each logical change

## Output Schema

```markdown
## Refactoring Summary

### Changes Made
1. {Change 1 with file paths}
2. {Change 2 with file paths}

### Files Modified
- `src/modules/{module}/{file}.ts` - {what changed}

### Breaking Changes
- {List any breaking changes, or "None"}

### Migration Required
- {Steps for dependents to migrate, or "None"}

### Test Results
- Coverage before: {X}%
- Coverage after: {Y}%
- All tests passing: {Yes/No}

### Follow-up Tasks
- [ ] {Any remaining work}
```
