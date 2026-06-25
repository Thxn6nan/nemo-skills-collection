---
name: project-compass
description: >
  Identify what to improve, which direction to develop, what to continue or cut,
  and what to do next to unlock a project's potential. Use this skill when a project
  needs strategic direction rather than defect-fixing — when the question is not
  "is this broken?" but "where should this go and how do we get there?"
  Trigger phrases: "what should we focus on next", "where should this project go",
  "what's worth improving", "should we continue or pivot", "what's holding this back",
  "help us prioritize", "what has the most potential", "what should we cut",
  "what's the next big move", "how do we make this better", "improve this project",
  "what direction should we take", or any question about what to build, keep, or drop.
  This skill gives strategic direction — not defect lists (work-audit) or
  scope checks (project-qa). It answers: what next, and why.
  DISAMBIGUATION — choose by what "next" means: strategic direction for an
  EXISTING project (grow/cut/pivot) → project-compass (this skill); sequencing
  MULTIPLE goals onto a timeline → roadmap-planning; deciding whether a single
  NEW goal is worth pursuing → goal-evaluator; breaking ONE approved goal into
  executable tasks → project-planner.
---

# Project Compass Skill

Finds the highest-leverage path forward for a project. Identifies what to grow,
what to cut, and which direction to take — with concrete next steps and the
reasoning behind each recommendation.

## Core Principle

> A project's potential is not determined by what it is today.
> It is determined by what is currently blocking it from becoming
> what it could be — and which of those blockers are worth removing.

The goal is not to make everything better. It is to identify the **one or two
moves** that unlock the most value, and the **one or two things** that are
consuming resources without a path to return.

---

## Three Questions This Skill Answers

Every engagement with this skill answers three questions in order:

```
1. WHERE IS THIS GOING?
   → Trajectory: if nothing changes, where does this project end up?
   → Potential ceiling: what's the best realistic version of this?
   → Gap: what's between current trajectory and potential ceiling?

2. WHAT IS HOLDING IT BACK?
   → Bottlenecks: the constraints limiting progress most
   → Drag: things consuming resources without producing value
   → Blind spots: assumptions that may be wrong

3. WHAT SHOULD HAPPEN NEXT?
   → Grow: what to invest more in
   → Kill: what to cut or stop
   → Keep: what to maintain without adding resources
   → Start: what new thing would unlock the most value
   → The single most important next action
```

---

## Workflow

### Step 1 — Understand the Current State

Gather enough context to answer:
- What does this project do, and for whom?
- What's working? What's not?
- What resources are being spent (time, compute, money, attention)?
- What has already been tried?
- What does the person/team most want to achieve?

If context is incomplete → ask one focused question before proceeding.
Do not run analysis on insufficient information.

### Step 2 — Trajectory Analysis

Determine where the project is heading if nothing changes.

```
MOMENTUM: [Building | Stable | Declining | Stalled]

Evidence for this momentum:
  Positive signals: <what is growing or improving naturally>
  Negative signals: <what is degrading or slowing>

If nothing changes, in 3 months this project will:
  <concrete prediction based on current trajectory>

The biggest risk to current trajectory:
  <what could cause faster decline or stall>
```

See `references/potential-analysis.md` for how to identify trajectory signals.

### Step 3 — Potential Ceiling Analysis

Determine the realistic best version of this project.

```
POTENTIAL CEILING: [Narrow / Moderate / High / Exceptional]

What this project could realistically become:
  <concrete description of best-case outcome>

What would need to be true for the ceiling to be reached:
  Condition 1: <what must happen>
  Condition 2: <what must change>
  Condition 3: <what must be built or acquired>

Current distance from ceiling: [Close | Moderate | Far]
Primary gap type: [Capability | Resources | Direction | Execution | Timing]
```

### Step 4 — Bottleneck Identification

Find the constraint limiting progress most.

