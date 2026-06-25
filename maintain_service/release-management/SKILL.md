---
name: release-management
description: >
  Manage how project outputs — features, products, reports, or changes — are
  released to users, clients, or stakeholders. Covers pre-release readiness,
  communication, rollout approach, and rollback. Use this skill before releasing
  any project deliverable that affects others, whether it's shipping a feature,
  publishing a report, deploying a product update, or handing off to a client.
  Trigger phrases: "how do we release this?", "prepare for launch", "ship this
  feature", "release plan", "how do we roll this out?", "communication plan for
  this release", "what do we do before we go live?", "ready to ship?",
  "release checklist", "how do we announce this?", "rollback plan if this fails".
  This skill covers the PROJECT RELEASE — not deployment infrastructure.
---

# Release Management Skill

Ensures project work reaches its intended recipients in a controlled,
communicated, and reversible way. A release without a plan is a gamble.

## Core Principle

> Every release is a commitment to the people receiving it.
> They will act on what you ship. Make sure it's ready,
> and make sure you know what to do if it isn't.

---

## Release Readiness Assessment

Before choosing a rollout approach, assess readiness on three dimensions:

### Quality Readiness

```
□ work-audit passed — no Critical or High defects
□ project-qa POST check passed — deliverable matches requirement
□ anti-hallucination check run — if output contains data or numbers
□ Acceptance criteria met — all criteria from the original requirement
□ Reviewed by someone other than the creator
```

### Risk Assessment

```
RISK FACTORS (each raises release risk):
  □ First release of this type of output          +HIGH
  □ Output affects a large number of recipients   +MEDIUM
  □ Reversing this release would be difficult      +HIGH
  □ Output contains decisions or recommendations   +MEDIUM
  □ Recipients have not seen a draft or preview    +MEDIUM
  □ Significant changes from previous version      +MEDIUM
  □ Time-sensitive (errors can't be fixed quietly) +HIGH

RISK LEVEL:
  0-1 factors → LOW    → Standard release
  2-3 factors → MEDIUM → Staged release or preview first
  4+ factors  → HIGH   → Controlled release with rollback ready
```

### Stakeholder Readiness

```
□ Recipients identified — who exactly receives this?
□ Communication drafted — what will they be told?
□ Timing confirmed — is this the right moment to release?
□ Support ready — is someone available to handle questions?
□ Expectations set — do recipients know what to expect?
```

---

## Rollout Approaches

Choose based on risk level:

### Standard Release (Low Risk)
Release to all recipients at once with standard communication.

```
Use when:
  - Routine update or expected delivery
  - Recipients are familiar with this type of output
  - Easy to correct if issues arise

Steps:
  1. Final quality check
  2. Prepare communication (what recipients need to know)
  3. Release to all recipients
  4. Confirm receipt where required
  5. Monitor for questions or issues for 24 hours
```

### Staged Release (Medium Risk)
Release to a subset of recipients first; expand after validation.

```
Use when:
  - New type of output or significant change
  - Large recipient group where problems would have wide impact
  - Feedback from a subset can improve the full release

Steps:
  1. Identify a pilot group (5-20% of recipients, or a friendly cohort)
  2. Release to pilot group
  3. Collect feedback; validate no critical issues
  4. If clean after 24-48 hours → release to all
  5. If issues found → fix first, then re-run pilot or release to all
```

### Controlled Release (High Risk)
Preview or approval gate before release; explicit confirmation at each step.

```
Use when:
  - High-stakes output (financial, legal, public-facing)
  - Difficult to reverse once released
  - Recipients are external clients or the general public

Steps:
  1. Internal review and sign-off
  2. Preview to key stakeholders for approval
  3. Approval gate — explicit confirmation before proceeding
  4. Controlled release with monitoring
  5. Formal acknowledgment from recipients
  6. Post-release review after 72 hours
```

---

## Release Communication

Every release needs communication that answers:

```
WHAT:    What are recipients receiving?
         (be specific — not "an update" but "the Q3 financial summary")

WHY:     Why are they receiving it now?
         (what need or request does this fulfill?)

HOW:     What do recipients need to do with it?
         (read, approve, implement, provide feedback?)

WHEN:    Is there a deadline for their action?

WHERE:   Where can they find it / how do they access it?

WHO:     Who do they contact with questions?
```

**Communication format by recipient type:**

```
Internal team:     Short message + link; minimal formality
Client / partner:  Professional email with context; clear ask
Public / users:    Release notes; user-facing language; no jargon
Executive:         One paragraph summary + key decisions or actions needed
```

---

## Rollback Plan

Required before any Medium or High risk release.

```
ROLLBACK PLAN FORMAT:
  Trigger condition:  "We roll back if: <specific condition>"
                      (not vague — define a measurable threshold)

  Rollback action:    "To roll back, we will: <exact steps>"
                      (pre-written, not figured out in a crisis)

  Rollback owner:     <who executes the rollback>

  Rollback time:      "We expect rollback to complete in: <time>"

  Communication:      "If we roll back, we will tell recipients: <message>"

COMMON ROLLBACK ACTIONS BY RELEASE TYPE:
  Document/report:    Send a correction or updated version; flag the issue
  Feature/product:    Revert to previous version; notify users of temporary rollback
  Process change:     Revert to previous process; communicate timeline for fix
  Data release:       Retract data; issue corrected version with explanation
```

---

## Release Record

Document every release for future reference:

```
RELEASE RECORD
══════════════════════════════════════
Release:        <what was released>
Version/date:   <identifier>
Approach:       [Standard | Staged | Controlled]
Recipients:     <who received it>
Released by:    <name>
Date/time:      <timestamp>

Quality checks passed:
  □ work-audit    □ project-qa    □ anti-hallucination (if applicable)

Outcome:        [Successful | Issues found | Rolled back]
Issues noted:   <any problems that arose>
Lessons:        <what to do differently next time>
══════════════════════════════════════
```

---

## Reference Files

- `references/release-checklist.md` — Pre-release checklist by output type
- `references/communication-templates.md` — Communication templates for common release types
