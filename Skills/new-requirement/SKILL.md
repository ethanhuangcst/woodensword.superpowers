---
name: new-requirement
description: SDD workflow for implementing a new feature - guides through spec writing, TDD development, and DoD validation
triggers:
  - User says "开发新功能", "新需求", "implement feature", "add feature"
  - User invokes /new-requirement
  - User starts a new feature development task
---

# New Requirement Workflow

Execute the SDD (Spec-Driven Development) workflow for a new feature.

## Step 0: Determine Feature Type and Scope

Before starting, clarify:
- Is this a **brand new feature** or a **sub-feature/user story of an existing feature**?
- What is the feature name? (used for folder/file naming)
- **List all user stories** in this feature — confirm with user before proceeding

**New feature** → create `specs/feature-name/` folder
**Sub-feature / user story** → update existing `specs/parent-feature/` files

> ⚠️ Per `Incremental Delivery` rule: **implement one user story at a time**.
> After listing all user stories, ask the user: "Which user story should we start with?"

---

## Step 1: Write Specs (for the selected user story only)

### 1.1 Requirements Spec (req)
Use the `atdd` skill to write user stories and acceptance criteria.

- **New feature**: create `specs/feature-name/req-feature-name.md`
- **Sub-feature**: insert new user stories and ACs into the existing `req-feature-name.md` at the appropriate location
- Write ACs only for the **current user story** — not the entire feature

### 1.2 Design Spec (design)
Use the `Fullstack` skill to write technical architecture, API design, and data models.

- Scope the design to what is needed for **this user story only**
- Do not design APIs or data models for future user stories

### 1.3 UI Spec (ui)
Use the `Frontend Design` skill to write UI layout, interactions, and components.

- Scope the UI to what is needed for **this user story only**

### 1.4 Consistency Check
Verify the three specs are consistent:
- [ ] Design spec APIs cover all requirement scenarios
- [ ] UI spec elements cover all user actions in requirements
- [ ] Design data models support all UI display needs
- [ ] Terminology is consistent across all three specs
- [ ] No missing features or edge cases

### 1.5 User Review
Present the specs to the user for approval. If not approved, revise and re-check consistency.

---

## Step 2: TDD Development

Use the `tdd` skill. Follow `Incremental Delivery` and `test-strategy` rules.

### Test cases come from the ACs in req spec

Each Gherkin Scenario in `req-feature-name.md` maps directly to automated tests:
- **Unit tests** - business logic functions (Jest/Vitest)
- **Integration tests** - API endpoints, database operations (real DB, not mocks)
- **E2E tests** - critical user flows (Playwright)

### Red-Green-Refactor-Commit cycle (one Scenario at a time):
1. **Red** - Convert the Scenario's Given/When/Then into a test, confirm it fails
2. **Green** - Write minimal code to make that test pass
3. **Refactor** - Clean up, keep test green
4. **Commit** - Immediately after passing: `test: [Scenario name]` or `feat: implement [Scenario name]`
5. Move to next Scenario — repeat until all ACs have passing tests

### Doc sync:
If specs need changes during development, use `update-docs` skill and commit with `docs: update spec for [reason]`.

---

## Step 3: DoD Validation

Use the `dod` skill to run the full checklist automatically.

If all checks pass → mark **this user story** as ✅ complete in `req-feature-name.md`:
```
✅ 已完成 - YYYY-MM-DD by [name]
```

If any check fails → fix issues and re-run DoD. Do NOT mark as complete.

After DoD passes → ask user: "This user story is done. Start the next one?"

---

## Skills & Rules Used

| Purpose | Skill/Rule |
|---------|-----------|
| Requirements spec | `atdd` |
| Design spec | `Fullstack` |
| UI spec | `Frontend Design` |
| TDD development | `tdd` |
| Doc sync | `update-docs` |
| DoD validation | `dod` |
| Delivery discipline | `Incremental Delivery` rule |
| Test quality | `test-strategy` rule |
