---
name: testing-strategy
description: >
  Plan and implement a complete testing approach — unit tests, coverage analysis,
  verification, and validation. Use this skill when writing tests for new code,
  when coverage is insufficient, when tests don't catch real bugs, when validating
  that work meets requirements, or when setting up a testing strategy for a project.
  Trigger phrases: "write unit tests", "test this code", "improve test coverage",
  "coverage analysis", "verify this works", "validate this meets requirements",
  "testing strategy", "how do we test this?", "tests aren't catching bugs",
  "what should we test?", "is this correct?", "does this do what was asked?".
  Covers V&V — Verification ("built it right") and Validation ("built the right thing").
  DISAMBIGUATION — choose by test level: unit tests, coverage analysis, and
  verification/validation of individual components → testing-strategy (this skill);
  full system flows from user input to output, integration test strategy →
  e2e-testing; finding defects in an already-produced deliverable → work-audit.
  Use testing-strategy and e2e-testing together: unit/coverage first, then E2E.
---

# Testing Strategy Skill

Answers two questions that every piece of software must answer before shipping:

```
VERIFICATION → "Are we building the thing right?"
               Is the code technically correct? Does it do what it's supposed to?
               Tools: unit tests, integration tests, code review, static analysis

VALIDATION   → "Are we building the right thing?"
               Does what we built actually meet the real need?
               Tools: acceptance criteria, user testing, requirement tracing
```

These are different questions. A system can pass all tests and still be wrong
(built the wrong thing correctly). A system can meet user needs while having
bugs in parts that don't affect those needs.

## Core Principle

> Tests are not about coverage numbers — they are about confidence.
> The question is not "what percentage of lines are covered?"
> The question is "can I change this code without fear?"
> Good tests answer that question. Coverage numbers are just a proxy.

---

## The Testing Hierarchy

```
                    /\
                   /V&V\          ← Validation: does it meet the real need?
                  /──────\
                 / Accept-\       ← Acceptance tests: meets stated requirements?
                /  ance    \
               /────────────\
              /  Integration  \   ← Do components work together?
             /────────────────\
            /    Unit Tests     \ ← Does each piece work in isolation?
           /────────────────────\
          /  Static Analysis /    \ ← Does the code even make sense?
         /  Type Checking         \
        /────────────────────────────\
```

Build from the bottom up. Don't write acceptance tests before unit tests pass.

---

## Phase 1: Test Planning

Before writing any test, decide what to test and why.

### Risk-Based Test Planning

```
NOT all code deserves equal testing effort.
Focus effort where failure has the highest cost.

HIGH RISK — test thoroughly:
  □ Code that handles money, permissions, or sensitive data
  □ Core business logic that is central to the product's value
  □ Code with complex conditions (many branches, edge cases)
  □ Code that has failed before (regression prevention)
  □ Code that other code depends on heavily (high fan-in)

MEDIUM RISK — test the important paths:
  □ API endpoints and public interfaces
  □ Data transformations and mappings
  □ Integration points with external systems

LOW RISK — test lightly or skip:
  □ Simple getters/setters with no logic
  □ Framework-generated code
  □ Configuration reading (test the config, not the reading)
  □ Third-party library behavior (test your use of it, not the library)
```

### Defining What "Done Testing" Means

```
Before writing tests, define completion criteria:

  □ Which behaviors must have unit tests?
  □ Which code paths must be covered?
  □ What are the critical edge cases for this feature?
  □ What failure modes must be tested?
  □ What acceptance criteria must pass?

"Done testing" = all of the above, not a coverage number.
```

---

## Phase 2: Unit Testing

Unit tests verify individual functions, methods, or classes in isolation.
See `references/unit-testing-guide.md` for full methodology.

### What Makes a Good Unit Test

```
ISOLATED:
  Tests one thing. External dependencies are replaced with test doubles.
  If a unit test fails, you know exactly where the problem is.

FAST:
  Unit tests run in milliseconds. If a test takes seconds, it's not a unit test.
  Fast tests are run constantly — slow tests are avoided.

DETERMINISTIC:
  Same code, same result, every run.
  No randomness. No reliance on time. No shared mutable state.

CLEAR:
  The test name describes what it tests and what the expected behavior is.
  A failing test tells you exactly what broke.
  
INDEPENDENT:
  Tests can run in any order.
  Tests do not depend on other tests having run first.
```

### The AAA Structure

Every unit test follows Arrange → Act → Assert:

```python
def test_calculate_order_total_applies_discount_for_premium_users():
    # ARRANGE — set up the conditions
    user = User(tier="premium")
    items = [Item(price=100), Item(price=50)]
    
    # ACT — perform the action being tested
    total = calculate_order_total(user, items)
    
    # ASSERT — verify the expected outcome
    assert total == 135  # 150 * 0.90 (10% premium discount)
```

### Test Doubles — When to Use Each

```
STUB:    Returns a fixed value. Use when you need a dependency to return
         something but don't care how it's called.
         "Always return user_id=42 from the auth service"

MOCK:    Verifies the dependency was called correctly. Use when the test
         cares about the interaction, not just the outcome.
         "Assert that send_email() was called exactly once with this address"

FAKE:    A working but simplified implementation. Use for things like
         in-memory databases during testing.
         "Use a dict-based cache instead of Redis"

SPY:     Like a mock, but wraps the real implementation. Use when you
         want real behavior but also want to verify calls.

RULE:    Mock at the boundary of your system (external services, I/O).
         Don't mock your own code — that tests the mocks, not the code.
```

