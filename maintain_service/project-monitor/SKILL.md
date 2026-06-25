---
name: project-monitor
description: >
  Track whether a project or product is achieving its goals after release,
  using defined KPIs, health signals, and structured review cadences. Use this
  skill after a project or feature has been released to monitor ongoing success,
  detect when outcomes are not meeting targets, and decide when to escalate to
  project-compass for a direction change. Trigger phrases: "track project health",
  "is this working?", "set up KPIs for this project", "monitor outcomes",
  "the project shipped but we don't know if it's working", "define success metrics",
  "review how the project is performing", "is the product achieving what we hoped?",
  "what should we be measuring post-launch?", "something feels off with the results".
  This skill tracks PROJECT and PRODUCT outcomes — business results, user outcomes,
  and goal achievement. Not technical infrastructure or AI system behavior.
---

# Project Monitor Skill

Tracks whether released work is achieving its intended outcomes.
Shipping is not success. Success is when the intended outcome is happening.

## Core Principle

> A released project that no one is measuring is a guess, not a result.
> Define what success looks like before you need to defend it.
> Measure it continuously so you know when something changes.

---

## Three Types of Project Metrics

Every project should track metrics at three levels:

```
OUTCOME METRICS — Is the project achieving its goal?
  These are the metrics from goal-evaluator's success definition.
  If the goal was "increase response rate to 50%", this is the response rate.
  These matter most. Everything else is in service of these.

LEADING INDICATORS — Early signals that predict outcome metrics
  Outcomes often take time to appear. Leading indicators give earlier signal.
  Example: If outcome = revenue growth, leading indicator = new trial signups.
  Track these weekly; outcomes may only be visible monthly.

HEALTH SIGNALS — Is the project or product operating normally?
  Not the goal itself, but the conditions that allow the goal to be achieved.
  Example: uptime, error rate, user retention, task completion rate.
  These alert you when something breaks before outcomes are affected.
```

---

## Setup Workflow (Run Once, Post-Release)

### Step 1 — Extract Success Metrics from goal-evaluator

The success definition and exit criteria from goal-evaluator define what to measure:

```
FROM GOAL-EVALUATOR:
  Success looks like:     → this becomes the primary outcome metric
  Stop/abandon if:        → this becomes the minimum acceptable threshold
  Exit condition:         → this becomes the alert trigger
  Minimum viable signal:  → this becomes the first leading indicator to watch
```

If goal-evaluator wasn't used, define metrics now before proceeding.

### Step 2 — Define the Metric Stack

For each level, define at least 1 metric:

```
OUTCOME METRICS (define 1-3):
  Metric:           <what we're measuring>
  Target:           <what success looks like>
  Minimum:          <below this = project is failing>
  Measurement:      <how and how often>
  Current baseline: <where it is now, pre-release or at release>

LEADING INDICATORS (define 2-4):
  Indicator:        <early signal>
  Why it predicts:  <how this connects to the outcome>
  Target:           <what we expect to see if on track>
  Measurement:      <how often>

HEALTH SIGNALS (define 2-5):
  Signal:           <operational health metric>
  Normal range:     <acceptable range>
  Alert threshold:  <when to investigate>
  Measurement:      <how often — usually automated>
```

### Step 3 — Set Review Cadence

```
WEEKLY (15-30 minutes):
  □ Check leading indicators — are we on track toward outcome?
  □ Review health signals — anything abnormal?
  □ Log results in tracking record
  □ Any action needed? (if yes → define it and own it)

MONTHLY (1 hour):
  □ Review outcome metrics — are we hitting targets?
  □ Compare to baseline from release — direction of travel?
  □ Review weekly trends — any slow-building patterns?
  □ Decision: stay the course / adjust / escalate to project-compass?

QUARTERLY (as needed):
  □ Is the success definition still the right one?
  □ Have targets been met? Should they be revised upward?
  □ Is this project still the right investment? (refer to goal-evaluator)
```

---

## Ongoing Monitoring Workflow

### When Metrics Are on Track

