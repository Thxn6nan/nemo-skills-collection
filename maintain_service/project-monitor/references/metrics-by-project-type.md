# Metrics by Project Type

Common metric stacks for different types of projects.
Adapt to your specific success definition — these are starting points.

---

## Product / Feature Project

```
OUTCOME METRICS
  Adoption rate:       % of target users actively using the feature
  Goal completion:     % of users achieving the intended outcome
  Retention impact:    change in user retention vs. control group

LEADING INDICATORS
  Activation rate:     % of new users who try the feature in first week
  Return usage:        % of users who use it more than once
  Support ticket rate: tickets about this feature (rising = friction)

HEALTH SIGNALS
  Error rate:          % of feature interactions that produce errors
  Load time:           P95 response time for feature-related actions
  Completion rate:     % of feature flows completed vs. abandoned
```

---

## Research / Analysis Project

```
OUTCOME METRICS
  Decision made:       Was the research used to make the intended decision?
  Decision quality:    Did the decision prove correct? (lagging, review after 90 days)
  Stakeholder confidence: Did the research increase confidence in the decision?

LEADING INDICATORS
  Engagement:          Did stakeholders read and engage with the output?
  Questions raised:    Are questions being answered vs. generating more confusion?
  Reuse:               Is the research being referenced in other work?

HEALTH SIGNALS
  Delivery on time:    Was it delivered when needed for the decision?
  Accuracy:            Have any factual errors been found post-delivery?
```

---

## Process Improvement Project

```
OUTCOME METRICS
  Time saved:          Actual time reduction vs. baseline
  Error rate change:   Change in errors or rework vs. baseline
  Adoption:            % of team following the new process

LEADING INDICATORS
  Training completion: % of affected people who've been trained
  Early adoption:      % following new process in first 2 weeks
  Compliance rate:     % of work that follows the new process (sampled)

HEALTH SIGNALS
  Workaround rate:     % of people reverting to old process ("shadow behavior")
  Support requests:    Questions about the new process (rising = confusion)
```

---

## Technical / Infrastructure Project

```
OUTCOME METRICS
  Reliability improvement:   Reduction in incidents or downtime vs. baseline
  Performance improvement:   Improvement in key latency or throughput metrics
  Developer velocity:        Change in deployment frequency or lead time

LEADING INDICATORS
  Test coverage:      % of new surface area covered by tests
  Incident rate:      Incidents per week (should decrease over time)
  Deploy frequency:   How often the team can safely deploy changes

HEALTH SIGNALS
  Build success rate: % of builds passing
  Rollback rate:      % of deployments requiring rollback
  Debt ratio:         % of work that is maintenance vs. new capability
```

---

## Client / Stakeholder Deliverable Project

```
OUTCOME METRICS
  Acceptance:         Was the deliverable accepted without major revision?
  Outcome achieved:   Did the client achieve the goal the deliverable was for?
  Relationship:       Is the client satisfied and likely to continue working together?

LEADING INDICATORS
  Feedback speed:     How quickly did client respond? (slow = low engagement)
  Revision count:     Number of revisions requested (fewer = better alignment)
  Questions asked:    Are questions clarifying or questioning the work?

HEALTH SIGNALS
  On-time delivery:   Was it delivered when promised?
  Scope alignment:    Did delivered work match agreed scope? (from project-qa)
  Quality score:      work-audit score on delivered material
```

---

## How to Choose the Right Metrics

```
STEP 1 — Start with the goal-evaluator success definition
  "Success looks like: <X>"
  This is your primary outcome metric. Measure X directly.

STEP 2 — Ask "what predicts X?"
  These become your leading indicators.
  They should be measurable before X is visible.

STEP 3 — Ask "what must be working for X to be achievable?"
  These become your health signals.
  They're the floor — X can't improve if these are broken.

STEP 4 — Verify measurability
  For each metric: can you actually measure it, with the data you have?
  If not: either find the data, change the metric, or note it as a gap.

STEP 5 — Set targets and thresholds before releasing
  Targets: set from the goal-evaluator ROI calculation
  Minimum thresholds: set from the exit criteria
  Never set targets after seeing the first results — that's working backward.
```
