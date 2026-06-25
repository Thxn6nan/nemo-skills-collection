---
name: goal-evaluator
description: >
  Evaluate whether a goal, feature, project, or investment is worth pursuing —
  before committing resources to it. Covers ROI, opportunity cost, timing,
  reversibility, strategic fit, bias correction, and exit criteria. Use this skill
  when deciding whether to start something new, when a goal feels compelling but
  unclear, when you need to justify a commitment, or when you suspect the real
  cost or value hasn't been honestly assessed. Trigger phrases: "is this worth
  doing", "should we pursue this", "evaluate this goal", "is the ROI good enough",
  "is the scope too small to bother", "help us decide if this is worth it",
  "we're thinking of doing X — is it a good idea", "is this the right thing to
  focus on", "assess this opportunity", "should we invest in this".
  Outputs one of five verdicts: PURSUE / REFINE / DEFER / DESCOPE / DROP.
  Use before project-compass (direction) and working-standards (execution).
---

# Goal Evaluator Skill

Answers one question before resources are committed:

> **Is this goal worth pursuing — now, at this scope, by this team?**

Standard ROI misses too much: vague goals that can't be evaluated, optimism
bias that inflates value and deflates cost, opportunity cost of what won't
get done, and timing that determines whether even a good goal succeeds.
This skill covers all of it.

---

## Five Possible Verdicts

Every evaluation ends with one of these:

```
PURSUE   ── Conditions are met. Start now with confidence.
REFINE   ── The idea has merit but the goal is too vague to evaluate or execute.
             Clarify first, then re-evaluate.
DEFER    ── Worth doing, but not now. A specific trigger or condition must
             be met first. Set a review date.
DESCOPE  ── The full scope isn't justified, but a smaller version is.
             Define the smaller version and pursue that.
DROP     ── The goal does not justify the investment at this time.
             Explain why and what to consider instead.
```

---

## Evaluation Workflow — 7 Phases

### Phase 1: Clarity Test

**Before evaluating anything, confirm the goal is clear enough to evaluate.**

A goal that cannot be made specific cannot be honestly assessed.
Vagueness is not a sign of ambition — it is a sign that the thinking
isn't done yet.

Ask:
```
□ Can you state in one sentence what "success" looks like?
□ Is there a measurable condition that marks the goal as complete?
□ Do all stakeholders agree on what the goal means?
□ Can you distinguish "done" from "better than before"?
```

If any answer is NO → **output REFINE immediately**.
Do not proceed through the remaining phases on a vague goal.
Provide 3–5 clarification questions that, when answered, would allow
full evaluation.

See `references/clarity-questions.md` embedded in the value framework.

---

### Phase 2: Value Analysis

**What do you actually get if this succeeds?**

Map value across four layers — goals often have value at one layer
but not others, which changes the assessment:

```
DIRECT VALUE — What immediately improves?
  Quantify where possible: "saves X hours/week", "reduces Y by Z%"
  If value is qualitative only → flag as hard to verify

INDIRECT VALUE — What else becomes possible because of this?
  Does success unlock other goals?
  Does it create capability that compounds over time?
  Does it change what the team/product can do?

STRATEGIC VALUE — Does this advance the direction we want to go?
  Even if direct value is modest, is this a stepping stone?
  Does it build skills, data, or position that matters later?

RISK OF NOT DOING IT — What happens if we skip this entirely?
  Is this defensive (prevents loss) or offensive (creates gain)?
  What's the realistic cost of inaction?
```

**Value ceiling:**
What is the maximum realistic value if everything goes well?
This is not the optimistic case — it is the best realistic case.

See `references/value-cost-framework.md` for value estimation methods.

---

### Phase 3: Cost Analysis

**What does this actually cost — fully loaded?**

Most cost estimates miss at least one of these:

```
RESOURCE COST — Direct investment required
  Time: how many person-hours/weeks/months?
  Money: direct spend, tooling, infrastructure?
  People: who specifically, and what else won't they do?

OPPORTUNITY COST — What will NOT happen because of this?
  List the top 2–3 things that get deprioritized or dropped.
  What is their combined value?
  Is this goal more valuable than those combined?

RISK COST — The expected cost of failure
  Risk cost = probability of failure × cost of failing
  Include: wasted resources + morale cost + delay of alternatives

REVERSAL COST — How expensive is being wrong?
  If this turns out to be the wrong goal after 4 weeks:
    What has been spent that can't be recovered?
    What has been changed that's hard to undo?
  Low reversal cost → lower bar to proceed
  High reversal cost → higher bar required before committing
```

**Total realistic cost = Resource + Opportunity + Risk + Reversal**

Most evaluations use only Resource cost and underestimate by 2–3×.

---

### Phase 4: Probability Assessment

**What is the realistic chance this succeeds?**

This is the most systematically underestimated input in any ROI calculation.

```
STEP 1 — Base rate check
  What percentage of similar goals at similar organizations succeed?
  Use this as the starting point, not your team's optimism.

  General base rates (adjust for context):
    Well-defined technical task, known solution path:  70–85%
    Feature with clear requirements, known domain:      50–70%
    Novel capability, uncertain solution:               30–50%
    Strategic initiative, depends on adoption:         20–40%
    Exploratory goal, unclear success criteria:        10–30%

STEP 2 — Adjust for team-specific factors
  Factors that raise probability:
    ✦ Team has done this exact type of work before
    ✦ Strong early evidence (prototype, user validation)
    ✦ All required skills exist in the team
    ✦ Low dependency on external decisions

  Factors that lower probability:
    ✦ Goal requires skills the team doesn't have
    ✦ Depends on 3+ external factors outside team control
    ✦ Similar goals have been attempted and abandoned before
    ✦ No clear owner

STEP 3 — Pre-mortem
  Imagine it is 6 months from now and this goal failed.
  Write the most likely reason it failed. Then ask:
  "Is that reason present right now?"
  If YES → the base probability should be reduced by 15–30%.
```

