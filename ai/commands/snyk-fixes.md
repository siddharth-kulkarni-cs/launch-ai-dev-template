# Snyk Vulnerability Fix Command

> Trigger: `/snyk-fix`
> Triages Snyk vulnerabilities and applies fixes using the appropriate strategy per issue.

---

## Instructions

When the user runs `/snyk-fix`, follow this workflow to scan, triage, and fix vulnerabilities.

### Step 1: Scan & Capture Results

Run Snyk and capture structured output for analysis.

```bash
# Full scan with JSON output for parsing
npx snyk test --json --severity-threshold=low > snyk-report.json

# Human-readable summary alongside
npx snyk test --severity-threshold=low --show-vulnerable-paths=all
```

If `snyk test` is not available or fails to authenticate:
```bash
# Check Snyk CLI is installed
npx snyk --version

# Authenticate if needed (opens browser)
npx snyk auth

# If behind a proxy or CI environment, use token
SNYK_TOKEN=<token> npx snyk test --json > snyk-report.json
```

If a `.snyk` policy file exists in the repo root, read it first to understand existing ignores and patches before proposing changes.

---

### Step 2: Triage by Severity and Fixability

Parse the JSON report and categorize every vulnerability into one of these buckets:

| Priority | Criteria | Action |
|---|---|---|
| **P0 — Critical, fixable** | Severity: critical + upgrade path exists | Fix immediately |
| **P1 — High, fixable** | Severity: high + upgrade path exists | Fix immediately |
| **P2 — Critical/High, transitive only** | No direct upgrade path | Override or patch |
| **P3 — Medium, fixable** | Severity: medium + upgrade path exists | Fix in this session |
| **P4 — Low / no fix available** | Low severity or no known fix | Ignore with justification |

Present the triage summary to the user before proceeding:

```
## Snyk Triage Summary

| Priority | Count | Strategy |
|----------|-------|----------|
| P0 Critical (fixable) | 2 | Direct upgrade |
| P1 High (fixable) | 3 | Direct upgrade |
| P2 Critical/High (transitive) | 1 | npm override |
| P3 Medium (fixable) | 4 | Direct upgrade |
| P4 Low / No fix | 2 | Ignore with policy |

Proceed with fixes? [Recommended: fix P0-P3, ignore P4]
```

---

### Step 3: Apply Fixes by Strategy

Work through each priority bucket using the appropriate fix strategy.

---

#### Strategy A: Direct Dependency Upgrade (P0, P1, P3)

This is the preferred fix. Snyk recommends a specific version — upgrade to it.

```bash
# 1. Identify the vulnerable package and the fix version from the Snyk report
#    Example: lodash@4.17.15 → fix in 4.17.21

# 2. Check what depends on it
npm ls <vulnerable-package>

# 3. Upgrade the direct dependency
npm install <package>@<fix-version> --save
# or for dev dependencies
npm install <package>@<fix-version> --save-dev

# 4. If it's a major version bump, check for breaking changes
npm outdated <package>
```

**After each upgrade:**
```bash
# Verify the vulnerability is resolved
npx snyk test --json | grep -A5 "<SNYK-ID>"

# Run tests to ensure nothing broke
npm run test:unit

# Type check
npx tsc --noEmit
```

**If a major version upgrade is required:**
1. Check the package's CHANGELOG / migration guide for breaking changes.
2. Search the codebase for usage: `grep -rn "require('<package>')\|from '<package>'" src/`
3. List the breaking changes to the user and ask for confirmation before applying.
4. After upgrading, run the full test suite, not just unit tests.

---

#### Strategy B: npm Overrides for Transitive Dependencies (P2)

When the vulnerability is in a transitive (nested) dependency and upgrading the direct parent doesn't resolve it.

```bash
# 1. Trace the dependency path
npm ls <vulnerable-transitive-package>

# Example output:
# my-app@1.0.0
# └── some-framework@3.2.0
#     └── vulnerable-lib@1.0.0    <-- this is vulnerable, fix is 1.0.5
```

```bash
# 2. First, try upgrading the direct parent
npm install some-framework@latest --save
npm ls <vulnerable-transitive-package>
# Check if the transitive dep version changed
```

```bash
# 3. If the parent upgrade didn't fix it, use npm overrides (npm 8.3+)
```

Add to `package.json`:
```json
{
  "overrides": {
    "<vulnerable-transitive-package>": "<fix-version>"
  }
}
```

For scoped overrides (only override within a specific parent):
```json
{
  "overrides": {
    "some-framework": {
      "vulnerable-lib": "1.0.5"
    }
  }
}
```