```
All outcome metrics heading toward target.
Leading indicators positive.
Health signals in normal range.

→ Maintain cadence. Log results. No action required.
→ If consistently exceeding targets: consider raising the bar or reducing oversight.
```

### When a Leading Indicator Lags

```
Outcome metrics not yet affected, but early signals are soft.

STEP 1 — Confirm it's a real signal (not noise or measurement error)
  □ Has the measurement method changed?
  □ Is this one week of softness or a trend?

STEP 2 — Investigate the cause
  □ What changed recently? (release, external event, team change)
  □ Is this affecting all user segments or a specific one?

STEP 3 — Decide
  □ Likely to self-correct → monitor closely for 1 more week
  □ Likely real → act now before outcome metrics are affected
  □ Unsure → run tenth-man to surface what might be missed
```

### When an Outcome Metric Falls Below Target

```
STEP 1 — Assess severity
  □ How far below target? (5% miss vs 30% miss are different situations)
  □ Is this a point-in-time miss or a trend?
  □ Is the minimum acceptable threshold breached?
    YES → escalate immediately; this triggers the exit condition from goal-evaluator
    NO  → investigate before escalating

STEP 2 — Diagnose
  □ Which leading indicators preceded this drop?
  □ Did anything change in the project, product, or external environment?
  □ Is this a quality issue → run work-audit
  □ Is this a scope issue → run project-qa
  □ Is this a strategic issue → run project-compass

STEP 3 — Act
  Fixable within current scope → define fix; assign owner; set timeline
  Requires direction change → run project-compass
  Outcome is unachievable with current approach → return to goal-evaluator
```

### When a Health Signal Breaches Alert Threshold

```
STEP 1 — Isolate the affected area
  □ Which component, feature, or user segment is affected?
  □ When did it start?

STEP 2 — Assess impact on outcomes
  □ Are outcome metrics also affected?
  □ How long until they will be if this continues?

STEP 3 — Fix or escalate
  □ Technical issue → fix; release fix using release-management
  □ Quality issue → run work-audit on relevant deliverable
  □ Systemic issue → run project-compass
```

---

## Monthly Review Template

```
╔══════════════════════════════════════════════════════════════╗
║              PROJECT HEALTH REVIEW — <Month Year>            ║
╚══════════════════════════════════════════════════════════════╝

PROJECT: <name>
REVIEW DATE: <date>
REVIEWED BY: <name>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OUTCOME METRICS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Metric              Target   This Month   Baseline   Trend
  ─────────────────   ──────   ──────────   ────────   ──────────
  <metric>            <X>      <X>          <X>        [▲ UP | ▼ DOWN | → FLAT]
  <metric>            <X>      <X>          <X>        [▲ | ▼ | →]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LEADING INDICATORS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Indicator           Expected   Actual     Status
  ─────────────────   ────────   ──────     ──────────────────
  <indicator>         <X>        <X>        [ON TRACK | SOFT | BEHIND]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HEALTH SIGNALS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Signal              Normal Range   This Month   Status
  ─────────────────   ────────────   ──────────   ──────────────────
  <signal>            <range>        <X>          [NORMAL | WATCH | ALERT]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ASSESSMENT & ACTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Overall status:  [ON TRACK | NEEDS ATTENTION | OFF TRACK | ESCALATE]

Key finding:     <the single most important observation this month>

Actions:
  □ <action> — Owner: <name> — By: <date>
  □ <action> — Owner: <name> — By: <date>

Escalation needed?
  □ To project-compass:   [YES if direction change needed | NO]
  □ To work-audit:        [YES if quality issue suspected | NO]
  □ To goal-evaluator:    [YES if original goal needs revalidation | NO]

╔══════════════════════════════════════════════════════════════╗
║  STATUS: [ON TRACK | NEEDS ATTENTION | OFF TRACK | ESCALATE] ║
╚══════════════════════════════════════════════════════════════╝
```

---

## Reference Files

- `references/metrics-by-project-type.md` — Common metrics for different project types
