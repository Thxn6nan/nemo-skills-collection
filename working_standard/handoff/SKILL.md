---
name: handoff
description: >
  Transfer work, context, and responsibility between people, teams, or phases
  with nothing lost in translation. Use this skill whenever ownership or work
  moves from one party to another — design to engineering, engineering to QA,
  team to team, person to person, or phase to phase. Trigger phrases: "hand
  this off", "transfer ownership", "design handoff", "dev handoff to QA",
  "knowledge transfer", "someone else is taking over", "document this for
  the next person", "onboarding the new owner", "passing the baton",
  "what does the next team need to know?", "prepare for handoff".
  A handoff is not done when the giver says "it's ready."
  It is done when the receiver confirms they have everything they need.
---

# Handoff Skill

Ensures that when work or ownership moves from one party to another,
nothing important is lost, assumed, or left to be rediscovered.

## Core Principle

> A handoff is complete when the receiver can continue independently
> without having to ask the giver basic questions.
> If the receiver needs to come back for clarification,
> the handoff was not finished — it was abandoned.

---

## Handoff Types

```
TYPE 1 — PHASE HANDOFF
  Work moves from one project phase to the next.
  Examples: Discovery → Design, Design → Engineering, Dev → QA, QA → Release
  
TYPE 2 — TEAM HANDOFF
  Ownership moves from one team to another.
  Examples: Platform team builds it, Product team owns it going forward.
  
TYPE 3 — PERSON HANDOFF
  A specific individual transfers their work or role to another person.
  Examples: Developer goes on leave, PM changes projects, offboarding.

TYPE 4 — ESCALATION HANDOFF
  Work that cannot be completed is handed to someone with more context or authority.
  Examples: Support to engineering, junior to senior, team to leadership.
```

---

## Handoff Workflow

### Step 1 — Confirm Readiness to Hand Off

The giver must verify the work is in a state worth handing off.

```
READINESS GATE:
  □ work-audit passed (no Critical or High defects)
  □ All decisions made — no open "we'll figure this out later" items
     (or: open items explicitly documented with clear ownership)
  □ The receiver has been identified and is available
  □ Enough time exists for a proper handoff (not 5 minutes before EOD)

DO NOT HAND OFF:
  🚩 Work that is "almost done" — finish it first or explicitly document what's left
  🚩 Work with undocumented critical decisions
  🚩 Work with unresolved blockers that the receiver will immediately hit
  🚩 Work where the receiver hasn't agreed to receive it
```

### Step 2 — Capture the Context Package

The context package is everything the receiver needs to continue independently.

**Six components — all required:**

```
1. CURRENT STATE
   What exists right now. Not what it will be — what it IS.
   Code: what is merged, what is in progress, what is pending
   Design: what is final, what is still being iterated
   Project: what phase, what is complete, what is next

2. DECISIONS MADE
   The important choices that were made and why.
   Not every decision — the ones that would confuse someone new.
   "We chose X over Y because Z" format.
   Without this: the receiver wastes time relitigating settled decisions.

3. OPEN ITEMS
   Things that are unresolved, in progress, or deferred.
   For each: what is it, who owns it after handoff, what's the status.
   Without this: open items get lost or duplicated.

4. KNOWN ISSUES AND RISKS
   Problems that exist, edge cases that aren't handled, technical debt incurred.
   Not to confess — to protect the receiver from being blindsided.
   "There is a known issue with X that happens when Y" is essential context.

5. DEPENDENCIES AND BLOCKERS
   What does this work depend on? What is blocking it?
   Who are the key people the receiver will need to work with?
   What external systems, approvals, or events are required?

6. HOW TO CONTINUE
   The next 3 actions the receiver should take.
   Not a full project plan — just enough to get started without being lost.
   "First, do X. Then Y. Don't do Z until A is resolved."
```

### Step 3 — Deliver the Handoff

Choose the format appropriate for the handoff type.
See `references/handoff-templates.md` for ready-to-use templates.

```
SYNCHRONOUS (recommended for complex handoffs):
  Walk through the context package together.
  Receiver asks questions. Giver answers.
  End with: receiver summarizes what they understand.
  If summary is wrong → correct before completing handoff.

ASYNC (acceptable for simpler handoffs):
  Write the context package in a shared document.
  Receiver reviews and responds with questions.
  Resolve all questions before handoff is considered complete.

NEVER:
  "It's all in Slack, just search for it"
  "You can figure it out from the code"
  "I'll answer questions as they come up" (with no documentation)
```

### Step 4 — Receiver Acceptance

**Handoff is not complete until the receiver explicitly accepts.**

```
RECEIVER ACCEPTANCE CHECKLIST:
  □ I understand the current state of the work
  □ I understand the key decisions that were made and why
  □ I know what open items exist and who owns each
  □ I am aware of the known issues and risks
  □ I know what my dependencies and blockers are
  □ I know what the next 3 actions are
  □ I have access to all necessary systems, files, and documents
  □ I know who to contact with questions after the handoff

EXPLICIT CONFIRMATION:
  Receiver sends: "I've reviewed the handoff document and I'm ready to take over.
                   I have questions about [X] — can we clarify before you're unavailable?"

  NOT acceptable: silence = acceptance
  NOT acceptable: "looks good" without having reviewed it
```

### Step 5 — Handoff Period (for complex handoffs)

For major handoffs (team changes, person offboarding), define a transition period.

```
TRANSITION PERIOD FORMAT:
  Phase 1 (Overlap): Both giver and receiver active. Receiver shadows.
  Phase 2 (Guided):  Receiver leads, giver available for questions.
  Phase 3 (Solo):    Receiver fully independent. Giver available only for emergencies.

  Duration by complexity:
    Simple task handoff:     0 days (clean document is enough)
    Feature ownership:       3-5 days
    Project ownership:       1-2 weeks
    Full role transition:    2-4 weeks
```

---

## Handoff Quality Signals

```
GOOD HANDOFF:
  ✅ Receiver continues work without contacting giver for basic questions
  ✅ No surprises discovered in the first week (known issues were documented)
  ✅ Decisions don't get re-opened unnecessarily
  ✅ Receiver finds the "how to continue" immediately actionable

POOR HANDOFF:
  🚩 Receiver contacts giver daily after handoff ("I should have mentioned...")
  🚩 Receiver rediscovers issues that giver knew about but didn't document
  🚩 Receiver makes decisions already made (decisions weren't documented)
  🚩 Work stalls because receiver doesn't know what to do next
  🚩 Receiver misses dependencies and hits blockers unexpectedly
```

---

## Connections to Other Skills

```
BEFORE handoff:
  work-audit         → ensure no Critical/High defects before transferring
  project-qa         → ensure deliverable matches requirements
  testing-strategy   → verify and validate before handing to QA

DURING handoff:
  working-standards  → receiver applies these to continue the work
  project-planner    → receiver uses this to plan next steps

AFTER handoff:
  retrospective      → if handoff had problems, capture lessons
  incident-response  → if something was missed and causes a production issue
```

---

## Reference Files

- `references/handoff-templates.md` — Ready-to-use templates for each handoff type
- `references/context-capture-guide.md` — How to capture context for complex handoffs
