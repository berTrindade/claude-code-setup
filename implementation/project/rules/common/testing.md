# Testing Requirements

## Minimum Test Coverage: 80%

All three test types are required:

1. **Unit Tests** — individual functions, utilities, pure logic
2. **Integration Tests** — API endpoints, database operations, service interactions
3. **E2E Tests** — critical user flows (pet onboarding, lab result viewing, care plan generation)

## Test-Driven Development (Mandatory)

1. Write test first (RED) — it must fail
2. Write minimal implementation (GREEN) — make it pass
3. Refactor (IMPROVE) — keep tests green
4. Verify coverage 80%+

## Troubleshooting Test Failures

1. Use the `tdd-guide` agent
2. Check test isolation — tests must not share state
3. Verify mocks are correct (Cognito, DB, lab API)
4. Fix the implementation, not the tests (unless the test is genuinely wrong)

## Agent Support

- **tdd-guide** — use proactively for new features; enforces write-tests-first
- **build-error-resolver** — when tests won't compile
