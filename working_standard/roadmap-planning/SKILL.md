---
name: roadmap-planning
description: >
  Build and maintain a product roadmap — prioritizing goals across multiple
  initiatives, placing them on a timeline, managing dependencies between goals,
  and communicating the plan to different stakeholders. Use this skill for
  quarterly planning, annual planning, or whenever multiple goals need to be
  sequenced and communicated as a coherent plan. Trigger phrases: "build a
  roadmap", "quarterly planning", "prioritize these goals", "what do we work
  on this quarter?", "how do we sequence these initiatives?", "stakeholder
  roadmap", "product roadmap", "annual planning", "what's our plan for the
  next 6 months?", "re-prioritize the roadmap", "how do we communicate what
  we're building?". Bridges goal-evaluator (should we do this?) and
  project-planner (how do we do this?). Answers: what, in what order, by when.
---

# Roadmap Planning Skill

Turns a collection of goals into a coherent, communicable plan.
A roadmap is not a promise — it is a current best thinking about what to
build, in what order, based on what we know today.

## Core Principle

> A roadmap answers three questions:
> What are we building? In what order? Why that order?
> Everything else — the timeline, the format, the presentation —
> is in service of those three answers.

---

## Where This Skill Fits

```
goal-evaluator     → "Should we pursue goal X?" (individual goal assessment)
        ↓
roadmap-planning   → "Given all approved goals, what's the sequence and timeline?"
        ↓
project-planner    → "How do we execute the next goal on the roadmap?"
```

**Input to this skill:** a pool of goals that have passed goal-evaluator
(or been otherwise approved as worth pursuing)

**Output:** a prioritized, time-bounded roadmap with stakeholder-ready views

---

## Roadmap Planning Workflow

### Phase 1: Goal Inventory

Collect all candidate goals into one place before prioritizing.

```
GOAL INVENTORY FORMAT:
  ID    Goal                          Source           EV*      Effort
  ────  ────────────────────────────  ───────────────  ──────   ──────
  G01   <goal>                        User research    High     Medium
  G02   <goal>                        Strategy         Medium   Low
  G03   <goal>                        Tech debt        Low      High
  G04   <goal>                        Stakeholder req  High     High
  ...

*EV = Expected Value from goal-evaluator, or estimated if not yet run

SOURCES TO COLLECT FROM:
  □ User research and feedback
  □ Sales and support requests
  □ Strategic initiatives from leadership
  □ Technical debt and infrastructure needs
  □ Regulatory or compliance requirements
  □ Competitive threats or opportunities
```

### Phase 2: Prioritization

Score and rank goals relative to each other.
Not every goal needs a precise score — the goal is relative ordering, not absolute truth.

**Use RICE scoring for product goals:**

```
RICE = (Reach × Impact × Confidence) / Effort

REACH:       How many users/customers does this affect per period?
             (number, not "all users" — estimate concretely)

IMPACT:      How much does this move the needle per user who is reached?
             3 = massive  2 = significant  1 = moderate  0.5 = minimal  0.25 = trivial

CONFIDENCE:  How confident are we in the Reach and Impact estimates?
             100% = high evidence  80% = medium  50% = low  (use 50% minimum)

EFFORT:      How many person-months does this require?
             (use estimates from goal-evaluator or project-planner)

EXAMPLE:
  Goal: Add push notifications
  Reach:      8,000 users/month
  Impact:     1 (moderate — increases engagement)
  Confidence: 80% (some evidence but not proven)
  Effort:     1 person-month
  
  RICE = (8,000 × 1 × 0.80) / 1 = 6,400

Higher RICE = higher priority. Sort all goals by RICE score.
```

**Adjust for dependencies:**

```
After scoring, check for dependencies between goals:
  □ Does G03 require G01 to be done first?
  □ Do G02 and G04 share infrastructure that should be built once?
  □ Do any goals conflict (pursuing both would dilute impact)?

DEPENDENCY MAP:
  G01 ──→ G03 (G03 requires G01)
  G02         (independent)
  G04 ──→ G05 (G05 requires G04)

Adjust order: goals that unlock others get a priority boost
even if their individual RICE score is lower.
```

### Phase 3: Capacity Planning

Fit goals into available capacity.

```
TEAM CAPACITY:
  Team size:          <N> people
  Planning period:    <N> months (quarter = 3, half-year = 6)
  Velocity estimate:  roughly <N> person-months of output
  Buffer for unplanned work: subtract 20-30%
  Available capacity: <N> person-months net

FIT GOALS INTO CAPACITY (highest RICE first):
  Goal    RICE   Effort    Cumulative   Fits in period?
  ─────   ─────  ────────  ──────────   ───────────────
  G02     8,200  0.5 mo    0.5 mo       ✅ Q1
  G01     6,400  1.0 mo    1.5 mo       ✅ Q1
  G03     5,100  0.5 mo    2.0 mo       ✅ Q1
  G04     4,800  2.0 mo    4.0 mo       ⚠️ Starts Q1, completes Q2
  G05     3,200  1.5 mo    5.5 mo       ✅ Q2 (after G04)

STOP when cumulative effort exceeds available capacity.
Goals below the line go to the Next or Later bucket.
```

