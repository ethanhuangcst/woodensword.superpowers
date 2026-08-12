---
name: change-requirement
description: SDD workflow for handling requirement changes - updates specs, tests, and code while maintaining consistency
triggers:
  - User says "需求变更", "requirement change", "change requirement", "modify AC", "update spec"
  - User invokes /change-requirement
  - An existing AC or user story needs to be modified
---

# Change Requirement Workflow

Handle a requirement change by updating specs first, then tests, then code.

## Step 0: Assess Impact

Before making any changes, identify:

- **Change type:**
  - AC modification (existing scenario conditions/outcomes change)
  - AC addition (new scenario under existing user story)
  - User story modification (goal or actor changes)
  - New user story → use `/new-requirement` instead

- **Impact scope:**
  - Which Scenarios are affected?
  - Which existing tests need updating?
  - Which existing code needs updating?
  - Do design or UI specs need updating?

Present the impact assessment to the user and get confirmation before proceeding.

---

## Step 1: Update Specs

### 1.1 Update Requirements Spec
In `specs/feature-name/req-feature-name.md`:
- Mark old AC as `~~old content~~（已变更）`
- Write new AC below it
- Note the reason for change

### 1.2 Update Design Spec (if affected)
Modify impacted API, data model, or architecture sections in `design-feature-name.md`.

### 1.3 Update UI Spec (if affected)
Modify impacted UI/interaction sections in `ui-feature-name.md`.

### 1.4 Consistency Check
- [ ] All three specs reflect the change consistently
- [ ] No affected sections were missed

### 1.5 User Review
Get user confirmation that spec changes are correct before proceeding.

---

## Step 2: Update Tests and Code

### 2.1 Update tests first (Red)
- Find the automated test(s) for the affected Scenario
- Update the test to match the new AC
- Confirm the test now fails (Red)

### 2.2 Update implementation (Green)
- Modify code to make the updated test pass (Green)

### 2.3 Regression check
- Run all tests
- Fix any regressions before continuing

### 2.4 Commit
```
fix: update [feature] - [brief description of change]
docs: update spec for [reason]
```

---

## Step 3: DoD Validation

Use the `dod` skill to run the full checklist.

If all checks pass → record the change in `req-feature-name.md`:
```
🔄 需求变更 - YYYY-MM-DD by [name]: [brief description]
```

---

## Skills & Rules Used

| Purpose | Skill/Rule |
|---------|-----------|
| Spec update | `update-docs` |
| TDD update | `tdd` |
| DoD validation | `dod` |
| Delivery discipline | `Incremental Delivery` rule |
