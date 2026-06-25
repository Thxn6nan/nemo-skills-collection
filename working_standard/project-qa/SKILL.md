---
name: project-qa
description: >
  Ensure project work stays in scope and matches what was agreed — before
  starting, during execution, and after delivery. Use this skill when a task
  feels bigger than it should be, when delivered work doesn't match what was
  asked for, when scope keeps expanding, or when you want to define clear
  boundaries before a task begins. Trigger phrases: "is this in scope?",
  "we're doing too much", "this isn't what was asked for", "define the scope",
  "the work has expanded beyond the original goal", "QA this task",
  "does this deliverable match the requirement?", "scope is unclear",
  "the team keeps adding things that weren't asked for".
  This skill focuses on SCOPE and REQUIREMENT FIT — not defect quality
  (work-audit) and not strategic direction (project-compass).
---

# Project QA Skill

Keeps project work aligned with what was agreed. Prevents scope creep,
catches requirement drift, and validates that deliverables match intent.

## Core Principle

> Work that is done correctly but wasn't asked for is not an asset —
> it is a cost. Scope discipline is not bureaucracy; it is respect
> for the project's priorities and the team's time.

---

## Three Modes

| Mode | When | Goal |
|------|------|------|
| **PRE** | Before starting a task | Define scope clearly so execution stays aligned |
| **MID** | During execution | Catch scope expansion before it compounds |
| **POST** | After delivery | Verify deliverable matches the original requirement |

---

## Mode 1: PRE — Define Scope Before Starting

Use when a task is ambiguous, large, or likely to expand.

### Step 1 — Extract the Requirement

From the original request, extract:
```
WHAT WAS ASKED FOR:
  Deliverable: <what the output should be>
  Format:      <how it should be structured or presented>
  Quality bar: <what "good enough" looks like>
  By when:     <if timing matters>

WHAT IS EXPLICITLY OUT OF SCOPE:
  (state even if obvious — prevents later disagreement)

WHAT IS AMBIGUOUS:
  (resolve before starting, not during)
```

### Step 2 — Define the Scope Boundary

Write one sentence that defines what this task IS and IS NOT:

```
"This task is: <specific deliverable>
 This task is not: <common scope creep risk for this type of work>"

Example:
"This task is: a written summary of the Q3 findings document.
 This task is not: an analysis, a recommendation, or a comparison to Q2."
```

### Step 3 — Issue a PRE Gate

```
SCOPE GATE: [APPROVED | NEEDS CLARIFICATION | BLOCKED]

In scope:        <list of what will be done>
Out of scope:    <explicit exclusions>
Ambiguities:     <questions that need answers before starting>
Risk of creep:   [Low | Medium | High]
Boundary statement: "<the one sentence above>"
```

If NEEDS CLARIFICATION → resolve ambiguities before proceeding.
If BLOCKED → the requirement itself needs to be reworked.

---

## Mode 2: MID — Scope Check During Execution

Use when work is underway and something feels like it's expanding.

For each action or addition under consideration, apply:

```
SCOPE CHECK:
  Action: <what is about to be done or added>

  1. Was this explicitly asked for?           YES / NO
  2. Is this required to complete what was asked? YES / NO
  3. Would skipping this cause the deliverable to fail the requirement? YES / NO

  If all three are NO → OUT OF SCOPE
  Flag it: document it as a separate suggestion, do not include in this delivery
```

**Scope Creep Signals — stop and check when you notice:**
```
🚩 "While I'm at it, I'll also..."
🚩 "This would be better if I also..."
🚩 "They'll probably want this too..."
🚩 "It's only a small addition..."
🚩 The estimate is growing significantly beyond what was planned
🚩 New dependencies are being introduced that weren't in the original plan
```

---

## Mode 3: POST — Verify Deliverable Matches Requirement

Use after a task is complete to confirm the output is what was asked for.

### Step 1 — Requirement Comparison

Map each element of the requirement to the deliverable:

```
REQUIREMENT ELEMENT       PRESENT IN DELIVERABLE?   NOTES
────────────────────────  ────────────────────────  ──────────────────
<element from request>    ✅ YES / ⚠️ PARTIAL / ❌ NO  <note if partial/no>
<element from request>    ✅ / ⚠️ / ❌
...
```

### Step 2 — Excess Check

List everything in the deliverable that was NOT in the requirement:

```
IN DELIVERABLE BUT NOT REQUESTED:
  Item:    <what's extra>
  Impact:  [Harmless | Confusing | Problematic]
  Action:  [Keep (harmless) | Flag for review | Remove]
```

### Step 3 — Scope Drift Score

```
Scope Drift = (Missing required elements + Unrequested additions) / Total elements

0–10%   → CLEAN   — deliverable matches requirement well
11–25%  → LOOSE   — minor gaps or additions; review and decide
26–50%  → DRIFTED — significant misalignment; revise before accepting
>50%    → MISS    — deliverable does not match what was asked; redo
```

### Step 4 — POST Report

```
POST SCOPE REVIEW
─────────────────────────────────────────
Original requirement: <summary>
Deliverable reviewed: <description>

Requirement coverage:
  ✅ Met:      <N> elements
  ⚠️ Partial:  <N> elements — <describe gaps>
  ❌ Missing:  <N> elements — <list what's absent>

Unrequested additions:
  <item> — [Keep | Flag | Remove]

Scope Drift Score: <X>%
Rating: [CLEAN | LOOSE | DRIFTED | MISS]

Required before acceptance:
  □ <specific fix>
  □ <specific fix>
─────────────────────────────────────────
```

---

## Reference Files

- `references/scope-boundary-guide.md` — How to write clear scope boundaries
- `references/requirement-mapping-format.md` — Detailed POST review format with examples