### Phase 4: Timeline Allocation

Place goals on the Now / Next / Later framework.
This is the most practical format for product teams — avoids false precision
of exact dates while still communicating sequence clearly.

```
NOW (current quarter / next 1-3 months):
  Goals the team is actively working on or starting immediately.
  These are commitments — the team is accountable for these.
  Keep this short: 2-4 goals maximum.

NEXT (following 1-2 quarters):
  Goals with high confidence we'll pursue after NOW is done.
  These are direction, not commitment.
  Subject to change based on what NOW teaches us.

LATER (6+ months / unscheduled):
  Goals that are worth doing eventually but not yet sequenced.
  Not a dumping ground — everything here should have passed goal-evaluator.
  Review quarterly: does anything here need to move up or drop?

DROPPED / NOT DOING:
  Goals explicitly decided against.
  Visible in the roadmap — shows stakeholders why things didn't make the cut.
  Prevents re-litigating the same decisions every quarter.
```

### Phase 5: Stakeholder Communication

Different audiences need different views of the same roadmap.
See `references/roadmap-formats.md` for templates.

```
EXECUTIVE / LEADERSHIP VIEW:
  Goals and outcomes, not features.
  "We're increasing user retention" not "We're building push notifications"
  Timeline: quarters, not weeks
  Detail: one sentence per item + expected outcome

PRODUCT / ENGINEERING VIEW:
  Goals with scope, dependencies, and sequence.
  Timeline: months / sprints
  Detail: goal + key features + owner + dependencies + success metric

CUSTOMER / PUBLIC VIEW (if applicable):
  What benefits are coming, not how they're being built.
  No internal initiatives or infrastructure work.
  Dates are ranges ("Coming Q3") or explicit ("Available July 2025")
  Never commit to exact dates unless certain.
```

### Phase 6: Roadmap Maintenance

A roadmap that isn't updated becomes a lie.

```
REVIEW TRIGGERS (update the roadmap when):
  □ A NOW goal is completed → pull next goal from Next bucket
  □ A goal's scope changes significantly → re-score and re-sequence
  □ New information invalidates an assumption → re-evaluate affected goals
  □ A new high-priority goal emerges → score it; decide what it displaces
  □ End of quarter → quarterly retrospective + re-plan

QUARTERLY REVIEW PROCESS (2-3 hours):
  1. Review NOW goals: what was completed? What wasn't? Why?
  2. Run retrospective on planning accuracy (was RICE useful? were estimates right?)
  3. Re-score all NEXT and LATER goals with new information
  4. Pull top goals from NEXT into new NOW bucket
  5. Update stakeholder views
  6. Communicate changes with rationale

RE-PRIORITIZATION RULES:
  Changing the roadmap mid-quarter is expensive.
  New goal entering NOW must displace something already there.
  The question is never "should we add X?" but "what does X replace?"
  If nothing can be displaced → X goes to NEXT bucket.
```

---

## Roadmap Health Checks

```
GOOD ROADMAP SIGNALS:
  □ Team can explain the order of goals without looking at the document
  □ Each goal has a clear owner
  □ NOW bucket has ≤4 goals
  □ Every goal on the roadmap has passed goal-evaluator (or equivalent)
  □ Stakeholders know which view to look at for their needs
  □ Last updated within the last 30 days

BAD ROADMAP SIGNALS:
  🚩 NOW bucket has 10+ goals (not a roadmap — a wishlist)
  🚩 No owner named for any goal
  🚩 "Later" bucket hasn't changed in 6 months (stale backlog)
  🚩 Roadmap and sprint board tell different stories
  🚩 Dates are specific (day/month) more than 2 months out
  🚩 Stakeholders are surprised by what's in NOW
```

---

## Connections to Other Skills

```
BEFORE roadmap-planning:
  goal-evaluator     → score individual goals (EV, reversibility, timing)
  project-compass    → identify what to Kill/Keep/Grow/Start

AFTER roadmap-planning:
  project-planner    → execute the top goal from NOW bucket
  working-standards  → standards for execution work
  release-management → release completed goals to users

ALONGSIDE:
  tenth-man          → challenge roadmap assumptions ("what are we missing?")
  project-monitor    → track whether shipped goals are achieving outcomes
  retrospective      → review planning accuracy at end of quarter
```

---

## Reference Files

- `references/prioritization-methods.md` — RICE, ICE, MoSCoW, and when to use each
- `references/roadmap-formats.md` — Templates for each stakeholder view
