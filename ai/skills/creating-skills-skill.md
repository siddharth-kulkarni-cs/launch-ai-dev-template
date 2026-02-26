---
name: creating-skills
description: Guidelines for creating new skill files. Use when adding domain knowledge, documenting patterns, or extending AI capabilities.
---

# Creating Skills Skill

## When to Use
- Adding a new domain or module skill
- Documenting complex patterns
- Extending AI assistant capabilities
- Capturing tribal knowledge

## When NOT to Create a Skill

❌ **Don't create a skill for:**
- Bug fixes (just fix the bug)
- Simple features (< 3 files changed)
- One-time tasks
- Generic programming concepts
- Information already in existing skills

## When to Create a Skill

✅ **Create a skill when:**
- A module has 3+ developers working on it
- Domain-specific patterns aren't covered elsewhere
- You repeatedly explain the same thing
- Complex integrations need documentation
- Major feature with unique patterns

## Skill Structure

### Required Sections
```markdown
---
name: {skill-name}
description: {One line}. {When runs}. {Key integrations}. Use when {scenarios}.
---

# {Title} Skill

## When to Use
- {3-5 specific scenarios}

## Overview
{High-level description + responsibilities}

## {Main Content Sections}
{Real code from repo with file paths}

## Common Pitfalls
❌ **Problem:** {Real issue}
✅ **Solution:** {Fix with code}

## Related Skills
- [{Skill}](/ai/skills/{skill}-skill.md)

## Troubleshooting
**Issue:** {Real problem}
**Fix:** {Solution}
```

### File Location
All skills go in `/ai/skills/` as flat files:
```
/ai/skills/{name}-skill.md
```

Examples:
- `testing-skill.md`
- `order-service-skill.md`
- `payment-flow-skill.md`

## Content Guidelines

### MUST Include
1. **Frontmatter** with name and description
2. **"When to Use"** section (3-5 scenarios)
3. **Real code** from the repo (with file paths)
4. **3-5 Common Pitfalls** with solutions
5. **Related Skills** links
6. **3-5 Troubleshooting** items

### MUST NOT Include
- Generic examples (use real code)
- Copy-pasted external docs
- Multiple alternative approaches (pick one)
- Long explanations (be concise)
- Implementation details that change frequently

### Code Examples
```markdown
## Pattern Name

**File:** `src/orders/order.service.ts`
```typescript
// Real code from the repo
export class OrderService {
  async createOrder(input: CreateOrderInput): Promise<Order> {
    // Implementation
  }
}
```
```

### Pitfall Format
```markdown
## Common Pitfalls

❌ **Problem:** Floating promises in async handlers
```typescript
// Bad: Error is swallowed
app.post('/orders', (req, res) => {
  orderService.create(req.body); // No await!
  res.json({ success: true });
});
```
✅ **Solution:** Always await async operations
```typescript
// Good: Properly awaited
app.post('/orders', async (req, res) => {
  const order = await orderService.create(req.body);
  res.json({ success: true, orderId: order.id });
});
```
```

## Quality Checklist

Before submitting a skill PR:

- [ ] Under 300 lines (link to references for detail)
- [ ] Contains 5-15 line code snippets (not walls of code)
- [ ] Has "Common Tasks" with step-by-step recipes
- [ ] Has "Pitfalls" with negative examples
- [ ] All code examples are from the actual repo
- [ ] Frontmatter description is complete
- [ ] Related skills are linked
- [ ] Troubleshooting section has real issues

## Updating Skills

### When to Update
- You introduce a new pattern → update relevant skill
- You find a new gotcha → add to Pitfalls
- Architecture changes → update affected skills
- Code examples become outdated → refresh them

### Update Process
1. Make skill update in the same PR as code change
2. Keep the skill under 300 lines
3. Update Related Skills if adding new skill
4. Update `/ai/skills/` index if needed

## Skill Categories

| Category | Purpose | Examples |
|----------|---------|----------|
| Dev | Development workflow | testing, code-style, commit-format |
| Setup | Environment setup | local-setup, docker-dev |
| Logic | Application logic | request-flow, auth-flow, payment-flow |
| Domain | Business domain | order-service, user-management |

## Common Pitfalls

❌ **Problem:** Skill too long (>300 lines)
✅ **Solution:** Extract details to reference files, keep skill focused

❌ **Problem:** Generic examples instead of real code
✅ **Solution:** Always use actual code from the repository

❌ **Problem:** Missing "When to Use" section
✅ **Solution:** Always start with 3-5 specific use scenarios

❌ **Problem:** Skill duplicates existing documentation
✅ **Solution:** Synthesize and link, don't copy

❌ **Problem:** No pitfalls section
✅ **Solution:** Every skill needs real pitfalls from experience

## Related Skills
- [Code Style](/ai/skills/code-style-skill.md) - File naming conventions
- [PR Review](/ai/skills/pr-review-skill.md) - Skill review checklist

## Troubleshooting

**Issue:** Skill not being picked up by AI
**Fix:** Check frontmatter format and file location

**Issue:** Skill is too broad
**Fix:** Split into focused skills by domain

**Issue:** Code examples outdated
**Fix:** Run grep to find current implementations, update examples