The Theory of Constraints: **improving anything that is not the bottleneck
does not improve the system.** Identify the one thing that, if resolved,
would unlock the most forward motion.

```
PRIMARY BOTTLENECK: <what is the single most limiting constraint>
  Type: [Technical | Knowledge | Resource | Process | External | People]
  Evidence: <why this is the primary constraint, not a secondary one>
  Effect: <what becomes possible once this is resolved>

SECONDARY BOTTLENECKS: (address after primary)
  1. <constraint> — will matter once primary is resolved
  2. <constraint>
```

See `references/potential-analysis.md` for bottleneck identification patterns.

### Step 5 — Kill / Keep / Grow / Start Classification

Classify every significant component, feature, or workstream:

```
KILL — Stop these. Consuming resources with no clear path to value.
  • <item> — Reason: <why cut> | Resource freed: <what's recovered>

KEEP — Maintain but don't add resources. Working fine, not a priority.
  • <item> — Reason: <why maintain without growth>

GROW — Invest more here. High potential, currently under-resourced.
  • <item> — Reason: <why this deserves more> | Expected return: <what grows>

START — Begin this. Not yet existing but would unlock significant value.
  • <item> — Reason: <why start now> | Prerequisite: <what must be true first>
```

See `references/prioritization-framework.md` for the Value-Effort matrix
and how to make Kill vs Keep vs Grow decisions.

### Step 6 — Direction Options

Present 2–3 distinct strategic directions (not just one answer).
Each direction should lead to a meaningfully different outcome.

```
DIRECTION A: <name — e.g., "Double Down">
  Strategy:    <what this means in practice>
  Best if:     <conditions under which this is the right choice>
  Trade-off:   <what this gains vs what it sacrifices>
  First move:  <most important immediate action>

DIRECTION B: <name — e.g., "Broaden Scope">
  Strategy:    <what this means in practice>
  Best if:     <conditions under which this is the right choice>
  Trade-off:   <what this gains vs what it sacrifices>
  First move:  <most important immediate action>

DIRECTION C: <name — e.g., "Pivot Focus">
  Strategy:    <what this means in practice>
  Best if:     <conditions under which this is the right choice>
  Trade-off:   <what this gains vs what it sacrifices>
  First move:  <most important immediate action>

RECOMMENDED: Direction <X>
  Because: <specific reasoning for this project's situation>
  Assuming: <what must be true for this to be the right call>
```

### Step 7 — Deliver the Compass Report

See `references/direction-report-format.md` for full template.

The report ends with:
```
THE ONE THING:
  The single highest-leverage action right now is:
  <concrete, specific, actionable — not "improve X" but "do Y to achieve X">
  
  Why this one: <reason this unlocks more than anything else>
  When to revisit direction: <what signal or milestone triggers a strategy review>
```

---

## What This Skill Is Not

| This skill answers | Use a different skill for |
|-------------------|--------------------------|
| What to build next | Whether what was built is correct → `work-audit` |
| Which direction to take | Whether the agent stayed in scope → `project-qa` |
| What to cut | Whether output contains errors → `anti-hallucination` |
| What has the most potential | Whether the system is well-engineered → `working-standards` |
| What assumptions are wrong | Stress-testing a specific plan → `tenth-man` |

---

## Reference Files

- `references/potential-analysis.md` — Trajectory signals, ceiling analysis, bottleneck identification
- `references/prioritization-framework.md` — Value-Effort matrix, Kill/Keep/Grow decision rules
- `references/direction-report-format.md` — Full report template with worked example

---

## Example Invocations

```
"What should we focus on to make this project better?"
"We've built a lot of things — what's worth keeping and what should we cut?"
"The project feels stuck — what's holding it back?"
"What has the most potential here and how do we unlock it?"
"Should we go deeper on this or pivot to something else?"
"What's the most important thing to do next?"
"We have 3 features half-built — which one should we finish?"
"Where is this project realistically going, and is that where we want it?"
```