```bash
# 4. Reinstall and verify
rm -rf node_modules package-lock.json
npm install
npm ls <vulnerable-transitive-package>
npx snyk test --json | grep -A5 "<SNYK-ID>"
npm run test:unit
```

**Important considerations for overrides:**
- Overrides force a version across the entire tree — this can break packages that depend on a specific version range.
- Always run the full test suite after applying overrides.
- Add a comment in `package.json` explaining why the override exists:
  ```json
  {
    "overrides": {
      "// vulnerable-lib override": "SNYK-JS-VULNERABLELIB-123456 — fix transitive vuln from some-framework",
      "vulnerable-lib": "1.0.5"
    }
  }
  ```
- Revisit overrides when the direct parent releases an update that includes the fix. Overrides should be temporary.


### Step 4: Verify All Fixes

After all fixes are applied, run a complete verification:

```bash
# 1. Re-scan with Snyk
npx snyk test --severity-threshold=low

# 2. Run full test suite
npm run test:unit
npm run test:integration  # if available

# 3. Type check
npx tsc --noEmit

# 4. Lint (in case dependency changes affected imports)
npm run lint

# 5. Build (ensure the project still compiles)
npm run build
```

---

### Step 5: Report

Present the final summary:

```markdown
## Snyk Fix Report

### Fixed
| Vulnerability | Package | Strategy | Before → After |
|---|---|---|---|
| SNYK-JS-LODASH-1018905 | lodash | Direct upgrade | 4.17.15 → 4.17.21 |
| SNYK-JS-EXAMPLE-567890 | nested-lib | npm override | 1.0.0 → 1.0.5 |

### Ignored (with policy)
| Vulnerability | Package | Reason | Expiry |
|---|---|---|---|
| SNYK-JS-LOW-111111 | dev-tool | Test-only dependency | 2026-06-01 |

### Remaining (no fix available)
| Vulnerability | Package | Severity | Notes |
|---|---|---|---|
| SNYK-JS-NOFIX-999999 | legacy-pkg | Medium | Awaiting upstream fix |

### Verification
- [x] `snyk test` — X vulnerabilities remaining (was Y)
- [x] `npm run test:unit` — all passed
- [x] `tsc --noEmit` — no type errors
- [x] `npm run build` — success

### Files Changed
- `package.json` — dependency upgrades, overrides added
- `package-lock.json` — regenerated
- `.snyk` — ignore policies added
```

---

## Decision Tree (Quick Reference)

```
Vulnerability found
│
├─ Is it Critical or High severity?
│  ├─ YES → Is there a direct upgrade path?
│  │  ├─ YES → Strategy A: Direct Upgrade (do it now)
│  │  └─ NO → Is it a transitive dependency?
│  │     ├─ YES → Strategy B: npm Override
│  │     └─ NO → Strategy C: Snyk Patch (if available)
│  │            or Strategy E: Replace dependency
│  │
│  └─ NO (Medium/Low) → Is there a direct upgrade path?
│     ├─ YES → Strategy A: Direct Upgrade
│     └─ NO → Strategy D: Ignore with policy
│
└─ Is the vulnerability only in devDependencies?
   └─ YES → Lower priority. Strategy D if no easy fix.
            Note: still fix Critical in devDeps.
```

---

## Common Gotchas

- **`package-lock.json` drift.** After overrides or upgrades, always delete `node_modules` and `package-lock.json` and reinstall: `rm -rf node_modules package-lock.json && npm install`. Otherwise stale resolutions persist.
- **Monorepo hoisting.** In monorepos (Nx, Turborepo, Lerna), overrides in the root `package.json` apply globally. Per-package overrides may not work depending on your hoisting strategy. Run `npx snyk test --all-projects` to scan all packages.
- **Snyk reports transitive vuln as "no upgrade path"** — always check if upgrading the direct parent to its latest version resolves the transitive issue before resorting to overrides.
- **Override version conflicts.** If two direct dependencies require incompatible versions of the same transitive dependency, an override may break one of them. Always run tests.
- **`.snyk` file must be committed.** The ignore policies and patches only work if the `.snyk` policy file is in the repo root and committed to git.
- **`snyk test` exit codes.** Exit code `1` means vulnerabilities found (not a CLI error). Use `--severity-threshold=high` in CI to only fail on high/critical.
- **Docker base image vulns.** If Snyk flags OS-level vulnerabilities in your Docker image, those are fixed by updating the base image (`FROM node:20-alpine`), not by npm overrides. Use `snyk container test` for container-specific issues.
