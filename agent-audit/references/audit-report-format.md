# Audit Report Format & Acceptance Criteria Guide

---

## Part 1: Full Audit Report Template

```
╔═══════════════════════════════════════════════════════╗
║           AGENT AUDIT REPORT                          ║
╚═══════════════════════════════════════════════════════╝

TASK:         "<what the agent was asked to do>"
OUTPUT TYPE:  [Code | Plan | Tool Sequence | Config | Data | Mixed]
AUDITED BY:   agent-audit skill
DATE:         <ISO date>

ACCEPTANCE CRITERIA:
  1. <criterion>
  2. <criterion>
  (See Part 2 of this document if criteria weren't defined beforehand)

───────────────────────────────────────────────────────
DEFECTS FOUND
───────────────────────────────────────────────────────

DEFECT-1 [L] Logic Error — <Short Title>
  Description:   <what is wrong>
  Location:      <file:line / section / step>
  Expected:      <what it should be>
  Actual:        <what it is>
  Severity:      Critical
  Reproducible:  Always
  Fix:           <concrete corrective action>

DEFECT-2 [O] Omission — <Short Title>
  Description:   <what is missing>
  Location:      <where it should appear>
  Expected:      <what should be there>
  Actual:        <what's there instead — or nothing>
  Severity:      High
  Reproducible:  Always
  Fix:           <what needs to be added>

DEFECT-3 [S] Standard Violation — <Short Title>
  Description:   <which standard was violated>
  Location:      <where>
  Expected:      <standard requirement>
  Actual:        <what was produced>
  Severity:      Medium
  Reproducible:  Always
  Fix:           <how to conform>

───────────────────────────────────────────────────────
DEFECT SUMMARY
───────────────────────────────────────────────────────

  Type breakdown:
    Logic Errors (L):        <N>
    Omissions (O):           <N>
    Regressions (R):         <N>
    Standard Violations (S): <N>
    Reliability Issues (I):  <N>

  Severity breakdown:
    Critical: <N>
    High:     <N>
    Medium:   <N>
    Low:      <N>

  Quality Score: <X>/100
  Formula: 100 − (Critical×25 + High×10 + Medium×3 + Low×1), minimum 0

───────────────────────────────────────────────────────
AUDIT RESULT
───────────────────────────────────────────────────────

  RESULT: [PASS | PASS WITH NOTES | FAIL]

  Criteria met:   <list>
  Criteria failed: <list>

  Required fixes before acceptance:
    □ DEFECT-N — <title>
    □ DEFECT-N — <title>

  Optional improvements (not blocking):
    □ DEFECT-N — <title>

╔═══════════════════════════════════════════════════════╗
║  VERDICT: [PASS | PASS WITH NOTES | FAIL]             ║
╚═══════════════════════════════════════════════════════╝
```

---

## Part 2: Acceptance Criteria Guide

Write acceptance criteria BEFORE auditing. Criteria written after seeing the output
are biased by what the agent actually produced.

### Format for Each Criterion

```
AC-N: <criterion statement>
  Verification method: <how to check this — manual review / run test / parse output>
  Pass condition:      <exactly what constitutes passing>
  Fail condition:      <exactly what constitutes failing>
```

### Criterion Types

**Functional** — Does it do what was asked?
```
AC-1: The function returns the sorted list in ascending order
  Verification: Run with input [3,1,2], [],[1],[1,1]
  Pass: Returns [1,2,3], [], [1], [1,1] respectively
  Fail: Any other return value or thrown exception
```

**Completeness** — Is everything required present?
```
AC-2: The plan includes a rollback section
  Verification: Manual review of document sections
  Pass: Section exists and contains ≥1 concrete rollback step
  Fail: Section missing or only says "rollback if needed" with no steps
```

**Standard compliance** — Does it match our conventions?
```
AC-3: All environment variables use SCREAMING_SNAKE_CASE
  Verification: Search for env var references in output
  Pass: All matches are uppercase with underscores
  Fail: Any camelCase, kebab-case, or lowercase env var name
```

**Non-regression** — Did it break anything?
```
AC-4: Existing tests still pass after the change
  Verification: Run test suite
  Pass: All previously-passing tests still pass
  Fail: Any test that was passing before the change now fails
```

**Safety** — Are dangerous things absent?
```
AC-5: No credentials in the output file
  Verification: Search for patterns matching API keys, passwords, tokens
  Pass: No matches found
  Fail: Any credential-like string found in the file
```

---

## Quality Score Interpretation

| Score | Rating | Meaning |
|-------|--------|---------|
| 90–100 | Excellent | Ship it. Minor polish at most. |
| 75–89 | Good | Fix High defects, then ship. |
| 50–74 | Acceptable | Fix Critical + High. Review Medium. |
| 25–49 | Poor | Significant rework needed. |
| 0–24 | Unacceptable | Reject. Agent did not produce usable output. |

---

## Worked Example — Short Audit

Task: "Write a Python function that validates an email address"

```
DEFECT-1 [L] No check for domain part
  Description:   Function accepts "user@" as valid (missing domain check)
  Location:      email_validator.py:12, regex pattern
  Expected:      Pattern requires at least one character after @, a dot, and TLD
  Actual:        Pattern only checks for presence of @
  Severity:      High
  Reproducible:  Always — input "user@" returns True
  Fix:           Update regex to: r'^[^@]+@[^@]+\.[^@]{2,}$' or use email-validator library

DEFECT-2 [O] No tests written
  Description:   Task said "write the function" — convention requires unit tests
  Location:      Missing file: test_email_validator.py
  Expected:      At minimum: valid email passes, invalid formats fail, edge cases covered
  Actual:        No test file produced
  Severity:      Medium
  Reproducible:  Always
  Fix:           Create test_email_validator.py with ≥5 test cases

Quality Score: 100 − (0×25 + 1×10 + 1×3 + 0×1) = 87/100
RESULT: PASS WITH NOTES — Fix DEFECT-1 before use in production
```
