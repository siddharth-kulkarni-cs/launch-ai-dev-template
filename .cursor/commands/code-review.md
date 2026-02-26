# PR Review Command

> Trigger: `/code-review`
> Performs an automated code review against project standards.

---

## Instructions

When the user runs `/code-review`, perform the following review on the staged or changed files:

### Step 1: Load Context
1. Read `skills/skill.md` for repo-wide patterns.
2. Identify which modules are affected by the changes.
3. Read the relevant `skills/{module}-skill/skill.md` for each affected module.
4. Read `skills/code-review-skill/skill.md` for review criteria.
5. Read `skills/code-review-skill/references/review-checklist.md` for the checklist.

### Step 2: Analyze Changes
Review every changed file against the **Review Categories** defined in `skills/code-review-skill/skill.md`

### Step 3: Report
Present findings organized by severity:

```
## 🔴 Critical (must fix)
- [file:line] Description of issue

## 🟡 Suggestions (should fix)
- [file:line] Description of suggestion

## 🟢 Nits (optional)
- [file:line] Minor style or readability note

## ✅ What looks good
- Brief positive notes on well-written code
```

If no issues are found, confirm the code looks clean and meets standards.
Do not update the code in case if issues found. Just report on the issues identified.
