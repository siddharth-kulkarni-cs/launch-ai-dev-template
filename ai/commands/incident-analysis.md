# Incident Analysis Command

## Goal
Analyze production incidents, identify root cause, and propose fixes.

## Inputs
- Error message or stack trace
- Logs (if available)
- Time of incident
- Affected endpoint/service

## Steps

1. **Parse Error Information**
   - Extract error type and message
   - Identify file and line number from stack trace
   - Note any correlation IDs or request IDs

2. **Locate Source Code**
   - Find the file mentioned in stack trace
   - Read surrounding context
   - Identify the failing operation

3. **Trace Data Flow**
   - Follow the request path to the error
   - Identify what data was being processed
   - Check for validation gaps

4. **Identify Root Cause**
   - Determine why the error occurred
   - Check for similar patterns in codebase
   - Review recent changes to affected code

5. **Propose Fix**
   - Write code fix with tests
   - Consider edge cases
   - Check for similar issues elsewhere

6. **Recommend Prevention**
   - Suggest monitoring/alerting improvements
   - Identify missing validation
   - Propose skill updates if pattern issue

## Constraints
- Don't make assumptions without evidence
- Check logs before concluding
- Consider all possible causes
- Propose defensive fixes

## Output Schema

```markdown
## Incident Analysis

### Summary
{One sentence description of the incident}

### Timeline
- {Time}: {Event}
- {Time}: {Event}

### Root Cause
{Detailed explanation of why this happened}

### Affected Code
**File:** `{path}`
```{language}
// Problematic code
```

### Fix
**File:** `{path}`
```{language}
// Fixed code
```

### Test Case
```{language}
// Regression test
```

### Prevention
1. {Recommendation 1}
2. {Recommendation 2}

### Related Issues
- {Links to similar issues if any}
```
