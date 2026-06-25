# Prioritization Methods

---

## Method 1: RICE (Recommended for most product goals)

**Best for:** Feature and initiative prioritization where you have usage data

```
RICE = (Reach × Impact × Confidence) / Effort

REACH — users affected per period
  Be specific: "8,000 active users per month" not "all users"
  Source: analytics, sales data, user research

IMPACT — per-user impact (use fixed scale):
  3.0 = massive     (core workflow; users will notice significantly)
  2.0 = significant (meaningful improvement to common task)
  1.0 = moderate    (noticeable but not critical)
  0.5 = minimal     (nice to have, rarely used)
  0.25 = trivial    (cosmetic, edge case)

CONFIDENCE — how certain are the above estimates?
  100% = proven by data or research
  80%  = good evidence, some assumptions
  50%  = mostly assumptions, limited evidence
  Never use below 50% — if confidence is that low, validate first

EFFORT — person-months to ship
  Be realistic. Apply the 1.5× planning fallacy multiplier.
  Include: design + development + testing + release

INTERPRETING RICE SCORES:
  Compare scores within the same context (same team, same period)
  RICE scores cannot be compared across different teams or products
  A score of 5,000 is "better" than 2,000 only relative to each other
```

---

## Method 2: ICE (Fast, lightweight scoring)

**Best for:** Early-stage brainstorming, rapid prioritization with limited data

```
ICE = Impact × Confidence × Ease

Each dimension scored 1-10:
  Impact:     How much value does this create if it works?
  Confidence: How confident are we it will work as expected?
  Ease:       How easy/fast is this to implement? (inverse of effort)

SCORE GUIDE:
  ICE > 500 = strong candidate
  ICE 200-499 = moderate candidate
  ICE < 200 = low priority or needs more validation

LIMITATION:
  Very subjective without data. Use for rough sorting only.
  Graduate to RICE once you have usage data.
```

---

## Method 3: MoSCoW (For requirement and scope decisions)

**Best for:** Defining MVP scope, managing stakeholder expectations on features

```
MUST HAVE:
  Without this, the product fails its core purpose.
  Non-negotiable. Release is blocked without it.
  Rule: if more than 60% of items are "Must Have" → you've lost the framework.

SHOULD HAVE:
  High value, expected by users, but not blocking release.
  These are the first candidates to cut if scope must shrink.

COULD HAVE:
  Nice to have. Included if time allows. Cut first under pressure.

WON'T HAVE (this time):
  Explicitly out of scope for this release.
  Documenting "Won't Have" prevents scope creep during development.
  "Won't Have this time" — may be revisited in future releases.

USAGE IN ROADMAP:
  Use MoSCoW within a goal (to scope what's in the first version)
  Use RICE across goals (to decide which goals to pursue)
  They solve different problems.
```

---

## Method 4: Opportunity Scoring (for user-need prioritization)

**Best for:** Understanding which user needs are most underserved

```
Based on Tony Ulwick's Outcome-Driven Innovation framework.

For each user need or job-to-be-done:

IMPORTANCE: How important is this outcome to users?
  Survey: "How important is it to you that you can [outcome]?"
  Scale: 1-10

SATISFACTION: How satisfied are users with current solutions?
  Survey: "How satisfied are you with your current ability to [outcome]?"
  Scale: 1-10

OPPORTUNITY SCORE = Importance + MAX(Importance - Satisfaction, 0)

INTERPRETATION:
  Score > 15: Highly underserved need → strong opportunity
  Score 10-15: Moderately underserved → worth considering
  Score < 10: Adequately served → low opportunity

WHY THIS IS USEFUL:
  Identifies where users struggle most, not what they ask for most.
  Users often ask for features (solutions) not outcomes (needs).
  This method finds needs with high importance but low satisfaction.
```

---

## Choosing the Right Method

```
SITUATION                              RECOMMENDED METHOD
──────────────────────────────────     ──────────────────
You have usage data and clear goals    RICE
Fast sorting, limited data             ICE
Scoping a single release               MoSCoW
Understanding which user needs to      Opportunity Scoring
  address

COMBINING METHODS:
  1. Opportunity Scoring → find which user needs are most underserved
  2. Brainstorm goals that address top opportunities
  3. RICE → prioritize goals against each other
  4. MoSCoW → scope the first version of each goal
```

---

## Dealing with Prioritization Politics

```
COMMON CHALLENGE:
  "The CEO wants X on the roadmap regardless of RICE score"
  "Sales says we'll lose the deal without Y"
  "Legal says Z is mandatory"

FRAMEWORK RESPONSE:
  Mandatory items (legal, compliance, contractual) → treat as constraints,
  not competing priorities. They go in; RICE is for the remaining capacity.

  Strategic bets (CEO directive, market timing) → add a "Strategic Weight"
  multiplier to RICE. Document explicitly that you're overriding the model.
  "We're doing X not because RICE says so, but because [strategic reason]."
  This makes the trade-off visible rather than hiding it.

  Sales-requested items → run through RICE with Reach = affected accounts × ARR.
  Often scores higher than teams expect when revenue impact is included.

RULE:
  Override the framework transparently, not silently.
  "We chose X over Y despite lower RICE because [reason]" is honest.
  Pretending X scores higher than it does is not.
```
