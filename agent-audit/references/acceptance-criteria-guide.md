# Acceptance Criteria Quick Guide

Short reference for when to write criteria and what makes them good vs. bad.

---

## When to Write Criteria

**Before** the audit — not after. Criteria written after seeing output are influenced
by what the agent produced (anchoring bias). Write them based only on the task request.

If criteria were never defined and you're auditing after the fact:
1. Write them now based on the original request
2. Flag any criterion that might have been influenced by seeing the output
3. Mark those as "retrospective" in the report

---

## Good vs. Bad Criteria

| ❌ Bad (vague) | ✅ Good (specific) |
|---------------|-------------------|
| "Code should work" | "Function returns correct sorted list for inputs of length 0, 1, and 5+" |
| "Plan should be complete" | "Plan includes: goal, timeline, owner, success metrics, and rollback section" |
| "No bugs" | "Passes all existing tests; handles null input without throwing" |
| "Good quality" | "No hardcoded credentials; no commented-out code; all errors handled" |
| "Follows standards" | "All variable names are camelCase; no lines exceed 100 characters" |

---

## Minimum Criteria by Output Type

### Code
1. Correct output for documented inputs
2. No crash on empty/null input
3. All error paths explicitly handled
4. No secrets hardcoded
5. Tests pass (if tests were in scope)

### Plan / Document
1. All required sections present
2. No internal contradictions
3. All action items have owners and timelines (or explicitly unassigned)
4. Assumptions are stated

### Tool Call Sequence
1. Correct execution order
2. No unauthorized side effects
3. Error recovery path exists
4. No redundant calls

### Config / Data File
1. Parses without error
2. All required fields present
3. No credentials in file
4. Values within allowed ranges
