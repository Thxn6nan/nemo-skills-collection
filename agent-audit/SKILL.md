---
name: agent-audit
description: >
  Audit agent outputs for defects, bugs, quality issues, and standard violations.
  Use this skill whenever you need to review what an agent produced — code, plans,
  documents, tool call sequences — and find problems before they reach production
  or cause downstream failures. Trigger when the user says "audit this output",
  "find bugs in what the agent did", "something feels off", "QA the agent's work",
  "review agent output quality", or "the agent produced something unexpected".
  Also trigger proactively after any high-stakes agent task (database migrations,
  API integrations, file system changes, deployments) even if not explicitly asked.
  This skill focuses on WHAT was produced (quality, correctness, defects),
  while agent-qa focuses on WHETHER work was in scope. Use both together for
  full coverage of agent behavior.
---

# Agent Audit Skill

Finds defects, inconsistencies, and quality violations in agent-produced work.
Agents are non-deterministic — the same prompt can yield outputs ranging from
excellent to subtly broken. This skill provides a systematic audit framework
to catch problems before they cause real damage.

## Core Principle

Agent outputs must be **verified, not trusted**.  
An output that *looks* correct is not the same as an output that *is* correct.  
Audit is not optional on high-stakes work — it is part of the definition of done.

---

## Defect Taxonomy

All findings are classified into one of five types:

| Type | Code | Description |
|------|------|-------------|
| **Logic Error** | `L` | Output is wrong — incorrect reasoning, bad computation, wrong result |
| **Omission** | `O` | Something required is missing from the output |
| **Regression** | `R` | Agent's work broke something that was working before |
| **Standard Violation** | `S` | Output doesn't match defined conventions, formats, or rules |
| **Reliability Issue** | `I` | Output is correct now but brittle — likely to break under change |

---

## Audit Workflow

### Step 1 — Define the Expected Output

Before finding defects, establish what "correct" looks like:
- What was the task?
- What does a passing output include/exclude?
- Are there explicit standards to check against? (coding style, API contracts, schema)
- What are the acceptance criteria?

If acceptance criteria don't exist → write them before auditing.
See `references/acceptance-criteria-guide.md`.

### Step 2 — Run the Defect Checklist

For each category relevant to the output type, run the appropriate checklist.
See `references/defect-checklists.md` for full checklists per output type:

- **Code output** → Logic, edge cases, error handling, security, tests
- **Plan/document output** → Completeness, accuracy, internal consistency, gaps
- **Tool call sequence** → Ordering, idempotency, error recovery, side effects
- **Data/config output** → Schema validity, required fields, value ranges, encoding

### Step 3 — Classify and Rate Each Defect

```
DEFECT-N [Type Code] <Short Title>
  Description:   <what is wrong>
  Location:      <file:line, section, step number, or field name>
  Expected:      <what it should be>
  Actual:        <what it is>
  Severity:      [Critical | High | Medium | Low]
  Reproducible:  [Always | Sometimes | Unknown]
  Fix:           <concrete corrective action>
```

Severity guide:
- **Critical** — Causes data loss, security breach, or system failure
- **High** — Causes incorrect behavior that users will encounter
- **Medium** — Causes incorrect behavior in edge cases or degrades quality
- **Low** — Cosmetic, inconsistent with standards, or minor quality issue

### Step 4 — Deliver the Audit Report

See `references/audit-report-format.md` for full format.

```
AUDIT RESULT: [PASS | PASS WITH NOTES | FAIL]

Defects found:
  Critical: <N>
  High:     <N>
  Medium:   <N>
  Low:      <N>

Quality Score: <X>/100
(= 100 − (Critical×25 + High×10 + Medium×3 + Low×1), min 0)

Required fixes before acceptance: <list Critical + High defects>
```

---

## Severity → Action Matrix

| Finding | Action |
|---------|--------|
| Any Critical | FAIL — must fix before proceeding |
| 1+ High | FAIL — fix before merge/deploy |
| Medium only | PASS WITH NOTES — fix in follow-up |
| Low only | PASS — log for improvement |
| No defects | PASS — document as clean |

---

## Consistency Audit (Multi-Run)

When an agent runs the same task multiple times, audit for consistency:

```
Run A output: <summary>
Run B output: <summary>
Run C output: <summary>

Divergences found:
  - <what differs between runs>
  - <which divergence is a defect vs. acceptable variation>

Consistency Rating: [Deterministic | Stable | Variable | Unreliable]
```

- **Deterministic** — Identical output every run (ideal for data transforms)
- **Stable** — Minor surface variation, same substance (acceptable for prose/plans)
- **Variable** — Meaningful differences between runs (investigate root cause)
- **Unreliable** — Contradictory outputs; agent cannot be trusted for this task

---

## Audit Triggers by Output Type

### Code
- Runs without errors?
- Handles null / empty / boundary inputs?
- All error paths covered?
- No hardcoded values that should be configurable?
- Tests written and passing?
- No leftover debug code or TODOs that should be resolved?

### Plans / Documents
- Every section present and complete?
- No internal contradictions?
- Claims backed by evidence or reasoning?
- Action items are concrete and assignable?
- Assumptions stated explicitly?

### Tool Call Sequences
- Calls in correct order?
- No duplicate or redundant calls?
- Error handling between steps?
- Idempotent where required?
- Writes before reads where data is needed?

### Config / Data Files
- Valid schema?
- All required fields present?
- No values outside allowed ranges?
- Secrets not hardcoded?
- Encoding correct (UTF-8, JSON valid, YAML parseable)?

---

## Reference Files

- `references/defect-checklists.md` — Detailed checklists per output type
- `references/audit-report-format.md` — Full report template with examples
- `references/acceptance-criteria-guide.md` — How to write acceptance criteria

---

## Example Invocations

```
"Audit the code the agent just wrote"
"Find bugs in this agent output before I merge it"
"Something feels off about what the agent produced — check it"
"QA the migration script the agent generated"
"The agent's plan has inconsistencies — find them"
"Run a defect check on this config file the agent created"
"Agent gave different answers twice — which one is correct?"
```