---

## Phase 3: Coverage Analysis

Coverage is a tool for finding untested paths, not a target to hit.

### Coverage Types

```
LINE COVERAGE (most common, least meaningful):
  % of lines executed during tests.
  Problem: a line can be executed without testing all its behaviors.
  Useful for: finding completely untested code.

BRANCH COVERAGE (more meaningful):
  % of branches (if/else paths) executed.
  Catches: "the else branch was never tested"
  Useful for: finding untested conditions.

MUTATION COVERAGE (most meaningful, most expensive):
  Tests are run against modified ("mutated") versions of the code.
  If tests still pass when code is mutated → tests aren't actually verifying behavior.
  Useful for: finding tests that pass but don't catch real bugs.
```

### Coverage Analysis Workflow

```
STEP 1 — Run coverage and find gaps
  Generate a coverage report.
  Filter to code with HIGH RISK (business logic, security, data handling).
  Ignore LOW RISK code (framework boilerplate, config reading).

STEP 2 — Interpret gaps
  Uncovered line in high-risk code → write a test
  Uncovered branch in high-risk code → write a test for that path
  Uncovered code in low-risk area → decide consciously to skip

STEP 3 — Don't chase the number
  Going from 85% to 90% by adding tests for trivial getters
  is less valuable than going from 60% to 70% in the business logic.

  Ask: "Is the uncovered code risky enough to test?"
  YES → write the test
  NO  → mark as excluded from coverage target; document why
```

### Coverage Targets (by risk level, not overall)

```
HIGH-RISK CODE:      ≥ 90% branch coverage
  Business logic, financial calculations, auth/authz, data transforms

MEDIUM-RISK CODE:    ≥ 70% branch coverage
  API handlers, integration points, configuration processing

LOW-RISK CODE:       No target
  Simple getters/setters, framework glue, configuration reading

OVERALL PROJECT:     Don't use as primary metric
  An overall project target hides where the gaps are.
  Prefer: "high-risk code is 90%+ covered" over "project is 80% covered"
```

---

## Phase 4: Verification

Verification confirms the implementation is technically correct.

```
VERIFICATION CHECKLIST:
  □ All unit tests pass
  □ All integration tests pass
  □ Branch coverage meets targets for high-risk code
  □ No regression in previously-passing tests
  □ Static analysis / linting passes (no type errors, no obvious bugs)
  □ Code review completed
  □ Performance meets requirements (where applicable)
  □ Error handling tested (not just happy path)

VERIFICATION IS NOT COMPLETE IF:
  □ Tests were written to match buggy implementation (tests should
    be written against the specification, not the code)
  □ Coverage is high but tests have no assertions
  □ Edge cases were identified but not tested
  □ Error paths are untested
```

---

## Phase 5: Validation

Validation confirms the implementation meets the actual need.

```
VALIDATION CHECKLIST:
  □ Acceptance criteria defined before implementation → check each one
  □ Happy path works end-to-end (not just unit level)
  □ The feature solves the original user problem (not just the literal spec)
  □ Edge cases that real users will encounter are handled
  □ Output is in the expected format for consumers

VALIDATION METHODS:
  Acceptance tests:
    Run the scenarios defined in the requirement.
    Each acceptance criterion has at least one test.

  Requirement tracing:
    For each requirement element, identify which test covers it.
    Missing trace = missing coverage of the requirement.

  Exploratory testing:
    Try to break it in ways the requirements didn't anticipate.
    Think like a user who doesn't read the docs.

VALIDATION IS NOT COMPLETE IF:
  □ Acceptance criteria were never written (can't validate against nothing)
  □ Tests only cover the happy path the developer envisioned
  □ The original problem statement hasn't been re-read before declaring done
  □ "Validated" based on code review only (not actual execution)
```

---

## V&V Report

Use this after completing testing to document readiness:

```
V&V REPORT
══════════════════════════════════════════════════
Feature / Component: <name>
Date: <date>

VERIFICATION STATUS
  Unit tests:          <N> passing / <N> total
  Integration tests:   <N> passing / <N> total
  Branch coverage (high-risk code): <X>%
  Static analysis:     [PASS | WARNINGS | FAIL]
  Regressions:         [NONE | <describe>]
  
  Verification: [PASS | FAIL — <what needs fixing>]

VALIDATION STATUS
  Acceptance criteria: <N> of <N> met
  Failed criteria:     <list any not met>
  Edge cases tested:   <describe what was tried>
  
  Validation: [PASS | FAIL — <what needs fixing>]

KNOWN GAPS
  <anything deliberately not tested and why>

OVERALL: [READY | NOT READY — <what must be resolved>]
══════════════════════════════════════════════════
```

---

## How This Skill Connects to Others

```
work-audit          → finds defects in deliverables (what's wrong)
testing-strategy    → verifies correctness + validates fit (is it right?)
e2e-testing         → tests the full system flow end-to-end
anti-hallucination  → verifies data and numbers specifically
project-qa          → validates scope alignment (in scope? matches requirement?)
```

---

## Reference Files

- `references/unit-testing-guide.md` — Unit test methodology, patterns, anti-patterns
- `references/coverage-analysis-guide.md` — Coverage interpretation and analysis workflow
