---
name: tdd
description: >
  Test-Driven Development workflow for features, bugs, and refactors. Use whenever implementing
  business logic, API routes, data transforms, or auth — especially after acceptance criteria exist.
  Follow Red-Green-Refactor, write tests before production code, and meet the common-test-strategy
  bar (critical path 100%, overall ≥80% where measurable). Pair with the atdd skill for Given-When-Then
  criteria first; use webapp-testing for browser verification. Do not skip tests for logic/API work
  even if the user only said "implement" or "fix".
---

# TDD

Implement behavior with **tests first**, then minimal code, then refactor. This skill **extends** the always-on **common-test-strategy** rule — it does not weaken mocks, coverage, or pyramid expectations.

## When to use

- New feature or user story (after or alongside **atdd** acceptance criteria)
- Bug fixes (add a failing test that reproduces the bug, then fix)
- Refactors (keep tests green; add tests first if coverage is missing)
- New API endpoints, services, or non-trivial helpers

Skip only for pure presentational UI with no logic when the project test strategy explicitly allows it.

## Workflow

1. **Clarify behavior** — From acceptance criteria (Gherkin or checklist) or a one-line behavior statement.
2. **Red** — Write a failing test named `should_[expected]_when_[condition]`. One behavior per test. AAA structure.
3. **Green** — Smallest change to pass; no speculative features.
4. **Refactor** — Clean up with tests still passing.
5. **Verify** — Run project test + lint + typecheck commands; check coverage if the project reports it.

## Project paths

Discover paths from the repo — do not assume a fixed monorepo layout:

- Test runner: `package.json` scripts (`test`, `test:unit`, `test:e2e`, `test:coverage`)
- Unit/integration: colocated `*.test.ts(x)` or `tests/` / `__tests__/` per project convention
- E2E: `e2e/`, `tests/e2e/`, or Playwright config path
- Specs/stories: `specs/`, `docs/`, or `specs/user-stories/` if the project uses them

## Integration with other skills / rules

| Need | Use |
| --- | --- |
| Acceptance criteria before coding | **atdd** skill |
| DoD / quality checklist | **common-test-strategy** rule |
| One story at a time | **incremental-delivery** rule |
| Browser automation | **webapp-testing** skill |
| Copy-paste examples | `references/test-patterns.md` |

## Mocks and real integrations

Prefer **real test database**, fixture APIs, or approved sandboxes over inventing success payloads in production paths. Mock only where **common-test-strategy** allows (time, randomness, paid third parties without sandbox, etc.). Do not ship features that work only against mocks unless the user explicitly allows it.

## Anti-patterns

- Implementation-only tests (asserting private state instead of observable outcomes)
- Brittle selectors in E2E (prefer `role`, accessible name, `data-testid`)
- `waitForTimeout` as the primary sync strategy on dynamic apps
- Deleting or skipping failing tests to go green
- Writing production code before any failing test for non-trivial logic

## Output when finishing a TDD cycle

Briefly report:

- Tests added/updated (paths)
- Commands run and result
- Coverage note if available
- Any behavior left untested and why (must align with allowed skip list)
