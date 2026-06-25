---
name: project-planner
description: >
  Translate an approved goal into an actionable execution plan — with task
  breakdown, dependency ordering, effort estimates, risk per task, milestones,
  and a clear sequence to start immediately. Use this skill after goal-evaluator
  outputs PURSUE or DESCOPE, and before working-standards begins execution.
  Trigger phrases: "plan this out", "break this down into tasks", "how do we
  approach this", "create a project plan", "sequence these tasks", "what should
  we do first", "turn this goal into steps", "estimate how long this will take",
  "map out the dependencies", "build a sprint plan", "what's the execution plan".
  This skill bridges decision (goal-evaluator) and execution (working-standards).
  It answers: what exactly needs to happen, in what order, by when.
  DISAMBIGUATION — "plan this out" is ambiguous: breaking ONE approved goal into
  executable tasks with dependencies and estimates → project-planner (this skill);
  sequencing MULTIPLE goals across a timeline → roadmap-planning; designing the
  technical architecture before building → system-design. Choose project-planner
  for task-level breakdown of a single initiative.
---

# Project Planner Skill

Converts an approved goal into a plan that can be executed immediately.
The output is not a document — it is a sequence of concrete tasks with
enough information that anyone on the team can pick one up and start.

## Core Principle

> A plan is not a list of things we hope will happen.
> It is a sequence of specific actions, with known dependencies,
> realistic estimates, and defined success conditions for each step.

A good plan surfaces problems before they happen.
A bad plan surfaces them during execution.

---

## The Planning Contract

Before planning, confirm three things:

```
□ The goal has passed goal-evaluator (or been consciously approved)
□ The goal statement is specific enough to decompose (Clarity Test passed)
□ The person who will execute has been identified — or the plan will identify them
```

If the goal is still vague → do not plan. Return to goal-evaluator and resolve
clarity first. Planning a vague goal produces a vague plan that cannot be executed.

---

## Six Planning Phases

### Phase 1: Goal Decomposition

**Break the goal into the smallest executable tasks.**

A task is the right size when:
```
□ One person can own it completely
□ It can be completed in 30 minutes to 1 day
□ Its output is clearly defined (done = X is true)
□ Someone else could pick it up and know what to do

Too large:  "Build the authentication system"  ← not a task
Right size: "Write the JWT token validation function with unit tests"
```

**Decomposition method — work top-down:**
```
Goal
├── Component A  (major workstream)
│   ├── Task A1
│   ├── Task A2
│   └── Task A3
├── Component B
│   ├── Task B1
│   └── Task B2
└── Component C (cross-cutting: testing, docs, integration)
    ├── Task C1
    └── Task C2
```

**Stop decomposing when:** every leaf task meets the right-size criteria above.

**Check for hidden tasks** — things that are always needed but often forgotten:

```
□ Environment / tooling setup (never "5 minutes")
□ External credentials, API access, or permissions
□ Schema or data model decisions before implementation
□ Integration tests (not just unit tests)
□ Error handling and edge cases (not just happy path)
□ Code review cycle time
□ Documentation updates
□ Handoff to next person or system
□ Monitoring / alerting setup if deploying
```

See `references/task-breakdown-guide.md` for decomposition patterns by project type.

---

### Phase 2: Dependency Mapping

**Identify what must be done before what.**

For each task pair (A, B), ask: "Can B start before A is completely done?"

```
Types of dependencies:
  HARD   — B cannot start until A is fully complete
             (e.g., "write tests" needs "define the function signature")
  SOFT   — B is easier if A is done first, but not blocked
             (e.g., "write documentation" is easier after implementation)
  NONE   — A and B are fully independent (parallelizable)
```

**Map the Critical Path:**
The critical path is the longest sequence of HARD dependencies.
It determines the minimum time to completion regardless of resources.

```
Find it by:
  1. Draw all tasks and HARD dependency arrows
  2. Find all paths from start to finish
  3. The longest path = critical path
  4. Any delay on the critical path delays the whole project

Tasks NOT on the critical path have "float" — they can slip
without delaying the project (up to their float amount).
```

**Parallelization opportunities:**
Independent tasks (no dependency between them) can be done simultaneously.
For agent workflows, these are prime candidates for parallel execution.

---

### Phase 3: Effort Estimation

**Estimate each task honestly — then correct for bias.**

**Three-point estimation for every task:**
```
  Optimistic (O):   If everything goes smoothly — no blockers, no unknowns
  Realistic (R):    Most likely outcome given past experience
  Pessimistic (P):  If the hard parts are harder than expected

  Weighted estimate = (O + 4R + P) / 6   ← PERT formula

  Example:
    O = 2h, R = 4h, P = 10h
    Weighted = (2 + 16 + 10) / 6 = 4.7h → round to 5h
```

