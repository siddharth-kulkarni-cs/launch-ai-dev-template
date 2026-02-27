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

> **Cursor Note:** When creating a new command, you must also create a pointer file at `.cursor/commands/{name}.md` with the following content:
> ```markdown
> # {Name} Command
> 
> > Source of truth: `/ai/commands/{name}.md`
> 
> When asked to {action}, please read and follow the instructions in `/ai/commands/{name}.md`.
> ```

## Phase 5: Generate Mandatory Skills (7 × 200-500 lines)

**File format:** `/ai/skills/{name}-skill.md` (flat, no nested folders)

### 1. tdd-skill.md
**Fixed content:** Test-driven development with red-green-refactor loop.
**Sections:** Philosophy, Anti-Pattern: Horizontal Slices, Workflow (Planning, Tracer Bullet, Incremental Loop, Refactor), Checklist Per Cycle.
**Must:** Emphasize vertical slices and testing behavior over implementation.

### 2. testing-skill.md
**Discover:** Find test dir, identify framework, read 2-3 tests, extract patterns
**Sections:** File Naming, Test Pattern (real code), Writing Tests, Mocking (framework-specific), Running Tests, Common Pitfalls
**Must:** Real code examples from repo tests

### 3. code-style-skill.md
**Discover:** Check linter config, read 5-10 source files for patterns (naming, organization, comments, errors), check formatter
**Sections:** Naming (functions, vars, constants, classes), File Organization, Comments Policy, Error Handling, {Language}-Specific
**Must:** Real patterns from codebase

### 4. commit-format-skill.md
**Discover:** `git log --oneline -20`, check CONTRIBUTING.md
**Sections:** Format (observed), Get Branch Name, Good Messages (why not what), When to Commit, Workflow
**Must:** Real examples from git log

### 5. code-review-skill.md
**Sections:** Test Checklist, Code Style Checklist, Security Checklist, Commit Checklist, Functional Review, Feedback Format
**Must:** Cross-reference all other skills

### 6. security-skill.md
**Discover:** Check .env.example, identify secret types, auth patterns, input validation
**Sections:** Never Commit Secrets (specific types), Environment Variables (how loaded), Input Validation, Auth Patterns, Logging Security
**Must:** Real patterns from code

### 7. creating-skills-skill.md
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

## Workflow

**Before any code changes or investigation:**

1. **Load Context:** Read `/ai/context/architecture.md` for system understanding
2. **Check Skills:** Read relevant `/ai/skills/` files for the specific task
3. **Follow Rules:** Apply `/ai/rules/` constraints for all changes
4. **Then Proceed:** Start investigation or implementation

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
   
> **Note for Cursor Users:** Skills and Rules are natively auto-discovered via the `.cursor/` directory. You do not need to manually search for them.

For other AI agents, use the following commands to find relevant skills:

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

## Phase 12: Create IDE-Specific Pointer Files

To enable native auto-discovery while maintaining `/ai/` as the single source of truth, generate pointer files for specific IDEs:

**For Cursor:**
- **Rules:** Create `.cursor/rules/<name>.mdc` files. Include YAML frontmatter (`description`, `globs`) and instruct the agent to read the corresponding `/ai/rules/` file.
- **Commands:** Create `.cursor/commands/<name>.md` files. Instruct the agent to read the corresponding `/ai/commands/` file.
- **Skills:** Create `.cursor/skills/<name>/SKILL.md` files. Instruct the agent to execute the corresponding `/ai/skills/` file.

**For GitHub Copilot:**
- **Code Review Instructions:** Create `.github/copilot-instructions.md` file containing:
  - Review focus areas (testing, code style, security, commit format)
  - Architecture and layer boundaries
  - Error handling patterns
  - References to `/ai/skills/` for detailed standards
  - Template-specific review guidance
  - Common pitfalls and anti-patterns
  - Final review checklist

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
- Create documentation without explicit request
- Duplicate code instead of extracting common logic
- Add obvious or redundant comments

## Constraints

- Prefer concise summaries over large documentation dumps
- Do not duplicate existing docs; synthesize
- Use clear structure and bullet points
- Assume this layer will be used by multiple IDE agents
- Focus implementation not explanation
- Do not create markdown files for code changes unless explicitly asked (prompt user if unclear)
- Avoid code duplication; extract common logic into reusable components
- Avoid adding comments unless extremely necessary; code should be self-documenting

## Execution Summary

1. Analyze (5-10min): tech stack + logic + critical paths
2. Create /ai structure
3. Generate context files (architecture, domain, glossary)
4. Generate rules
5. Generate core commands
6. Create 7 mandatory skills (30-60min)
7. Create application logic skills (30-90min)
8. Create optional skills (15-30min)
9. Create workflow skills
10. Create examples
11. Create index.json
12. Create AGENTS.md (5min): 50-80 lines
13. Create IDE-Specific Pointer Files (Cursor `.mdc`, GitHub `.github/copilot-instructions.md`)

**Total:** 2-4 hours
**Output:** AI-fied repo with discoverable, cross-IDE compatible layer