# AI-fication Playbook

**For AI Agents: Execute to create AI enablement layer**

## Goal

Analyze repository and generate structured `/ai` directory that works across IDE agents (Cursor, Claude Code, Copilot, etc.)

[Important] Apart from the instructions given below, strictly follow all the mandatory skills that are present in this repository.


## Phase 0: Repository Analysis (5-10 min)

**Scan order:** README → package manager file → test dir → entry points → config files

**Extract:**
- **tech_stack:** language, framework, test_framework, package_manager
- **patterns:** has_tests, has_docker, has_local_setup, needs_debugging_docs
- **application_logic:** entry_points, key_modules, integrations, data_flows
- **critical_paths:** services, boundaries, dependencies, files not to modify

**Application Logic Discovery:**
```
FOR EACH entry file:
  1. Read, identify imports
  2. Map dependencies
  3. Identify routing/dispatch
  4. Track data transforms
  5. List external integrations
```

**Decision Matrix:**
- Has tests → Create testing skill (mandatory)
- Has Docker → Create docker-dev skill
- Has setup script → Create local-setup skill
- Complex errors → Create debugging skill
- Request routing >100 lines → Create request-flow skill
- Multi-cloud/provider → Create providers skill
- Auth flows >50 lines → Create auth-flow skill
- Data transforms 3+ steps → Create pipeline skill

## Phase 1: Create Folder Structure

```
/ai
├── README.md
├── context/
│   ├── architecture.md
│   ├── domain.md
│   └── glossary.md
├── commands/
├── skills/
├── rules/
├── examples/
└── index.json
```

## Phase 2: Generate Context Files

**architecture.md** → system overview, service boundaries, data flow, external dependencies, critical paths

**domain.md** → business concepts, entities, important events, metrics

**glossary.md** → important terminology used in repo

