---
name: e2e-testing
description: >
  Design and implement an end-to-end testing strategy that validates the full
  system from user input to expected output. Covers test pyramid strategy,
  coverage planning, test design, and maintenance. Use this skill when setting
  up a testing strategy for a new project, when existing tests don't catch
  enough real bugs, when tests are too slow or flaky, or when preparing for
  a major release. Trigger phrases: "test strategy", "e2e testing", "how do
  we test this?", "test coverage", "integration testing", "tests keep breaking",
  "too many flaky tests", "what should we test?", "testing approach",
  "acceptance testing", "our tests don't catch real bugs".
---

# End-to-End Testing Skill

Builds a testing strategy that catches real problems before users do.
Tests that don't catch bugs are overhead. Tests that catch bugs are insurance.

## Core Principle

> Test at the level that gives you the most confidence for the least
> cost. E2E tests are expensive to write and maintain — use them for
> the paths that matter most. Unit tests are cheap — use them for logic.
> The right mix is more important than total coverage percentage.

---

## The Test Pyramid

```
                    /\
                   /E2E\          Few — test critical user journeys only
                  /──────\
                 /Integr- \       Some — test component interactions
                /  ation   \
               /────────────\
              /   Unit Tests  \   Many — test individual functions and logic
             /──────────────────\
```

**Rule:** The pyramid should not be inverted (many E2E, few unit tests).
An inverted pyramid is slow, expensive, and fragile.

---

## Phase 1: Test Strategy

### Define What Needs Testing

```
CRITICAL USER JOURNEYS (must have E2E tests):
  List the 5-10 flows that, if broken, would immediately cause user harm
  or business loss. Examples:
    □ User can sign up and log in
    □ User can complete a purchase
    □ Core feature works end-to-end
    □ Data can be saved and retrieved correctly

COMPONENT INTERACTIONS (integration tests):
  □ API calls return correct data given specific inputs
  □ Database read/write operations work correctly
  □ Third-party service integrations work as expected
  □ Events trigger correct downstream actions

BUSINESS LOGIC (unit tests):
  □ Calculations are correct
  □ Validation rules are enforced
  □ Edge cases handled (empty, null, max values)
  □ Error handling works
```

### Coverage Targets

```
NOT "100% line coverage" — that is a metric, not a goal.
Real goal: confidence that the important paths work.

Practical targets:
  Critical user journeys:   100% covered by at least one E2E test
  API endpoints:            80%+ covered by integration tests
  Business logic functions: 90%+ covered by unit tests
  Error handling paths:     key errors covered; not necessarily all

What to skip:
  □ Trivial getters/setters (low value, high maintenance)
  □ Third-party library internals (not your code)
  □ One-time migration scripts (test the data, not the script)
```

---

## Phase 2: Test Design

### Good Test Design Principles

```
EACH TEST SHOULD:
  □ Test ONE behavior (not multiple things in one test)
  □ Have a clear name: "should return 404 when user not found"
             not: "test user endpoint"
  □ Follow Arrange-Act-Assert structure:
      Arrange: set up the conditions
      Act:     perform the action being tested
      Assert:  verify the expected outcome
  □ Be independent: tests should not depend on execution order
  □ Be deterministic: same test, same result, every time

COMMON TEST DESIGN MISTAKES:
  🚩 Testing implementation details instead of behavior
     (tests break when code is refactored but behavior is the same)
  🚩 Tests that depend on each other (order-dependent test suites)
  🚩 Tests with no assertions (they always pass, even when code breaks)
  🚩 Mocking too much (test passes but real system would fail)
  🚩 Mocking too little (slow tests that hit real external services)
```

### Test Data Strategy

```
OPTIONS (choose based on your context):

STATIC FIXTURES:
  Pros: simple, fast, predictable
  Cons: can become outdated, maintenance overhead
  Use for: unit tests and simple integration tests

FACTORIES / BUILDERS:
  Create test data programmatically with sensible defaults.
  Pros: flexible, composable, easy to create variations
  Use for: integration tests with varying scenarios

DATABASE SEEDING:
  Seed a known state before each test (or test suite).
  Pros: realistic, tests actual DB behavior
  Cons: slower, requires cleanup
  Use for: integration and E2E tests

TEST DATABASE:
  Separate database for tests (never test against production).
  Reset between test runs.
  Use transaction rollback to undo changes without re-seeding.
```

---

## Phase 3: E2E Test Implementation

### What Makes a Good E2E Test

```
COVERS A REAL USER JOURNEY:
  Not: "the button exists"
  Yes: "user can click 'Add to Cart' and see item in cart"

USES PRODUCTION-LIKE DATA:
  E2E tests with toy data miss real-world edge cases.
  Use data factories that produce realistic values.

INDEPENDENT AND ISOLATED:
  Each test creates its own data, doesn't rely on other tests.
  Test cleanup is automatic (rollback, teardown, or test-specific users).

VALIDATES OUTCOMES, NOT IMPLEMENTATION:
  Check what the user sees or what the API returns.
  Not which function was called or which SQL ran.

RESILIENT TO MINOR UI CHANGES:
  Use data-testid attributes for element selection (not CSS classes or text).
  Text-based selectors break whenever copy changes.
```

### Dealing with Flaky Tests

```
COMMON CAUSES OF FLAKINESS:
  □ Timing issues: test clicks before element is ready
     Fix: wait for element state, not for time (waitForElement, not sleep)
  □ Shared state: test depends on state from another test
     Fix: make tests independent with their own setup
  □ External dependencies: test calls a real API that sometimes fails
     Fix: mock or stub external services in tests
  □ Race conditions: async operations complete in different orders
     Fix: explicit waits and proper async handling
  □ Test pollution: previous test left data that affects this test
     Fix: cleanup or isolation (transactions, unique test data)

FLAKY TEST POLICY:
  A flaky test is worse than no test — it trains the team to ignore failures.
  When a test fails intermittently:
    □ Fix it immediately, or
    □ Delete it temporarily until fixed
  Never: add retries to mask flakiness
```

---

## Phase 4: Test Maintenance

### Keeping Tests Healthy

```
REGULAR MAINTENANCE (monthly):
  □ Remove tests for deleted features
  □ Update tests when behavior legitimately changes
  □ Review test execution time — tests that take >30s need investigation
  □ Review flaky test list — each one should have an owner and timeline

TEST REVIEW IN CODE REVIEW:
  □ New code has corresponding tests
  □ Tests follow naming conventions
  □ Tests cover the happy path AND relevant error paths
  □ No hardcoded test data that will break in different environments

METRICS TO TRACK:
  □ Test suite execution time (should not grow unboundedly)
  □ Flaky test rate (how many tests fail intermittently per week)
  □ Test failure rate (separate from flakiness — are tests actually catching bugs?)
  □ Coverage trend (is coverage stable or declining as code grows?)
```

### Test in CI/CD

```
REQUIRED IN CI PIPELINE:
  □ Unit tests: run on every commit (fast — should complete in <5 minutes)
  □ Integration tests: run on every PR (medium — should complete in <15 minutes)
  □ E2E tests: run before every deployment to production (<30 minutes)

BLOCKING RULES:
  □ No deploy if unit or integration tests fail
  □ No deploy if E2E tests fail on critical journeys
  □ Flaky tests: fail the build but don't block (alert team instead)

PARALLEL EXECUTION:
  Run test suites in parallel to keep CI fast.
  Shard E2E tests across multiple runners.
  Target: total CI time < 20 minutes for most projects.
```

---

## Reference Files

- `references/test-patterns.md` — Test pattern library with examples