**Planning fallacy correction — apply multiplier to weighted estimate:**
```
  Task type                              Multiplier
  ─────────────────────────────────────────────────
  Familiar, done before, well-defined    ×1.2
  Mostly familiar, some new parts        ×1.5
  New domain or significant unknowns     ×2.0
  Research-heavy or exploratory          ×2.5
  Depends on external team/system        ×2.0 (+ buffer for wait time)
```

**Total project estimate:**
```
  Critical path duration = Σ (corrected estimates of critical path tasks)
  Total effort = Σ (corrected estimates of ALL tasks)
  
  These are different numbers:
    Critical path duration → calendar time (how long before done)
    Total effort → person-hours needed (how much work)
```

---

### Phase 4: Risk per Task

**Each task gets its own risk assessment — not just the project overall.**

For each task, identify:

```
TASK RISK FORMAT:
  Task: <name>
  Primary risk: <what is most likely to make this take longer or fail>
  Trigger: <how you'll know the risk is happening>
  Response: <what to do when it triggers>
  Risk type: [Blocked | Underestimated | External | Knowledge gap | Dependency slip]
```

**Special flag — Risk Frontloading:**
Tasks that are high-risk should be done EARLY, not late.
This is the most important sequence principle beyond dependencies.

```
Why: A task that fails in week 1 costs 1 week.
     A task that fails in week 6 costs 6 weeks + all the work that depended on it.

Rule: If two tasks have no dependency between them, do the riskier one first.
```

**Tasks that are almost always higher-risk than estimated:**
```
🚩 "We just need to integrate with their API" — external dependency
🚩 "The data is already there, just need to pull it" — data quality unknown
🚩 "We've done this before" — context is always different
🚩 "It's just configuration" — config is often where time disappears
🚩 "We'll handle edge cases after the happy path works" — edge cases are the work
```

---

### Phase 5: Sequence and Milestones

**Combine dependencies, risk, and value to determine execution order.**

**Sequencing rules (apply in priority order):**
```
Rule 1 — Respect HARD dependencies (non-negotiable)
Rule 2 — Do high-risk tasks early (risk frontloading)
Rule 3 — Do tasks that unblock others next (maximize parallel work)
Rule 4 — Do tasks that deliver visible value next (early signal)
Rule 5 — Do low-risk, no-dependency tasks last (or in parallel to above)
```

**Milestones — not "percentage done" but observable gates:**
```
Good milestone: "The API returns correct data for the 3 test cases"
Bad milestone:  "Backend is 70% done"

Each milestone should answer: "If we stop here, what do we have?"
A milestone with a useful answer is a real stopping point if priorities change.
```

**Recommended milestone cadence:**
```
M1 (end of ~20% of timeline): Core assumption validated
   → The thing most likely to be wrong has been tested
   
M2 (end of ~50% of timeline): Critical path half complete
   → Realistic projection of final delivery is now possible
   
M3 (end of ~80% of timeline): Feature complete (unhardened)
   → All functionality present; QA and hardening remain
   
M4: Done
   → Meets definition of success from goal-evaluator
```

---

### Phase 6: Plan on a Page

**The deliverable — everything needed to start execution.**

See `references/plan-format.md` for the full template and worked example.

```
PLAN SUMMARY
  Goal:           <one sentence>
  Total effort:   <N person-days>
  Calendar time:  <N weeks if 1 person | N weeks if N people>
  Critical path:  <sequence of critical tasks>
  Start today:    <the single first task to begin right now>

TASK TABLE
  ID   Task                    Owner   Est    Depends on   Risk
  ──   ──────────────────────  ──────  ─────  ───────────  ────
  T1   <task>                  <who>   <Xh>   —            Low
  T2   <task>                  <who>   <Xh>   T1           Medium
  ...

MILESTONES
  M1 [week X]: <observable condition>
  M2 [week X]: <observable condition>
  M3 [week X]: <observable condition>
  M4 [week X]: Done — <success definition from goal-evaluator>

KNOWN RISKS
  <task>: <risk> → Response: <what to do>
```

---

## Handoffs To and From This Skill

```
FROM goal-evaluator:
  Receives: approved goal + success definition + exit criteria + assumptions

TO working-standards:
  Passes: task list + sequence + effort estimates + risk flags

TRIGGERS project-qa when:
  A task's scope is ambiguous — use project-qa PRE mode to define boundaries

TRIGGERS tenth-man when:
  The plan feels confident — use tenth-man to find what's missing

TRIGGERS anti-hallucination when:
  Estimates are derived from data — verify the numbers
```

---

## Reference Files

- `references/task-breakdown-guide.md` — Decomposition patterns by project type
- `references/plan-format.md` — Full plan template + worked example

---

## Example Invocations

```
"Plan out how we'll build this feature"
"We've decided to pursue this goal — break it into tasks"
"Sequence these tasks so we can start tomorrow"
"How long will this realistically take?"
"Create a sprint plan for this"
"Map the dependencies for this project"
"What should we do first and why?"
"Turn this goal into a week-by-week execution plan"
```
