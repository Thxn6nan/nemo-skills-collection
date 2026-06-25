---
name: work-audit
description: >
  Review project deliverables for defects, quality issues, and gaps before
  they are accepted, published, or handed off. Use this skill when reviewing
  any project output — code, documents, designs, data, reports, plans — to
  find problems before they cause rework downstream. Trigger phrases: "review
  this before we ship it", "audit this deliverable", "find issues in this",
  "QA this output", "something feels off about this", "check this before
  sending", "review the quality of this work", "is this good enough to ship?",
  "find defects in this code/document/design". This skill finds WHAT IS WRONG
  with what was produced.
  DISAMBIGUATION — "review this" is ambiguous; choose by what you're checking
  for: defects and quality issues in the deliverable → work-audit (this skill);
  whether the work matches the agreed scope and requirement → project-qa;
  whether the code is correctly tested and verified → testing-strategy;
  whether the plan has hidden risks or blind spots → tenth-man. When the
  request is a general "review before ship" with no other signal, default to
  work-audit for defect-finding.
---

# Work Audit Skill

Reviews deliverables for defects before they are accepted or move downstream.
A defect found before handoff costs a fraction of one found after.

## Core Principle

> The purpose of audit is not to assign blame — it is to find problems
> while they are still cheap to fix. A defect in a draft costs minutes.
> The same defect after delivery costs hours or days.

---

## Defect Taxonomy

All defects are classified into five types:

| Code | Type | Description |
|------|------|-------------|
| **L** | Logic Error | The output is wrong — incorrect reasoning, bad calculation, wrong result |
| **O** | Omission | Something required is missing |
| **R** | Regression | The work broke something that was working before |
| **S** | Standard Violation | Output doesn't match agreed conventions or requirements |
| **I** | Integrity Issue | Output is correct now but fragile — likely to break or mislead later |

---

## Audit Workflow

### Step 1 — Establish Acceptance Criteria

Before finding defects, be clear about what "correct" looks like.
If acceptance criteria don't exist → define them before auditing.

```
ACCEPTANCE CRITERIA FORMAT:
  AC-1: <what the deliverable must do or contain>
        Pass: <what passing looks like>
        Fail: <what failing looks like>
  AC-2: ...
```

### Step 2 — Select the Appropriate Checklist

Run the checklist matching the deliverable type:
See `references/audit-checklists.md`

- **Code** → correctness, error handling, security, maintainability, tests
- **Document / Report** → completeness, accuracy, consistency, clarity
- **Plan** → feasibility, completeness, dependencies, assumptions stated
- **Data / Config** → schema validity, required fields, value ranges
- **Design** → requirement coverage, edge cases, consistency

### Step 3 — Classify and Record Each Defect

```
DEFECT-N [Type Code] Short Title
  What is wrong:   <specific description>
  Where:           <file, section, line, field, or step>
  Expected:        <what it should be>
  Actual:          <what it is>
  Severity:        [Critical | High | Medium | Low]
  Fix:             <concrete corrective action>
```

**Severity guide:**
- **Critical** — Causes failure, data loss, security issue, or blocks use entirely
- **High** — Causes incorrect behavior users will encounter; must fix before shipping
- **Medium** — Incorrect in edge cases or degrades quality; fix in next revision
- **Low** — Cosmetic, inconsistent with standards, minor polish

### Step 4 — Deliver the Audit Report

```
AUDIT RESULT: [PASS | PASS WITH NOTES | FAIL]

Quality Score: 100 − (Critical×25 + High×10 + Medium×3 + Low×1)

Defects:
  Critical: <N>  → must fix before shipping
  High:     <N>  → must fix before shipping
  Medium:   <N>  → fix in next revision
  Low:      <N>  → optional polish

Required fixes: <list Critical + High defects>
```

**Result rules:**
- Any Critical → FAIL
- Any High → FAIL
- Medium only → PASS WITH NOTES
- Low only → PASS
- No defects → PASS (document as clean)

---

## Severity → Action

| Finding | Required Action |
|---------|----------------|
| Critical | Stop. Fix before any further use or distribution |
| High | Fix before this deliverable is accepted or shipped |
| Medium | Log and fix in next revision; does not block acceptance |
| Low | Fix if time allows; no block |

---

## Reference Files

- `references/audit-checklists.md` — Checklists by deliverable type
- `references/audit-report-format.md` — Full report template with examples
