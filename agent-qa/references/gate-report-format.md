# PRE Mode: Gate Report Format

Use this template when reviewing an agent task plan BEFORE execution.

---

## Full Gate Report Template

```
═══════════════════════════════════════
AGENT QA — TASK GATE REPORT
═══════════════════════════════════════

ORIGINAL REQUEST:
  "<exact user request, verbatim or as close as possible>"

MINIMUM DELIVERABLE:
  "<one sentence: what does 'done' look like?>"

SCOPE BOUNDARY:
  IN:  <what the agent IS authorized to do>
  OUT: <what the agent is NOT authorized to do, even if helpful>

───────────────────────────────────────
PLANNED ACTIONS REVIEW
───────────────────────────────────────

✅ APPROVED
  1. <action> — <reason it's in scope>
  2. <action> — <reason it's in scope>

⚠️ FLAGGED (needs clarification before proceeding)
  1. <action>
     Reason: <why this is questionable>
     Question for user: <what needs to be confirmed>

❌ BLOCKED (must be removed from plan)
  1. <action>
     Reason: <why this is out of scope or harmful>
     Alternative: <what to do instead, if anything>

───────────────────────────────────────
GATE VERDICT
───────────────────────────────────────

GATE: [APPROVED | TRIMMED | BLOCKED]

  APPROVED  → All actions are in scope. Agent may proceed.
  TRIMMED   → Flagged/blocked actions removed. Revised plan ready.
  BLOCKED   → Task plan cannot proceed as written. Needs redesign.

Risk of scope creep (if approved): [Low | Medium | High]
Recommended constraint to add: "<specific guardrail wording>"
═══════════════════════════════════════
```

---

## Filled Example

```
═══════════════════════════════════════
AGENT QA — TASK GATE REPORT
═══════════════════════════════════════

ORIGINAL REQUEST:
  "Fix the broken unit test in auth/login_test.py"

MINIMUM DELIVERABLE:
  "The failing test in auth/login_test.py passes without breaking other tests."

SCOPE BOUNDARY:
  IN:  Edit auth/login_test.py and auth/login.py if the test logic requires it
  OUT: Refactoring unrelated code, changing other test files, updating dependencies,
       modifying config files, "improving" code not related to the failing test

───────────────────────────────────────
PLANNED ACTIONS REVIEW
───────────────────────────────────────

✅ APPROVED
  1. Read auth/login_test.py — required to understand the failing test
  2. Read auth/login.py — required to understand what's being tested
  3. Edit the test assertion that's incorrect — directly fixes the problem

⚠️ FLAGGED
  1. Run full test suite after fix
     Reason: Useful but may be out of scope if slow; confirm user wants this
     Question: Should I run all tests or just auth/ tests to verify no regression?

❌ BLOCKED
  1. "Refactor login.py to use the new auth pattern while I'm here"
     Reason: Not requested; changes untested code; introduces scope creep
     Alternative: Log as a separate suggestion, do not implement
  2. Update requirements.txt to latest versions
     Reason: Completely unrelated to the failing test; high risk of breaking things
     Alternative: Skip entirely

───────────────────────────────────────
GATE VERDICT
───────────────────────────────────────

GATE: TRIMMED

Risk of scope creep (if approved): Low (after removing blocked items)
Recommended constraint to add:
  "Only modify files in auth/ directory. Do not refactor or update dependencies."
═══════════════════════════════════════
```

---

## Scope Boundary Wording Guide

Good scope boundaries are:
- **Specific** about files/directories: "Only edit files in src/components/auth/"
- **Action-typed**: "Read and edit only; do not delete or rename"
- **Explicit about exclusions**: "Do not touch tests, configs, or package files"
- **Time-bounded if relevant**: "Fix only the current failing test, not future-proofing"

Avoid vague boundaries like "stay on task" or "be focused" — these don't constrain agents.
