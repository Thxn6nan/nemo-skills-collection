---
name: retrospective
description: >
  Extract actionable learning from completed work — what went well, what
  didn't, and what will actually change as a result. Use after a project,
  sprint, release, or significant event. Produces concrete action items,
  not just discussion. Trigger phrases: "retrospective", "retro", "what did
  we learn from this project?", "what went wrong?", "how do we improve?",
  "post-project review", "what should we do differently next time?",
  "team reflection", "lessons learned". A retrospective without action items
  is a venting session. This skill produces change.
---

# Retrospective Skill

Converts experience into improvement. A retrospective done well changes
how the team works next time. A retrospective done poorly wastes an hour.

## Core Principle

> The goal is not to discuss the past — it is to change the future.
> Every retrospective must end with specific, owned, time-bounded actions.
> If nothing changes after the retrospective, it was not worth having.

---

## Before the Retrospective

```
□ Gather data (don't rely on memory alone):
  - Timeline of key events
  - Metrics (velocity, quality, incidents, schedule variance)
  - Feedback from stakeholders or users if available

□ Create psychological safety:
  - Blameless framing: "how did our systems and processes fail?" not "who failed?"
  - Share data before opinions — ground discussion in facts

□ Set time box: 60-90 minutes for a sprint; up to 2 hours for a major project

□ Identify a facilitator (not the team lead — they should participate freely)
```

---

## Core Format: Start / Stop / Continue

Simple, works for most teams.

```
START: What should we begin doing that we aren't doing?
STOP:  What should we stop doing that isn't serving us?
CONTINUE: What is working well that we should keep doing?

Process:
  1. Silent individual writing (5-7 minutes): everyone writes their own items
  2. Share and cluster: group similar items together
  3. Dot voting: each person gets 5 votes to prioritize (can stack on one item)
  4. Discuss top-voted items (time-boxed: 5-7 minutes per item)
  5. Convert to action items
```

---

## Format for Project-Level Retros: 4Ls

For larger projects or major releases where more depth is needed.

```
LIKED:     What went well? What would we do again?
LEARNED:   What did we discover? What surprised us?
LACKED:    What was missing? What would have helped?
LONGED FOR: What do we wish we had? What would we want next time?

Process: same as Start/Stop/Continue — write, share, cluster, vote, discuss, action
```

---

## Format for Incident or Failure Retros

For retrospectives focused on something that went wrong.

```
TIMELINE: Walk through what happened (from the scribe's log if available)
  → Makes the discussion concrete, not memory-based

WHAT WENT WELL:
  Things that worked during the incident/failure response.
  Don't skip this — it prevents over-correcting and recognizes good work.

WHAT COULD BE IMPROVED:
  Systems, processes, tools, communication — not people.
  "Our monitoring didn't alert us" not "Alex didn't notice"

5 WHYS:
  Drive to root cause on the most important failure.
  Stops at the level where a systemic change is possible.

ACTION ITEMS:
  Specific, owned, time-bounded. See action item format below.
```

---

## Converting Discussion into Action Items

This is where most retrospectives fail — vague commitments that never happen.

```
BAD ACTION ITEM:
  "Improve our testing"
  "Better communication"
  "Document things more"

GOOD ACTION ITEM:
  "Add integration tests for the payment flow — Owner: Sam — By: Friday"
  "Send a weekly project status email every Monday — Owner: Jordan — Starting: next week"
  "Write the API documentation for the 3 new endpoints — Owner: Alex — By: next sprint"

GOOD ACTION ITEM CRITERIA:
  □ Specific: anyone reading it knows exactly what to do
  □ Owned: one named person is responsible (not "the team")
  □ Time-bounded: a specific date or milestone, not "eventually"
  □ Achievable: can realistically be done in the timeframe

ACTION ITEM LIMIT:
  Maximum 3-5 action items per retrospective.
  Teams that generate 15 action items complete 0.
  Choose the 3-5 that will have the most impact.
```

---

## Retrospective Output Format

```
RETROSPECTIVE RECORD
══════════════════════════════════════
Project/Sprint: <name>
Date:           <date>
Participants:   <names>
Facilitator:    <name>

WHAT WENT WELL:
  + <item>
  + <item>

WHAT COULD BE IMPROVED:
  - <item>
  - <item>

KEY INSIGHTS:
  <1-3 most important observations from the discussion>

ACTION ITEMS:
  □ <specific action> — Owner: <name> — By: <date>
  □ <specific action> — Owner: <name> — By: <date>
  □ <specific action> — Owner: <name> — By: <date>

REVIEW DATE: <when action items will be checked>
══════════════════════════════════════
```

---

## Making Retrospectives Stick

The most common failure: action items are written and never reviewed.

```
□ Assign a single owner to each action item (not "the team")
□ Schedule the review date before leaving the retrospective
□ Open the NEXT retrospective by reviewing last time's action items
□ If an action item was not completed: why not? What's the blocker?

RETROSPECTIVE HEALTH CHECK (ask periodically):
  "Are things actually changing as a result of our retrospectives?"
  YES → keep the format; consider whether to increase frequency
  NO  → something is wrong: safety? action quality? follow-through?
        address the meta-problem before running more retrospectives
```
