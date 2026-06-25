---
name: debugging-rootcause
description: >
  Find why something isn't working — the real cause, not just the symptom.
  Use this skill when a bug, failure, or unexpected behavior needs to be
  diagnosed before it can be fixed. Covers hypothesis-driven debugging,
  evidence gathering, root cause isolation, and verification. Trigger phrases:
  "why isn't this working?", "find the root cause", "something is broken but
  I don't know why", "debug this", "the error doesn't make sense", "it works
  locally but not in production", "intermittent failure", "trace this bug",
  "what's causing this?". This skill finds WHY — work-audit finds WHAT IS WRONG,
  project-monitor detects THAT something is wrong.
  DISAMBIGUATION — use a different skill instead when: the problem is in
  production NOW with users affected, needing coordinated crisis response
  (use incident-response; it calls this skill for its diagnosis step); the
  problem is specifically slowness or capacity under load (use
  performance-scaling); reviewing not-yet-broken work for latent defects
  (use work-audit). Choose debugging-rootcause to find the cause of a
  specific known failure — not to coordinate a crisis or audit for unknowns.
---

# Debugging & Root Cause Skill

Finds the actual cause of a problem — not the first plausible explanation,
and not just the surface symptom. A fix that doesn't address the root cause
creates the illusion of progress while the real problem remains.

## Core Principle

> A symptom points toward the problem. The first explanation that fits
> the symptom is almost never the root cause. Debug evidence, not intuition.
> Never fix until you can explain exactly why the fix will work.

---

## The Debugging Contract

Before starting, establish:
```
□ SYMPTOM:     What is the observable wrong behavior? (precise — not "it's broken")
□ EXPECTED:    What should happen instead?
□ CONTEXT:     When does it happen? Always? Intermittently? Under what conditions?
□ CHANGED:     What changed recently? (deployment, data, config, environment)
□ TRIED:       What has already been attempted? (avoid repeating work)
```

If CHANGED can be answered → start there. Most bugs have a recent cause.

---

## Debugging Workflow

### Phase 1 — Reproduce the Problem

**A bug you cannot reproduce reliably cannot be debugged reliably.**

```
Step 1: Can you reproduce it on demand?
  YES → proceed to Phase 2
  NO  → find the reproduction condition first

Finding reproduction conditions:
  □ Which environments does it occur in? (prod only? staging too? local?)
  □ Which users/inputs trigger it? (all users? specific data? specific sequence?)
  □ Is it time-dependent? (only at peak load? after X hours uptime? certain times?)
  □ Is it order-dependent? (only after doing Y first?)

Minimum reproducible case:
  Strip away everything not needed to trigger the bug.
  The simpler the reproduction, the easier the diagnosis.
  If you cannot reduce it — document the full context required.
```

### Phase 2 — Gather Evidence

**Do not form a hypothesis before gathering evidence. Evidence first.**

```
PRIMARY EVIDENCE SOURCES:
  □ Error messages — read them fully, not just the first line
  □ Stack traces — find the frame that is YOUR code, not library internals
  □ Logs — what happened immediately before the failure?
  □ State — what was the data / input / context when it failed?
  □ Diff — what changed between "last known good" and now?

EVIDENCE QUALITY CHECKS:
  □ Is this log from the right time? (timestamps match the incident?)
  □ Is this log from the right instance? (not a different server?)
  □ Is the error message the actual error or a wrapper? (look for "caused by")
  □ Is the state you're looking at the state AT failure or after?
```

### Phase 3 — Form and Test Hypotheses

**One hypothesis at a time. Test it. Then move to the next.**

```
HYPOTHESIS FORMAT:
  "I believe the cause is: <X>
   Because: <evidence that supports this>
   I will verify by: <specific test that proves or disproves X>
   If correct, I expect to see: <observable outcome>"

HYPOTHESIS TESTING RULES:
  □ Change ONE thing at a time — changing multiple things makes it
    impossible to know what fixed it (or whether anything did)
  □ If the test is inconclusive — do not assume the hypothesis is correct
  □ A fix that "seems to work" is not verified — define what verified means
  □ If hypothesis is disproven — update the evidence, not just the hypothesis

COMMON DEBUGGING TRAPS:
  🚩 "It must be X" before looking at evidence → confirmation bias
  🚩 Fixing the symptom without finding the cause → the bug will return
  🚩 Assuming the last change caused it → may be coincidental
  🚩 Stopping when it "seems fixed" → confirm with reproduction test
  🚩 Debugging in production without a safe way to revert changes
```

### Phase 4 — Isolate the Root Cause

Use one of the root cause methods from `references/root-cause-methods.md`:

**5 Whys** — for causal chains:
```
Problem: The report sent the wrong totals
Why 1: The totals calculation included cancelled orders
Why 2: The order filter wasn't applied before aggregation
Why 3: The filter was added to a different query than the aggregation uses
Why 4: The refactor separated queries without updating all callers
Why 5: There was no test that verified end-to-end filtering + aggregation
Root cause: Missing integration test allowed a refactor to break a filter
```

**Bisection** — for regressions:
```
"It worked at commit A. It's broken at commit Z.
 Test commit M (halfway). Working? → problem is between M and Z.
 Test halfway again. Repeat until exact commit is found."
```

**Component Isolation** — for distributed/complex systems:
```
Replace each component with a known-good stub until the bug disappears.
The bug is in the last component you replaced.
```

### Phase 5 — Verify the Fix

**The fix is not done until you have verified it addresses the root cause.**

```
VERIFICATION CHECKLIST:
  □ Can you reproduce the original bug with the fix reverted?
    (confirms you're testing the right thing)
  □ Does the fix make the reproduction test pass?
  □ Does the fix make sense given the identified root cause?
    (if not — you may have fixed a symptom, not the cause)
  □ Do existing tests still pass?
  □ Is there a new test that would have caught this bug originally?
    → Add it. Prevention > detection.
  □ Does the fix work under the same conditions that triggered the bug?
    (same load, same data, same environment)
```

---

## Debugging by Symptom

See `references/debugging-by-symptom.md` for common patterns:
- Works locally, fails in production
- Intermittent / non-reproducible failures
- Performance degradation
- Wrong output (no error)
- Timeout or hanging
- Memory leak
- Data corruption or inconsistency

---

## Root Cause Report

When the cause is found, document it before fixing:

```
ROOT CAUSE REPORT
══════════════════════════════════════
Symptom:       <observable wrong behavior>
Root cause:    <actual underlying cause>
Causal chain:  <symptom ← cause 1 ← cause 2 ← root cause>
Evidence:      <what confirmed this>

Fix:           <what will be changed and why it addresses the root cause>
Verification:  <how we confirmed the fix works>

Prevention:    <what would have prevented this — test? monitoring? review?>
Similar risks: <other places in the system where the same issue could exist>
══════════════════════════════════════
```

---

## Reference Files

- `references/root-cause-methods.md` — 5 Whys, bisection, isolation, fault tree
- `references/debugging-by-symptom.md` — Common symptom patterns and approaches
