---
name: bug-fixing
description: SDD workflow for fixing bugs - reproduce first, write failing test, fix, verify no regression
triggers:
  - User says "bug", "fix", "修复", "error", "broken", "not working"
  - User invokes /bug-fixing
  - A bug has been reported or discovered
---

# Bug Fixing Workflow

Fix bugs by reproducing first, writing a failing test, then fixing the code.

## Step 0: Reproduce and Locate

Before touching any code:

1. **Reproduce** - Find the trigger condition, confirm the bug is stable
2. **Locate root cause** - Read error messages, logs, and relevant code
3. **Assess impact** - Which features, tests, or users are affected?

Do NOT start modifying code until the root cause is identified.

---

## Step 1: Write a Failing Regression Test (Red)

Write a test that reproduces the bug before fixing it:

- **Logic error** → unit test
- **API behavior error** → integration test (real DB, no mocks)
- **User flow error** → e2e test (Playwright)

Run the test, confirm it fails (Red).

Commit: `test: reproduce bug - [brief description]`

---

## Step 2: Fix the Code (Green)

- Fix only the root cause — no unrelated changes
- Run the new test, confirm it passes (Green)
- Run all tests, confirm no regressions
- If regressions found, fix them before continuing

Commit: `fix: [brief description]`

---

## Step 3: Update Specs (if needed)

If the bug was caused by an unclear or incorrect spec:
- Use `update-docs` skill to update the relevant AC or design spec
- Commit: `docs: clarify spec for [scenario]`

If the bug was a plain code error, skip this step.

---

## Step 4: DoD Validation

Use the `dod` skill to run the full checklist.

If all checks pass, record in `req-feature-name.md` (if applicable):
```
🐛 Bug 修复 - YYYY-MM-DD by [name]: [brief description]
```

---

## Important Notes

- Never fix a bug without a regression test — the bug will come back
- Never modify tests to make them pass around the bug
- If root cause is unclear after investigation, pause and ask the user
