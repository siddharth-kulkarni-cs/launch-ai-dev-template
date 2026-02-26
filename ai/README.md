# AI Development Layer

> Cross-IDE compatible AI enablement layer for this repository.
> Works with Cursor, Claude Code, Copilot, and other AI coding assistants.

## Directory Structure

```
/ai
├── README.md           # This file
├── context/            # Repository context for AI understanding
│   ├── architecture.md # System overview, boundaries, data flow
│   ├── domain.md       # Business concepts, entities, events
│   └── glossary.md     # Important terminology
├── commands/           # Slash commands for common workflows
├── skills/             # Flat skill files ({name}-skill.md)
├── rules/              # Development rules and constraints
├── examples/           # Code examples and patterns
└── index.json          # Machine-readable manifest
```

## Quick Start

1. **Find a skill:** `ls ai/skills/`
2. **Search skills:** `grep -r "description:.*keyword" ai/skills/*-skill.md`
3. **Read a skill:** `cat ai/skills/{name}-skill.md`

## Mandatory Skills

| Skill | File | Purpose |
|-------|------|---------|
| Testing | `testing-skill.md` | Test patterns, frameworks, coverage |
| Code Style | `code-style-skill.md` | Naming, organization, conventions |
| Commit Format | `commit-format-skill.md` | Commit messages, workflow |
| PR Review | `pr-review-skill.md` | Review checklist, feedback format |
| Security | `security-skill.md` | Secrets, validation, auth patterns |
| Creating Skills | `creating-skills-skill.md` | How to add new skills |

## Usage

AI agents should:
1. Read relevant context files before making changes
2. Consult skill files for domain-specific patterns
3. Follow rules for constraints and guardrails
4. Use commands for structured workflows
