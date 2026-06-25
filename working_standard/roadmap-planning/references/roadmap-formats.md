# Roadmap Formats — Templates by Audience

---

## Format 1: Now / Next / Later (Master Roadmap)

The canonical format for product teams. Avoids false precision of dates
while communicating priority and sequence clearly.

```
╔══════════════════════════════════════════════════════════════════════╗
║                    PRODUCT ROADMAP — Q3 2025                         ║
║                    Last updated: <date>  Owner: <name>               ║
╚══════════════════════════════════════════════════════════════════════╝

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NOW  (July – September 2025)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
These are commitments. The team is actively working on these.

  🟢 [G01] Push Notifications
     Goal:    Increase message response rate from 34% to 50%
     Owner:   Engineering — Team A
     Status:  In Progress (Week 3 of 5)
     RICE:    6,400

  🟢 [G02] Bulk Export Feature
     Goal:    Reduce support tickets for data requests by 60%
     Owner:   Engineering — Team B
     Status:  Starting Week 4
     RICE:    5,100

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXT  (October – December 2025)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
High confidence we'll pursue these after NOW completes.
Subject to change based on what we learn.

  🔵 [G03] Advanced Filtering
     Goal:    Enable power users to find content faster
     RICE:    4,800
     Blocker: Requires G01 infrastructure

  🔵 [G04] API Rate Limit Dashboard
     Goal:    Reduce developer support tickets by 40%
     RICE:    4,200
     Note:    Scope TBD pending user interviews in August

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LATER  (2026 and beyond)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Worth doing. Not yet sequenced. Reviewed quarterly.

  ⚪ [G05] White-label Support         RICE: 3,800
  ⚪ [G06] Mobile App                  RICE: 3,200
  ⚪ [G07] AI-Assisted Drafting        RICE: 2,900

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NOT DOING (decided against)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Explicitly declined. Prevents re-litigating.

  ❌ [G08] Windows Desktop App
     Reason: Web app meets needs; mobile is higher priority (G06)
     
  ❌ [G09] Built-in Video Calling
     Reason: Integrations with Zoom/Meet serve this need; build cost not justified
```

---

## Format 2: Executive / Leadership View

One page. Outcomes, not features. Quarters, not months.

```
PRODUCT DIRECTION — 2025

THEME: Increase user retention and reduce support burden

Q1 2025 (COMPLETED):
  ✅ Improved onboarding → 23% increase in 30-day activation
  ✅ Performance improvements → P95 latency reduced from 800ms to 180ms

Q2 2025 (COMPLETED):
  ✅ Team collaboration features → 340 new team accounts
  ⚠️ Search redesign → delayed to Q3 (scope was larger than estimated)

Q3 2025 (IN PROGRESS):
  → Notifications & engagement (expected: +16pp response rate)
  → Self-serve data export (expected: -60% data-related support tickets)

Q4 2025 (PLANNED):
  → Power user tools (filtering, bulk actions)
  → Developer experience improvements

2026 DIRECTION:
  → Mobile presence
  → Platform / API ecosystem
  → [Strategic initiative under evaluation]

KEY RISKS:
  → Search redesign timeline — mitigation: scoped down to MVP for Q3
  → Headcount: 2 open engineering roles affecting Q4 capacity
```

---

## Format 3: Engineering / Product Team View

More detail. Owners, dependencies, success metrics.

```
ROADMAP — DETAILED VIEW — Q3 2025
Updated: <date>

GOAL: Push Notifications [G01]
  Owner:         Team A (Lead: Sam)
  RICE Score:    6,400
  Effort:        5 weeks
  Start:         July 1
  Target:        August 5
  Success metric: Message response rate ≥ 50% within 30 days of launch
  Dependencies:   None (G03 depends on this)
  Status:        Week 3 — retrieval layer complete; UI in progress
  Risks:         iOS APNs certificate process (1 week buffer added)

GOAL: Bulk Export [G02]
  Owner:         Team B (Lead: Jordan)
  RICE Score:    5,100
  Effort:        3 weeks
  Start:         July 22
  Target:        August 12
  Success metric: Data-related support tickets < 40/month (currently 100/month)
  Dependencies:   None
  Status:        Planning complete; starting Week 4
  Risks:         Large export performance — load testing needed before release

CAPACITY REMAINING THIS QUARTER:
  Team A: 2 weeks after G01 → buffer / start G03 early
  Team B: 5 weeks after G02 → available for G04 scoping
```

---

## Format 4: Customer-Facing / Public Roadmap

Benefits language. No internal metrics or infrastructure items. Ranges, not exact dates.

```
WHAT WE'RE WORKING ON

COMING SOON (next 1-3 months):
  📱 Instant notifications
     Get notified immediately when you receive a message or update.
     Never miss an important response again.

  📊 Export your data
     Download your full history in CSV or PDF.
     Yours to keep, use, or share however you need.

COMING LATER THIS YEAR:
  🔍 Advanced filters
     Find exactly what you need with powerful filtering and search.

  👩‍💻 Developer dashboard
     Monitor your API usage, rate limits, and performance in one place.

ON OUR RADAR (future):
  📱 Mobile app
  🤖 AI writing assistance

HAVE A SUGGESTION?
  We prioritize based on what matters most to you.
  Share your feedback: [feedback link]

Last updated: July 2025
```

---

## Quarterly Planning Template

Use this at the start of each quarter.

```
QUARTERLY PLANNING SESSION — Q[N] [Year]
Date: <date>   Participants: <team>

1. RETROSPECTIVE ON LAST QUARTER (30 min)
   What did we complete? What didn't we finish? Why?
   Was our RICE scoring accurate? What did we learn?
   
   Completed: <list>
   Not completed: <list + reason>
   Planning accuracy: estimated <N> weeks / actual <N> weeks
   Key learnings: <what to do differently>

2. GOAL INVENTORY UPDATE (20 min)
   New goals to add: <list from feedback, strategy, research>
   Goals to remove: <no longer relevant, already solved, deprioritized>
   
3. RE-SCORE ALL NEXT + LATER GOALS (30 min)
   Have any RICE inputs changed?
   New evidence that changes impact or confidence?
   
   Updated scores:
   [G-ID]  Old RICE  New RICE  Reason for change
   
4. CAPACITY PLANNING (20 min)
   Available person-weeks this quarter: <N>
   Buffer for unplanned (20%): subtract <N>
   Net capacity: <N> person-weeks

5. NOW BUCKET SELECTION (20 min)
   Goals selected for NOW, in priority order:
   [G-ID]  Goal  Owner  Effort  Dependencies
   
6. STAKEHOLDER COMMUNICATION (10 min)
   Who needs to know what changed and why?
   What format do they need?
   When will we communicate?

DECISIONS MADE:
  <key decisions and their rationale>

ROADMAP UPDATED: YES / pending until <date>
```
