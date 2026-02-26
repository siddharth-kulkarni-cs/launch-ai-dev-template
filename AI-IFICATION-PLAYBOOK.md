# AI-Assisted Development — Repository Setup Guide

## Purpose

This guide provides the standard structure for adding Cursor rules, commands, and skill files to every repository in our product. The goal is to give AI coding assistants (Cursor, Claude, Copilot) the context they need to produce code that fits our codebase — not just code that works.

---

## Directory Structure Overview

```
repo-root/
│
├── .cursor/                          # Cursor IDE configuration (committed to git)
│   ├── rules/
│   │   └── dev-workflow.mdc          # Global dev rules (can reference rules/dev-workflow.md)
│   ├── commands/
│   │   ├── pr-review.md              # Slash command: /pr-review
│   │   ├── execute-tests.md          # Slash command: /execute-tests
│   │   └── snyk-fixes.md             # Slash command: /snyk-fix
│   └── skills/
│       └── skills.md                 # Repo-wide skills entry  (can reference skills/skills.md)
│
├── rules/                            # Optional: full rule content (referenced by .cursor/rules)
│   └── dev-workflow.md               # Testing, naming, PR process, skill references
│
├── README.md                         # Repo overview + repo-wide skill (tech stack, run instructions)
│
├── skills/                           # Domain / module-specific skills
│   ├── skills.md                     # Index of all skills in this repo
│   ├── code-review-skill/
│   │   ├── skill.md                  # Code review automation skill
│   │   └── references/
│   │       ├── review-checklist.md   # PR review checklist (required)
│   │       └── common-pitfalls.md    # Known issues, anti-patterns (optional)
│   │
│   ├── _module-template-skill/       # Template to copy when creating a new module skill
│   │   └── skill.md
│   │
│   └── {module-name}-skill/          # One per major module / domain area
│       ├── skill.md                  # Module-specific patterns and recipes
│       ├── references/               # Supporting docs referenced by skill.md
│       │   └── api-contracts.md
│       ├── docs/                     # Detailed documentation (design docs, ADRs)
│       │   └── architecture.md
│       └── scripts/                  # Helper scripts for automation
│           └── seed-data.sh
│
└── ... (existing repo files)
```

---

## What Goes Where

| Location | Scope | Content | Consumed By |
|---|---|---|---|
| `README.md` | Repo-wide | Repo overview, tech stack, project structure, how to run; doubles as repo-wide skill | AI agents, developers, AGENTS.md |
| `.cursor/rules/` | Repo-wide | Should reference to rules defined in dev-workflow.md | (autoloaded by Cursor). |
| `.cursor/commands/` | Repo-wide | Slash commands for common workflows | Cursor IDE (manual trigger) |
| `.cursor/skills/` | Repo-wide | Should reference to skill.md inside the `skills` folder | Cursor IDE (auto-loaded) |
| `rules/dev-workflow.md` | Repo-wide | All development rules should be added here | Cursor transitively autoloaded cursor rule |
| `skills/{name}-skill/` | Module/domain | Deep module knowledge, recipes, references | AI agents, developers |


## Naming Conventions

| Item | Convention | Example |
|---|---|---|
| Repo overview | `README.md` (root) | Overview, tech stack, run instructions, related skills |
| Rule files | `kebab-case.md` | `dev-workflow.mdc` |
| Command files | `kebab-case.md` | `pr-review.md`, `execute-tests.md`, `snyk-fixes.md` |
| Skill directories | `{name}-skill/` | `order-service-skill/` |
| Module template | `_module-template-skill/` | Copy this to create new module skills |
| Skill entry point | Always `skill.md` | `skill.md` |
| Reference files | `kebab-case.md` | `review-checklist.md`, `common-pitfalls.md` |

---

## Maintenance Rules

1. **Skill files are code.** They go through PR review just like source code.
2. **Architectural changes require skill updates.** If you change a pattern, update the relevant skill.md in the same PR.
3. **Quarterly audit.** Each team reviews their skills quarterly for accuracy.
4. **Keep skills focused.** One skill per module/domain. If a skill exceeds ~300 lines, split it.
5. **References are for detail.** Keep skill.md concise and link to references/ for deep dives.
