# Scope Boundary Guide & Requirement Mapping Format

---

## Part 1: Writing Clear Scope Boundaries

A scope boundary is the one sentence that ends disagreements about what is and
isn't part of a task. Write it before starting, not during.

### Good vs Bad Boundaries

```
❌ BAD (vague):
"Write a report on the user research findings."

Vague because: What kind of report? How long? Which findings? Recommendations included?

✅ GOOD (specific):
"This task is: a 1-2 page written summary of the 5 key themes from the
 user research sessions, in prose format.
 This task is not: quantitative analysis, recommendations, or comparison
 to previous research."
```

### Boundary Template

```
"This task is: [deliverable] + [format/length] + [from/about what source or input]
 This task is not: [most likely scope creep risk] + [related thing not included]"
```

### Common Scope Creep Risks by Task Type

```
TASK TYPE           COMMON CREEP                 HOW TO EXPLICITLY EXCLUDE
────────────────    ─────────────────────────    ──────────────────────────
Summarize X         Adding analysis/opinions      "summary only, no recommendations"
Fix bug Y           Refactoring related code      "fix Y only; no changes to other code"
Write feature Z     Adding related features       "Z only; related ideas go in backlog"
Review document     Rewriting the document        "comments and suggestions, not rewrites"
Research topic A    Solving the problem found     "research and synthesis only, not solution"
Create plan         Executing the plan            "plan document only, not implementation"
```

---

## Part 2: Requirement Mapping — Full POST Format

Use this when doing a thorough POST review of a completed deliverable.

```
╔══════════════════════════════════════════════════════════════╗
║            PROJECT QA — POST SCOPE REVIEW                    ║
╚══════════════════════════════════════════════════════════════╝

TASK:             <what was requested>
DELIVERABLE:      <what was produced>
REVIEW DATE:      <date>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
REQUIREMENT COVERAGE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  #   Requirement Element              Status      Notes
  ─   ─────────────────────────────   ─────────   ──────────────────
  1   <element>                        ✅ Met
  2   <element>                        ⚠️ Partial   <what's missing>
  3   <element>                        ❌ Missing   <why it matters>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
UNREQUESTED ADDITIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Item                    Impact        Decision
  ──────────────────────  ────────────  ─────────────────────
  <addition>              Harmless      Keep — adds value, no confusion
  <addition>              Confusing     Flag — may mislead receiver
  <addition>              Problematic   Remove — changes scope/expectation

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SCOPE DRIFT SCORE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Required elements:       <N total>
  Met:                     <N>  ✅
  Partial:                 <N>  ⚠️
  Missing:                 <N>  ❌
  Unrequested additions:   <N>

  Drift = (Missing + Unrequested) / (Total required + Unrequested) × 100 = <X>%

  Rating: [CLEAN 0-10% | LOOSE 11-25% | DRIFTED 26-50% | MISS >50%]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
REQUIRED BEFORE ACCEPTANCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  □ <specific fix needed>
  □ <specific fix needed>

  (empty = CLEAN, accept as-is)

╔══════════════════════════════════════════════════════════════╗
║  VERDICT: [ACCEPTED | REVISE | REDO]                         ║
╚══════════════════════════════════════════════════════════════╝
```

### Worked Example

```
TASK:        Write a 1-page brief summarising the competitor analysis
DELIVERABLE: 3-page document with summary, analysis, and recommendations
REVIEW DATE: 2025-08-14

REQUIREMENT COVERAGE:
  1   Summary of competitor findings     ✅ Met
  2   1-page length                      ❌ Missing   Document is 3 pages

UNREQUESTED ADDITIONS:
  Analysis section          Confusing    Flag — was explicitly out of scope
  Recommendations section   Problematic  Remove — changes nature of deliverable

DRIFT SCORE: (1 missing + 2 unrequested) / (2 required + 2 unrequested) × 100 = 75%
Rating: MISS

REQUIRED BEFORE ACCEPTANCE:
  □ Trim to 1 page (summary only)
  □ Remove analysis and recommendations sections
    (log them as separate work items if stakeholder wants them)

VERDICT: REDO
```
