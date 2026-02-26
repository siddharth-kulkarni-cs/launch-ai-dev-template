# Explain Code Command

## Goal
Provide clear, contextual explanation of code sections.

## Inputs
- File path
- Line range (optional)
- Specific question (optional)

## Steps

1. **Read Target Code**
   - Load the specified file
   - If line range provided, focus on that section
   - Include surrounding context for understanding

2. **Analyze Structure**
   - Identify the purpose of the code
   - Map dependencies and imports
   - Note design patterns used

3. **Trace Data Flow**
   - Follow input through transformations
   - Identify side effects
   - Note async operations

4. **Check Related Skills**
   - Reference relevant skill files
   - Note if code follows or deviates from patterns
   - Identify any anti-patterns

5. **Generate Explanation**
   - Start with high-level purpose
   - Explain key sections
   - Note important details
   - Highlight potential issues

## Constraints
- Explain at appropriate level of detail
- Reference actual code, not abstractions
- Note deviations from project patterns
- Don't over-explain obvious code

## Output Schema

```markdown
## Code Explanation

### Overview
{2-3 sentences describing what this code does and why}

### Purpose
{Why this code exists, what problem it solves}

### How It Works

#### {Section 1 Name}
```{language}
// Relevant code snippet
```
{Explanation of this section}

#### {Section 2 Name}
```{language}
// Relevant code snippet
```
{Explanation of this section}

### Key Details
- **{Detail 1}:** {Explanation}
- **{Detail 2}:** {Explanation}

### Dependencies
- `{Import 1}` - {What it's used for}
- `{Import 2}` - {What it's used for}

### Patterns Used
- {Pattern 1}: {How it's applied}
- {Pattern 2}: {How it's applied}

### Potential Issues
- {Any concerns or improvements to note}

### Related Code
- `{Related file 1}` - {Relationship}
- `{Related file 2}` - {Relationship}
```
