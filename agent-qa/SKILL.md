---
name: agent-qa
description: >
  Enforce scope discipline and quality standards for agent tasks — preventing agents
  from doing work that wasn't asked for, isn't necessary yet, or doesn't match the
  original intent. Use this skill whenever an agent seems to be doing "extra" work,
  going off-task, taking unilateral decisions, modifying things it shouldn't touch,
  or when you want to define a quality gate before an agent starts a task.
  Also trigger when reviewing an agent's task plan before execution ("does this plan
  match what was asked?"), or when the agent's behavior seems inconsistent between runs.
  Trigger phrases: "is this in scope?", "the agent did too much", "QA this task",
  "define what the agent should/shouldn't do", "agent went off script", "set boundaries
  for the agent", "validate the task plan", "inconsistent agent behavior".
---

# Agent QA Skill

Enforces the **Minimum Necessary Work** principle: agents should do exactly what
was asked — no more, no less — with consistent, predictable behavior.

## Core Principle

An agent that does *more* than asked is not being helpful — it's being unpredictable.
Scope creep in agents causes: unintended side effects, wasted compute, hard-to-debug
state changes, and loss of human oversight.

> "Did the task. Only the task. Nothing else." — the gold standard.

---

## Three Modes

This skill operates in three modes depending on when it's invoked:

| Mode | When | Goal |
|------|------|------|
| **PRE** — Task Gate | Before agent starts | Validate scope before execution |
| **MID** — Action Guard | During execution | Challenge each action in real time |
| **POST** — Scope Review | After agent finishes | Audit what was actually done vs. asked |

---

## Mode 1: PRE — Task Gate

Use when reviewing an agent's task plan before it runs.

### Step 1 — Extract the Original Intent
Identify the canonical task statement:
- What was the user's **exact request**?
- What is the **minimum deliverable** that satisfies it?
- What is **explicitly out of scope**?

### Step 2 — Validate Each Planned Action
For every step in the agent's plan, ask:

```
RELEVANCE CHECK:
□ Does this action directly serve the original goal?
□ Was this action explicitly or clearly implicitly requested?
□ Would skipping this action cause the goal to fail?

If any answer is NO → flag as OUT-OF-SCOPE
```

### Step 3 — Issue a Task Gate Report
See `references/gate-report-format.md` for full format.

```
GATE: [APPROVED | TRIMMED | BLOCKED]

In-scope actions:   <list>
Flagged actions:    <list with reason>
Blocked actions:    <list — must be removed>

Scope boundary: "<one sentence defining what this task IS and IS NOT>"
```

---

## Mode 2: MID — Action Guard

Use as a real-time checkpoint during agent execution.
Before each tool call or action, apply this test:

```
ACTION: <what the agent is about to do>

1. NECESSITY  — Is this required to complete the task? (Yes/No)
2. AUTHORIZED — Was this type of action asked for? (Yes/No/Implied)
3. REVERSIBLE — Can this be undone if wrong? (Yes/No)
4. SIDE EFFECTS — Does this change state outside the task scope? (Yes/No)

DECISION:
  All Yes / Low risk → PROCEED
  Any No + irreversible → HOLD and confirm with user
  Out-of-scope side effect → SKIP and log
```

---

## Mode 3: POST — Scope Review

Use after the agent has finished to audit what it actually did.

### Step 1 — Reconstruct the Action Log
List every action the agent took (tool calls, file writes, API calls, decisions).

### Step 2 — Classify Each Action

| Category | Meaning |
|----------|---------|
| ✅ **Required** | Directly necessary for the goal |
| ⚠️ **Bonus** | Not asked for but harmless |
| ❌ **Violation** | Changed things outside scope or caused side effects |
| 🔁 **Redundant** | Did the same thing twice unnecessarily |

### Step 3 — Scope Drift Score

```
Scope Drift Score = (Bonus + Violation + Redundant) / Total Actions × 100

0–10%   → CLEAN    (agent stayed on task)
11–25%  → LOOSE    (minor drift, review bonus actions)
26–50%  → DRIFTED  (significant out-of-scope work done)
>50%    → RUNAWAY  (agent did not follow instructions reliably)
```

### Step 4 — Deliver Scope Review
See `references/scope-review-format.md` for full format.

---

## Consistency Standards

For agents run multiple times on similar tasks, check:

- **Determinism** — Same input → same category of output?
- **Boundary respect** — Does the agent stop at the same place each time?
- **Decision consistency** — Are ambiguous cases handled the same way?

If inconsistency detected → document the trigger condition and recommend
adding it to the agent's explicit scope definition.

---

## Anti-Patterns to Flag

```
🚩 "While I'm at it..." — agent doing extra work unprompted
🚩 "I also fixed/updated/improved..." — unsolicited changes
🚩 "I assumed you'd want..." — agent making scope decisions for the user
🚩 Modifying files not mentioned in the task
🚩 Making API calls not required for the stated goal
🚩 Deleting or overwriting things "to clean up"
🚩 Installing dependencies beyond what the task needs
```

---

## Reference Files

- `references/gate-report-format.md` — PRE mode full report template
- `references/scope-review-format.md` — POST mode full report template

---

## Example Invocations

```
"QA this task plan before the agent runs it"
"The agent modified files I didn't ask it to — review what happened"
"Define the scope boundary for this agent task"
"Agent behavior is inconsistent between runs — what's wrong?"
"Did the agent do anything it wasn't supposed to?"
"Validate that this agent plan matches what was asked"
```