**Keep:** Concise, structured, synthesize (don't duplicate existing docs)

## Phase 3: Generate Rules

Create rule files covering:
- Coding conventions
- Architectural constraints
- Performance and safety guardrails
- Files/folders that must not be modified automatically

## Phase 4: Generate Core Commands (as markdown)

Create at least:
- **pr-summary**
- **test-generation**
- **refactor-module**
- **incident-analysis**
- **explain-code**

**Each command must define:** goal, inputs, steps, constraints, output schema

## Phase 5: Generate Mandatory Skills (6 × 200-500 lines)

**File format:** `/ai/skills/{name}-skill.md` (flat, no nested folders)

### 1. testing-skill.md
**Discover:** Find test dir, identify framework, read 2-3 tests, extract patterns
**Sections:** File Naming, Test Pattern (real code), Writing Tests, Mocking (framework-specific), Running Tests, Common Pitfalls
**Must:** Real code examples from repo tests

### 2. code-style-skill.md
**Discover:** Check linter config, read 5-10 source files for patterns (naming, organization, comments, errors), check formatter
**Sections:** Naming (functions, vars, constants, classes), File Organization, Comments Policy, Error Handling, {Language}-Specific
**Must:** Real patterns from codebase

### 3. commit-format-skill.md
**Discover:** `git log --oneline -20`, check CONTRIBUTING.md
**Sections:** Format (observed), Get Branch Name, Good Messages (why not what), When to Commit, Workflow
**Must:** Real examples from git log

### 4. code-review-skill.md
**Sections:** Test Checklist, Code Style Checklist, Security Checklist, Commit Checklist, Functional Review, Feedback Format
**Must:** Cross-reference all other skills

### 5. security-skill.md
**Discover:** Check .env.example, identify secret types, auth patterns, input validation
**Sections:** Never Commit Secrets (specific types), Environment Variables (how loaded), Input Validation, Auth Patterns, Logging Security
**Must:** Real patterns from code

### 6. creating-skills-skill.md
**Fixed content:** When/Not to Create, Structure, AI Guidelines
**Rules:** Major feature YES, bug fix NO, focus implementation not explanation.  Also update index.json if a new rule is created if applicable.  Add the reference to .cursor/skills folder.

## Phase 6: Application Logic Skills (as needed)

**Identify:** Trace request flow, find integrations, scan business modules, check data pipelines

**Template:**
```markdown
---
name: {name}
description: {What it does}. {When runs}. {Key integrations}. Use when {scenarios}.
---

# {Title}

## When to Use
- {3-5 scenarios}

## Overview
{High-level + responsibilities}

## Flow
### Step 1: {Entry}
**File:** `{path}`
{Description}
```{lang}
{Real code}
```

### Step 2-N: {Continue}
{Decision trees, real code}

## Data Structures
{Input/output from code}

## Integration Points
{Per service: file, auth, operations, real code}

## Configuration
{Env vars, schema from code}

## Common Patterns
{Real use cases with code}

## Performance
{Metrics, optimizations}

## Common Pitfalls
❌ **Problem:** {Real issue}
✅ **Solution:** {Fix with code}

## Validation
- [ ] {Checklist}

## Related Skills
{Links}

## Troubleshooting
**Issue:** {Real problem}
**Fix:** {Solution}
```

**Naming examples:** request-flow, cloud-providers, {domain}-logic, {purpose}-pipeline, deployment-lookup, auth-flow

## Phase 7: Optional Skills (as needed)

**local-setup:** Prerequisites, install, config, verify
**debugging:** Logs, common errors, analysis, tools
**docker-dev:** Setup, commands, profiles, debug

## Phase 8: Reusable Skills (workflow patterns)

Examples:
- large refactor workflow
- debugging workflow
- schema migration workflow
- observability investigation workflow



## Phase 10: Create index.json

Machine-readable manifest listing:
- critical services
- entry points
- commands
- rules
- important paths

## Phase 11: Create AGENTS.md (50-80 lines, root)

```markdown
# Development Guidelines - {repo-name}

> Quick reference. See `/ai/skills/` for guides.

## Standards

**Testing:** {rule}
→ [Testing](/ai/skills/testing-skill.md)

**Code Style:** {rule}
→ [Code Style](/ai/skills/code-style-skill.md)

**Commits:** {format}
→ [Commit Format](/ai/skills/commit-format-skill.md)

**Security:** {rule}
→ [Security](/ai/skills/security-skill.md)

**Reviews:** {process}
→ [PR Review](/ai/skills/code-review-skill.md)

## Architecture

{one-line description}

**Key flows:**
- [{Flow}](/ai/skills/{flow}-skill.md)

## Finding Skills

```bash
ls ai/skills/
grep -r "description:.*keyword" ai/skills/*-skill.md
cat ai/skills/{name}-skill.md
```

**Categories:**
- **Dev:** testing, code-style, commit-format, code-review, security, creating-skills
- **Setup:** {list if exists}
- **Logic:** {list if exists}
```

## Phase 12: Create Cursor Instructions

Add `.cursor/{placeholder}-rules.md` that:
- Each rule that is added to ai/rules folder should have a corresponding rule in .cursor folder that links to this folder
- Explains explains a placeholder rule
- Points to `/ai/rules` as source of truth
- Instructs agent to consult commands and rules before changes
- Lists critical constraints

Add `.cursor/{placeholder}-skills.md` that:
- Each skill that is added to ai/skills folder should have a corresponding skill in .cursor folder that links to this folder
- Explains explains a placeholder skill
- Points to `/ai/skills` as source of truth


Add `.cursor/{placeholder}-command.md` that:
- Each command that is added to ai/commands folder should have a corresponding command in .cursor folder that links to this folder
- Explains explains a placeholder command
- Points to `/ai/commands` as source of truth
- Instructs agent to consult commands and rules before changes
- Lists critical constraints

## Content Rules

**MUST:**
2. "When to Use" (3-5 scenarios)
3. Real code from repo (with file paths)
4. 3-5 Common Pitfalls
5. Related Skills links
6. 3-5 Troubleshooting items

**MUST NOT:**
- Generic examples
- Copy-pasted docs
- Multiple alternatives
- Long explanations

## Constraints

- Prefer concise summaries over large documentation dumps
- Do not duplicate existing docs; synthesize
- Use clear structure and bullet points
- Assume this layer will be used by multiple IDE agents
- Focus implementation not explanation

## Execution Summary

1. Analyze (5-10min): tech stack + logic + critical paths
2. Create /ai structure
3. Generate context files (architecture, domain, glossary)
4. Generate rules
5. Generate core commands
6. Create 6 mandatory skills (30-60min)
7. Create application logic skills (30-90min)
8. Create optional skills (15-30min)
9. Create workflow skills
10. Create examples
11. Create index.json
12. Create AGENTS.md (5min): 50-80 lines
13. Create .cursor/rules.md

**Total:** 2-4 hours
**Output:** AI-fied repo with discoverable, cross-IDE compatible layer