# Audit Checklists & Report Format

---

## Checklist: Code

```
CORRECTNESS
□ Output matches the stated requirement
□ Logic handles the primary use case correctly
□ Return values and types match what callers expect
□ No off-by-one errors, incorrect conditions, or wrong operators

EDGE CASES
□ Handles empty / null / zero inputs
□ Handles maximum and minimum values
□ Handles unexpected input types gracefully (error, not crash)
□ Concurrent access considered if applicable

ERROR HANDLING
□ All failure paths are handled explicitly (no silent failures)
□ Error messages are informative
□ Resources released on failure (files closed, connections returned)
□ No swallowed exceptions (bare except/catch with no action)

SECURITY
□ No credentials or secrets hardcoded
□ User input validated before use
□ No SQL injection vectors
□ Sensitive data not logged in plaintext

MAINTAINABILITY
□ No leftover debug code (print, console.log, debugger)
□ No commented-out code blocks
□ No magic numbers (should be named constants)
□ Names reveal intent

TESTS (if in scope)
□ Tests include assertions (not just "runs without error")
□ Tests cover failure cases, not only happy path
□ Test names describe what they verify
```

---

## Checklist: Document / Report

```
COMPLETENESS
□ All required sections present
□ No section left as placeholder or TBD without explanation
□ All claims are supported or flagged as estimates

ACCURACY
□ Factual claims are correct (verify surprising ones)
□ Numbers are consistent throughout (same figure not stated differently in two places)
□ Dates, names, and references are accurate

INTERNAL CONSISTENCY
□ Conclusions follow from the evidence presented
□ Section 3 does not contradict Section 1
□ Recommendations are consistent with the findings

CLARITY
□ The intended audience can understand this without additional context
□ Jargon is defined or avoided
□ The main point of each section is clear from the first paragraph

ACTIONABILITY
□ Recommendations are specific, not vague
□ Next steps have owners or clear directions
□ Assumptions are stated explicitly
```

---

## Checklist: Plan

```
FEASIBILITY
□ Tasks are achievable with available resources
□ Timeline is realistic (not optimistic)
□ Dependencies are identified and accounted for

COMPLETENESS
□ All major workstreams are covered
□ Handoffs between tasks or people are defined
□ Success criteria are defined for the plan overall

RISK
□ Key risks are identified
□ Mitigation or response for each key risk is stated
□ Assumptions underlying the plan are made explicit

CLARITY
□ Each task has a clear owner
□ Each task has a clear definition of done
□ The plan can be executed without asking clarifying questions
```

---

## Checklist: Data / Config

```
VALIDITY
□ File parses without errors (JSON valid, YAML valid, CSV consistent)
□ All required fields present
□ No extra fields that would be rejected by the consumer
□ Field names match expected schema exactly

VALUES
□ Values within allowed ranges
□ Enum values match exactly what the consumer accepts
□ No credentials or secrets in the file
□ Numbers are numbers, strings are strings (not accidentally swapped)

FORMAT
□ Encoding is correct (UTF-8 unless otherwise required)
□ Line endings consistent
□ Timestamps in expected format and timezone
```

---

## Full Audit Report Template

```
╔══════════════════════════════════════════════════════════════╗
║                   WORK AUDIT REPORT                          ║
╚══════════════════════════════════════════════════════════════╝

DELIVERABLE:  <what is being audited>
TYPE:         [Code | Document | Plan | Data | Design | Mixed]
DATE:         <ISO date>

ACCEPTANCE CRITERIA:
  AC-1: <criterion> — Pass: <condition> / Fail: <condition>
  AC-2: ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DEFECTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DEFECT-1 [L] <Short Title>
  What is wrong:  <specific description>
  Where:          <location>
  Expected:       <what it should be>
  Actual:         <what it is>
  Severity:       Critical
  Fix:            <concrete action>

DEFECT-2 [S] <Short Title>
  What is wrong:  ...
  Severity:       High
  Fix:            ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Critical: <N>    High: <N>    Medium: <N>    Low: <N>

  Quality Score: 100 − (Critical×25 + High×10 + Medium×3 + Low×1) = <X>/100

  Criteria met:    AC-1 ✅  AC-2 ❌
  Criteria failed: AC-2 — <reason>

  Required before acceptance:
    □ DEFECT-1 — <title>
    □ DEFECT-2 — <title>

╔══════════════════════════════════════════════════════════════╗
║  RESULT: [PASS | PASS WITH NOTES | FAIL]                     ║
║  Quality Score: <X>/100                                      ║
╚══════════════════════════════════════════════════════════════╝
```