See `references/bias-correction-guide.md` for full pre-mortem protocol.

---

### Phase 5: Expected Value Calculation

**The math that makes the trade-off explicit.**

```
EXPECTED VALUE (EV) = (Probability of Success × Value if Successful)
                    − Total Cost
                    − (Probability of Failure × Reversal Cost)

Example:
  Value if successful:    80 units
  Probability:            55%
  Total cost:             30 units
  Reversal cost:          10 units
  Probability of failure: 45%

  EV = (0.55 × 80) − 30 − (0.45 × 10)
     = 44 − 30 − 4.5
     = 9.5 units

  Positive EV → pursue (if opportunity cost is also favorable)
  Negative EV → drop or descope
  Near-zero EV → compare against alternatives before deciding
```

**Opportunity cost adjustment:**
Subtract the EV of the best alternative that won't be pursued.
If (EV of this goal) < (EV of best alternative) → even positive EV
goals can be the wrong choice.

```
  Adjusted EV = EV of this goal − EV of best forgone alternative
  
  Adjusted EV > 0 → this goal beats the alternative
  Adjusted EV < 0 → the alternative is the better use of resources
```

---

### Phase 6: Bias Correction

**Challenge the estimates before accepting them.**

Human judgment on goals is systematically biased. Apply these corrections:

```
OPTIMISM BIAS CHECK — Value inflation
  "If someone else proposed this goal and estimated this value,
   would you accept that estimate?"
  Common inflation patterns:
    - Assuming best-case adoption
    - Ignoring maintenance cost after launch
    - Double-counting value across layers
    - Assuming the problem is as bad as the most vocal complainers suggest
  
  Correction: Identify the one assumption that, if wrong,
  would most reduce the value estimate. How likely is it wrong?

PLANNING FALLACY CHECK — Cost deflation
  "When was the last time a goal like this came in at or under estimate?"
  If rarely → apply a multiplier to resource cost:
    Simple, well-understood work: ×1.2
    Moderate complexity: ×1.5
    Novel or cross-team work: ×2.0

SUNK COST CHECK — Past investment distorting judgment
  "If we had not started anything on this yet, would we still pursue it?"
  If NO → sunk cost is inflating the case for continuing.
  Past investment is not a reason to continue future investment.

MOTIVATED REASONING CHECK — Wanting the answer
  "Who proposed this goal, and do they benefit from it being approved?"
  "Have we genuinely considered the DROP verdict, or are we looking
   for reasons to PURSUE?"
  If the evaluation feels like it's building a case rather than
  finding the truth → run the pre-mortem again from a skeptical angle.
```

See `references/bias-correction-guide.md` for full correction protocols.

---

### Phase 7: Strategic Fit + Timing + Exit Criteria

**Three final checks before the verdict.**

**Strategic Fit:**
```
□ Does pursuing this move us toward the direction we've committed to?
□ Does this build capability we'll use again, or is it one-off?
□ If this succeeds, does it make the next goal easier or harder?
□ Is this the best use of our resources toward our stated direction?

If strategic fit is LOW but EV is HIGH → consider DESCOPE
(capture the value without investing in an off-direction capability)
```

**Timing:**
```
□ Why now rather than in 3 months?
  If there's no good answer → consider DEFER

□ Is there a forcing function (deadline, external event, window closing)?
  YES + genuine → valid reason to act now
  NO → timing is arbitrary; is that ok?

□ What changes in 3 months that makes this better or worse?
  Better conditions coming → DEFER
  Window closing → act now or lose the opportunity
  No change → timing is flexible; proceed on merit

□ What must be true before this is the right time?
  If conditions aren't met → DEFER with specific trigger defined
```

**Exit Criteria:**
```
Every goal needs a defined stopping condition — not just success,
but also the conditions under which we stop before success.

Define before starting:
  Success looks like:     <measurable outcome>
  Stop/abandon if:        <conditions that trigger stopping>
  Review checkpoint:      <date or milestone to reassess>
  Minimum viable signal:  <earliest evidence that validates or invalidates>
```

A goal without exit criteria runs until resources run out.

---

## Output: The Evaluation Report

See `references/evaluation-report-format.md` for full template.

```
VERDICT: [PURSUE | REFINE | DEFER | DESCOPE | DROP]

EV Summary:
  Value if successful:    <X>
  Probability:            <X>%
  Total cost:             <X>
  Adjusted EV:            <X> ([positive/negative])

Key finding: <the one thing that most determined this verdict>
Primary risk: <what most threatens success>
Exit condition: <when to stop if pursuing>
```

---

## How This Fits in the Skill Collection

```
goal-evaluator     ← Should we do this at all? (BEFORE committing)
project-compass    ← Where should this go? (DURING the project)
working-standards  ← How should we build this? (WHILE executing)
work-audit        ← Is the output correct? (AFTER producing)
tenth-man          ← What are we missing? (AT ANY POINT)
anti-hallucination ← Can we trust the numbers? (WHEN data is used)
```

---

## Example Invocations

```
"Evaluate whether we should build a caching layer for our API"
"Is it worth adding multi-language support to the product?"
"We're thinking of refactoring the auth module — is it worth it?"
"Should we pursue this partnership or focus internally?"
"Is this feature request worth the 3 weeks it would take?"
"We have 4 potential goals — help us decide which one to pursue"
"Is the scope of this too small to bother with?"
"Evaluate this business case before we present it"
```
