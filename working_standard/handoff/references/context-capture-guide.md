# Context Capture Guide

How to surface and document the context that lives in someone's head
before it walks out the door.

---

## The Context Extraction Problem

Most knowledge in a project is tacit — it exists in someone's head, not in documents.
This includes: why decisions were made, what was tried and failed, what the tricky
edge cases are, who the right person is to ask about X, and what "normal" looks like.

A handoff document that only captures what's in documents misses most of what matters.
This guide helps extract the tacit knowledge before the handoff.

---

## The 5 Questions Interview

Run this as a conversation with the giver before writing the handoff document.
Record or take notes. Don't skip questions even if they seem obvious.

```
Q1: "Walk me through a typical day/week of work on this.
     What do you actually do?"
     → Surfaces the real workflow, not the documented one

Q2: "What's the thing you're most worried about handing over?
     What do you think I'll struggle with?"
     → Surfaces hidden complexity and risk

Q3: "What's something that looks wrong but is actually right?
     What would I be tempted to change that I shouldn't?"
     → Surfaces counterintuitive decisions that need protection

Q4: "Who are the 3 people I absolutely need to know?
     Who has context I can't get from documents?"
     → Surfaces the human dependency graph

Q5: "What almost went wrong? What do you wish you'd known earlier?"
     → Surfaces near-misses and institutional knowledge about risk
```

---

## Context Layers

Capture context at three levels of depth:

```
LAYER 1 — WHAT (captured in most handoff documents)
  What exists, what is built, what is the current state.
  Easy to document. Most people do this.

LAYER 2 — WHY (often missing from handoff documents)
  Why decisions were made. Why things are structured this way.
  Why certain approaches were rejected.
  The most valuable and most often lost context.

LAYER 3 — HOW IT ACTUALLY WORKS (almost never documented)
  The informal processes, workarounds, tribal knowledge.
  "The deploy script has a bug so you have to run X first"
  "The stakeholder says she wants weekly updates but she really
   means she wants to be looped in on anything that changes scope"
```

---

## Decision Documentation Format

For each significant decision in the project:

```
DECISION: <what was decided>
DATE: <when — approximate is fine>
CONTEXT: <what situation prompted this decision>
OPTIONS CONSIDERED:
  A: <option> — why not chosen: <reason>
  B: <option> — why not chosen: <reason>
  C: <option> — CHOSEN — because: <reason>
TRADE-OFF ACCEPTED: <what was given up>
REVISIT IF: <conditions that would make this worth reconsidering>
DECISION OWNER: <who made this call>
```

---

## The "Hit by a Bus" Test

Before completing a handoff, ask:

```
"If the current owner were completely unavailable tomorrow —
 no calls, no email, no questions — could the receiver continue?"

Run through these scenarios:
  □ An urgent bug is reported. Can the receiver find and fix it?
  □ A stakeholder asks for a status update. Can the receiver give one?
  □ An architectural question comes up. Can the receiver find the rationale?
  □ A production incident occurs. Can the receiver follow the runbook?
  □ A new team member joins. Can the receiver onboard them?

If any answer is NO → the handoff document is incomplete.
```

---

## Runbook Checklist (for operational handoffs)

If the handoff includes operational responsibility, verify these exist:

```
DAILY OPERATIONS:
  □ How to check if everything is healthy (monitoring links, key metrics)
  □ What "normal" looks like vs what should trigger concern
  □ Routine tasks (deploys, reports, cleanups) and their schedule

INCIDENT RESPONSE:
  □ Who to contact when something breaks (escalation chain)
  □ How to roll back a bad deployment
  □ Where the runbook lives for common incident types
  □ How to communicate an incident to stakeholders

ACCESS:
  □ How to get access to production systems
  □ Who approves emergency access
  □ Where secrets/credentials are stored

KNOWN FRAGILITIES:
  □ "Don't do X without doing Y first"
  □ "This breaks when Z happens"
  □ "This only works if A is running"
```

---

## Handoff Quality Checklist

Use before marking a handoff as complete:

```
COMPLETENESS:
  □ All 6 context components documented (state, decisions, open items,
    issues, dependencies, how-to-continue)
  □ 5 Questions Interview completed
  □ Decision log captured for major decisions
  □ Tacit knowledge extracted (not just documented facts)

CLARITY:
  □ A person unfamiliar with this project could understand the document
  □ No jargon without explanation
  □ "What" and "why" both answered (not just what)

RECEIVER READY:
  □ Receiver has confirmed access to all systems
  □ Receiver has confirmed understanding (not just receipt)
  □ Open questions from receiver are resolved or tracked
  □ Transition period agreed (if needed)

GIVER AVAILABLE:
  □ Giver has committed to a window of availability post-handoff
  □ Escalation path defined if giver becomes unavailable
```
