---
name: incident-response
description: >
  Respond to production incidents — outages, degradations, data issues, or
  security events — with a structured process that minimizes impact, coordinates
  the team, and drives to resolution. Use this skill when something in production
  is broken, degraded, or behaving unexpectedly and users or stakeholders are
  affected. Trigger phrases: "production is down", "something is wrong in prod",
  "users are reporting errors", "the site is slow", "we have an incident",
  "something broke after the deployment", "critical bug in production",
  "how do we respond to this?", "who does what during an incident".
  This skill handles the CRISIS — project-monitor detects the problem,
  debugging-rootcause finds the cause, release-management deploys the fix.
---

# Incident Response Skill

Manages production incidents from detection to resolution.
During a crisis, the biggest cost is confusion — who does what, in what order,
with what information. This skill provides the structure to minimize that cost.

## Core Principle

> In an incident, speed of communication matters as much as speed of fix.
> Affected users and stakeholders need to know what's happening.
> The team needs to know who is in charge and what each person should do.
> Contain first. Diagnose second. Fix third. Learn last.

---

## Incident Severity Levels

Classify first. Response scales with severity.

```
SEV-1 — CRITICAL (wake people up)
  Complete outage or data loss affecting all or most users.
  Immediate financial, legal, or reputational impact.
  Examples: site down, payment system failing, data breach.
  Response time: immediate. All hands available.

SEV-2 — HIGH (respond within 30 minutes)
  Major feature broken, significant degradation, subset of users affected.
  Examples: checkout broken, login failing for 20% of users.
  Response time: 30 minutes. Core team responds.

SEV-3 — MEDIUM (respond within 2 hours)
  Non-critical feature broken, minor degradation, workaround exists.
  Examples: export function failing, search slow but working.
  Response time: 2 hours during business hours.

SEV-4 — LOW (next business day)
  Minor issue, no user-facing impact, or cosmetic problem.
  Response time: planned work, no urgency.
```

---

## Response Roles

Assign these roles at the start of every SEV-1/SEV-2 incident:

```
INCIDENT COMMANDER (IC) — one person only
  Owns the incident. Makes decisions when team disagrees.
  Does NOT debug — coordinates others doing the debugging.
  Keeps the timeline. Drives communication.
  Declares resolution.

TECHNICAL LEAD (TL) — one person
  Leads the diagnosis and fix.
  Reports findings to IC.
  Manages the technical work.

COMMUNICATIONS LEAD (CL) — one person
  Handles external and internal communication.
  Updates status page, sends stakeholder messages.
  Frees IC and TL to focus on the problem.

SCRIBE — one person (can be IC if small team)
  Documents everything in real time:
    - What was tried and when
    - What was found and when
    - Decisions made and why
  The post-incident review depends on this record.
```

---

## Response Workflow

### Step 1 — Declare and Classify (first 5 minutes)

```
□ Is this actually an incident? (not a false alarm / monitoring bug?)
□ Assign severity level (SEV-1 through SEV-4)
□ Assign roles (IC, TL, CL, Scribe)
□ Open incident channel / war room
□ Scribe begins timeline log

TIMELINE LOG ENTRY FORMAT:
  [HH:MM] <who> — <what was observed or done>
  Example:
  [14:23] @alex — Users reporting 500 errors on checkout. Confirming in logs.
  [14:25] @sam  — Confirmed: error rate 45% on /api/orders. SEV-1 declared.
  [14:26] @alex — IC: alex, TL: sam, CL: jordan. War room: #incident-2025-08-14
```

### Step 2 — Communicate (first 10 minutes)

**Do not wait until you know the cause to communicate.**

```
INTERNAL (immediately):
  Notify: engineering leadership, customer success, support team.
  Message: "We have a SEV-[N] incident. [What is affected]. We are investigating.
            Next update in [15/30] minutes."

EXTERNAL (within 10 minutes for SEV-1/2):
  Update status page: "We are investigating reports of [symptoms]."
  Do NOT: speculate on cause in external communication.
  Do NOT: promise a fix time until you know what the fix is.

UPDATE CADENCE:
  SEV-1: update every 15 minutes, even if "still investigating"
  SEV-2: update every 30 minutes
  SEV-3/4: update when there is meaningful progress
```

### Step 3 — Contain (before diagnosing)

**Limit the damage before finding the cause.**

```
CONTAINMENT OPTIONS (apply the fastest one that works):
  □ Rollback last deployment (if incident followed a deploy)
  □ Disable the broken feature (feature flag off)
  □ Route traffic away from affected component
  □ Rate limit or block traffic causing the problem
  □ Scale up resources (if resource exhaustion is the cause)
  □ Failover to backup system

CONTAINMENT DECISION:
  "Can we reduce user impact right now, even before knowing the root cause?"
  YES → contain first, then diagnose on a contained system
  NO  → proceed directly to diagnosis

IMPORTANT: containment is not the fix. It buys time to diagnose safely.
```

### Step 4 — Diagnose (use debugging-rootcause skill)

```
While TL diagnoses:
  □ IC manages communication cadence
  □ CL sends updates on schedule
  □ Scribe logs everything

TL applies debugging-rootcause:
  □ Gather evidence (logs, metrics, traces)
  □ Form and test one hypothesis at a time
  □ Find root cause before proposing fix
```

### Step 5 — Fix and Verify

```
□ Fix addresses the root cause (not just the symptom)
□ Fix tested in staging if possible (even briefly)
□ Deployment follows release-management process
□ Verify: monitor for 15-30 minutes post-fix before declaring resolved
□ Confirm: error rate returns to baseline, user reports stop
```

### Step 6 — Declare Resolution

```
IC declares resolution when:
  □ Error rates have returned to normal for 15+ minutes
  □ Affected features are confirmed working
  □ No new user reports of the same issue

RESOLUTION COMMUNICATION:
  Internal: "Incident resolved at [time]. Root cause: [brief]. Post-mortem: [date]."
  External (status page): "This incident has been resolved. We will publish a
                           post-incident review within [24-48] hours."
```

---

## Post-Incident Review (PIR)

Run within 48 hours of resolution. Blameless. Focus on systems, not people.

```
PIR AGENDA (60 minutes):
  1. Timeline review (15 min): walk through what happened, minute by minute
  2. Impact assessment (5 min): how many users? how long? what was lost?
  3. What went well (10 min): what parts of the response worked?
  4. What went poorly (10 min): what slowed us down or made things worse?
  5. Root cause deep dive (10 min): 5 Whys on the technical cause
  6. Action items (10 min): specific, owned, time-bounded improvements

PIR OUTPUT FORMAT:
  Date/time of incident:
  Duration:
  Severity:
  Impact summary:
  Root cause:
  Timeline: [detailed log from scribe]
  What went well:
  What could be improved:
  Action items:
    □ <action> — Owner: <name> — Due: <date>
    □ <action> — Owner: <name> — Due: <date>
```

---

## Incident Communication Templates

See `references/communication-templates.md` for ready-to-use templates.

---

## Reference Files

- `references/communication-templates.md` — Templates for every incident communication type
