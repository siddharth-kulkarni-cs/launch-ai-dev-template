# PR Summary Command

## Goal
Generate a comprehensive PR summary from staged/committed changes.

## Inputs
- Git diff (staged or between commits)
- PR title (optional)
- JIRA ticket (optional)

## Steps

1. **Gather Changes**
   ```bash
   git diff --staged
   # or
   git diff main...HEAD
   ```

2. **Analyze Changes**
   - Identify files modified, added, deleted
   - Categorize by type (feature, fix, refactor, test)
   - Extract key changes per file

3. **Generate Summary**
   - Write concise description of what changed
   - Explain why (if commit messages provide context)
   - List affected modules/areas

4. **Create Checklist**
   - Testing requirements
   - Documentation updates needed
   - Migration steps (if applicable)

## Constraints
- Summary should be under 500 words
- Focus on "what" and "why", not "how"
- Don't include implementation details
- Reference skill files if patterns were followed

## Output Schema

```markdown
## Summary
{2-3 sentences describing the change}

## Changes
- **{Module}:** {What changed}
- **{Module}:** {What changed}

## Type
{feat|fix|refactor|test|docs|chore}

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests (if needed)
- [ ] Manual testing steps

## Checklist
- [ ] Code follows style guide
- [ ] No console.log statements
- [ ] No any types
- [ ] Documentation updated
```
