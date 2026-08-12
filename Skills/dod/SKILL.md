---
name: dod
description: Definition of Done checker - validates that a feature meets all completion criteria before marking it as done
triggers:
  - User asks to check DoD
  - User asks to verify feature completion
  - User says "check if done" or "validate completion"
  - Feature development is complete and needs validation
---

# DoD (Definition of Done) Checker

Systematically validate that a feature meets all Definition of Done criteria before marking it as complete.

## When to Use

- After completing a feature implementation
- Before marking a feature as "done" in requirements
- When user asks to verify completion status
- As part of the feature acceptance process

## Validation Process

### Step 1: Code Quality Checks

Run automated checks:

```bash
# Run linter
npm run lint

# Run type checker (if TypeScript)
npx tsc --noEmit

# Run all tests
npm test

# Check test coverage
npm run test:coverage
```

Verify:
- [ ] All tests pass (unit + integration + e2e)
- [ ] No linting errors
- [ ] No TypeScript errors
- [ ] Test coverage meets requirements:
  - Overall coverage ≥ 80%
  - Critical paths coverage = 100%
  - Statements coverage ≥ 80%
  - Branches coverage ≥ 80%
  - Functions coverage ≥ 80%
  - Lines coverage ≥ 80%

### Step 2: Functionality Validation

Check against acceptance criteria:
- [ ] Read the feature's `req-*.md` file
- [ ] Verify each acceptance criterion is implemented
- [ ] Confirm all scenarios pass (Given-When-Then)
- [ ] Test error handling and edge cases
- [ ] Verify frontend-backend integration (if applicable)

### Step 3: Documentation Sync

Verify documentation is up-to-date:
- [ ] Check if `req-*.md` reflects actual implementation
- [ ] Check if `design-*.md` reflects actual architecture
- [ ] Check if `ui-*.md` reflects actual UI (if applicable)
- [ ] Verify API documentation is current (if applicable)
- [ ] Check if README needs updates

### Step 4: Version Control

Verify Git status:
- [ ] All changes are committed
- [ ] Commit messages are clear and descriptive
- [ ] No untracked files (except intentional ignores)
- [ ] Branch is pushed to remote
- [ ] No merge conflicts

### Step 5: Non-Functional Requirements

Check quality attributes:
- [ ] No obvious performance issues (test manually)
- [ ] No security vulnerabilities (check for SQL injection, XSS, etc.)
- [ ] Proper error messages shown to users
- [ ] Logging is in place for debugging

### Step 6: User Acceptance

Final validation:
- [ ] Feature is testable by user (deployed or runnable locally)
- [ ] User has manually tested the feature
- [ ] User confirms feature meets expectations
- [ ] No blocking bugs reported

## Output Format

Present results as a checklist with status:

```markdown
## DoD Validation Results for [Feature Name]

### ✅ Code Quality
- ✅ All tests pass (45/45)
- ✅ No linting errors
- ✅ No TypeScript errors
- ✅ Test coverage: 87% overall (meets ≥80% requirement)
  - Statements: 87.5%
  - Branches: 85.2%
  - Functions: 90.1%
  - Lines: 87.3%
  - Critical paths: 100%

### ✅ Functionality
- ✅ All 4 acceptance criteria implemented
- ✅ Error handling verified
- ✅ Edge cases tested

### ⚠️ Documentation
- ✅ req-feature.md up-to-date
- ⚠️ design-feature.md needs update (API endpoint changed)
- ✅ README updated

### ✅ Version Control
- ✅ All changes committed (3 commits)
- ✅ Pushed to origin/feature-branch
- ✅ No untracked files

### ✅ Non-Functional
- ✅ Performance acceptable
- ✅ No security issues found
- ✅ Error messages clear

### ⏳ User Acceptance
- ⏳ Pending user testing

---

**Status: BLOCKED** - Design documentation needs update before marking as done.
**Action Required:** Update design-feature.md to reflect API changes, then re-run DoD check.
```

## Status Indicators

- ✅ **PASS** - All criteria met, feature can be marked as done
- ⚠️ **WARNING** - Minor issues found, fix before marking done
- ❌ **BLOCKED** - Critical issues found, must fix before proceeding
- ⏳ **PENDING** - Waiting for external input (e.g., user testing)

## After Validation

If all checks pass:
1. Mark the feature as ✅ completed in `req-*.md`
2. Add completion timestamp and validator name
3. Notify user that feature is done

If checks fail:
1. List all failing items clearly
2. Provide actionable steps to fix each issue
3. Do NOT mark feature as done
4. Re-run DoD check after fixes

## Important Notes

- Never skip checks to "save time"
- If a check cannot be automated, perform it manually
- Document any exceptions or waivers explicitly
- DoD is a quality gate, not a suggestion

## Example Usage

**User:** "Check if the login feature is done"

**Assistant:**
1. Runs all automated checks (lint, tests, coverage)
2. Reads `specs/login/req-login.md` to verify ACs
3. Checks documentation sync
4. Verifies Git status
5. Presents checklist with status
6. Either marks as done or lists blockers
