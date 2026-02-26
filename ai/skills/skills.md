# Skills Index

> This directory contains domain-specific skill files for AI-assisted development.
> Each skill is a self-contained knowledge package for a module or cross-cutting concern.

---

## Available Skills

| Skill | Directory | Description |
|---|---|---|
| Code Review | `code-review-skill/` | PR review automation — quality, security, performance checks |
| {Module A} | `{module-a}-skill/` | {Brief description} |
---

## Creating a New Skill

1. Copy `_module-template-skill/` as `{your-module}-skill/`.
2. Fill in all `{placeholder}` values in `skill.md`.
3. Add reference docs in `references/` as needed.
4. Add design docs in `docs/` if applicable.
5. Add helper scripts in `scripts/` if applicable.
6. Update this Available Skills table.
7. Submit a PR for review.

---

## Skill Anatomy

```
{name}-skill/
├── skill.md          # Entry point — always read this first
├── references/       # Supporting docs (pitfalls, checklists, API contracts)
├── docs/             # Design documents, ADRs, architecture diagrams
└── scripts/          # Automation helpers (seed data, migrations, etc.)
```

**Rules:**
- `skill.md` should be under 300 lines. Link to `references/` for detail.
- Include code snippets (5–15 lines) — show, don't just tell.
- Include a "Pitfalls" section — what NOT to do is as valuable as what to do.
- Update the skill in the same PR when you change the module's patterns.
