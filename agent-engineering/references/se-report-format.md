# SE Assessment Report Format

---

## Full Report Template

```
╔══════════════════════════════════════════════════════════════╗
║            AGENT SE HEALTH ASSESSMENT                        ║
╚══════════════════════════════════════════════════════════════╝

AGENT / SYSTEM:   <name or description>
SCOPE:            [Full Assessment | Principle Focus: <name>]
ASSESSMENT DATE:  <ISO date>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PRINCIPLE SCORES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Principle            Score   Weight  Weighted   Status
  ─────────────────── ─────── ─────── ──────── ─────────
  Reliability          X / 10   ×20%   = XX.X   [●●●○○]
  Security & Trust     X / 10   ×15%   = XX.X   [●●●●○]
  Efficiency           X / 10   ×15%   = XX.X   [●●○○○]
  Scalability          X / 10   ×12%   = XX.X   [●●●○○]
  Maintainability      X / 10   ×12%   = XX.X   [●○○○○]
  Cost-effectiveness   X / 10   ×10%   = XX.X   [●●●○○]
  User Satisfaction    X / 10   ×10%   = XX.X   [●●●●○]
  Innovation           X / 10   × 6%   = XX.X   [●●○○○]
  ─────────────────────────────────────────────────────
  SE HEALTH SCORE                        XX.X / 100

  Status: [CRITICAL <40 | AT RISK 40-54 | STABLE 55-69 | STRONG 70-84 | EXCELLENT 85+]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FINDINGS BY PRINCIPLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[RELIABILITY — X/10]
  Strengths:
    + <what is working well>
  Gaps:
    - <specific gap with evidence>
    - <specific gap with evidence>
  Critical issues: <none | describe>

[SECURITY & TRUST — X/10]
  Strengths:
    + <what is working well>
  Gaps:
    - <specific gap>
  Critical issues: <none | describe>

[EFFICIENCY — X/10]
  Strengths:
    + <what is working well>
  Gaps:
    - <specific gap>

[SCALABILITY — X/10]
  Strengths: ...
  Gaps: ...

[MAINTAINABILITY — X/10]
  Strengths: ...
  Gaps: ...

[COST-EFFECTIVENESS — X/10]
  Strengths: ...
  Gaps: ...

[USER SATISFACTION — X/10]
  Strengths: ...
  Gaps: ...

[INNOVATION — X/10]
  Strengths: ...
  Gaps: ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CRITICAL GAPS  (score ≤ 4 — blocks production readiness)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  ⛔ <Principle> [X/10] — <one-line summary of why this is critical>
  ⛔ <Principle> [X/10] — <one-line summary>

  (or: No critical gaps found)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
IMPROVEMENT ROADMAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  PHASE 1 — Fix Critical Gaps (do now, blocks production)
  ─────────────────────────────────────────────────────────
  □ [<Principle>] <Concrete action>
    Expected improvement: X/10 → ~Y/10
    Effort: <hours/days>
    
  □ [<Principle>] <Concrete action>
    Expected improvement: X/10 → ~Y/10
    Effort: <hours/days>

  PHASE 2 — Address Significant Gaps (next sprint)
  ─────────────────────────────────────────────────
  □ [<Principle>] <Action>
    Expected improvement: X/10 → ~Y/10

  □ [<Principle>] <Action>

  PHASE 3 — Optimization (next quarter)
  ──────────────────────────────────────
  □ [<Principle>] <Action>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PROJECTED SCORE (after Phase 1 + 2)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Current:   XX.X / 100  [STATUS]
  After P1:  ~XX.X / 100 [STATUS]
  After P2:  ~XX.X / 100 [STATUS]

╔══════════════════════════════════════════════════════════════╗
║  VERDICT: [CRITICAL | AT RISK | STABLE | STRONG | EXCELLENT] ║
║  Production Ready: [YES | NO — resolve critical gaps first]  ║
╚══════════════════════════════════════════════════════════════╝
```

---

## Worked Example — Customer Support Agent

```
╔══════════════════════════════════════════════════════════════╗
║            AGENT SE HEALTH ASSESSMENT                        ║
╚══════════════════════════════════════════════════════════════╝

AGENT / SYSTEM:   Customer Support Resolution Agent v2.1
SCOPE:            Full Assessment
ASSESSMENT DATE:  2025-08-14

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PRINCIPLE SCORES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Principle            Score   Weight  Weighted   Status
  ─────────────────── ─────── ─────── ──────── ─────────
  Reliability          6 / 10   ×20%   = 12.0   [●●●○○]
  Security & Trust     4 / 10   ×15%   =  6.0   [●●○○○] ⛔
  Efficiency           5 / 10   ×15%   =  7.5   [●●○○○]
  Scalability          7 / 10   ×12%   =  8.4   [●●●●○]
  Maintainability      3 / 10   ×12%   =  3.6   [●○○○○] ⛔
  Cost-effectiveness   5 / 10   ×10%   =  5.0   [●●○○○]
  User Satisfaction    7 / 10   ×10%   =  7.0   [●●●●○]
  Innovation           6 / 10   × 6%   =  3.6   [●●●○○]
  ─────────────────────────────────────────────────────
  SE HEALTH SCORE                        53.1 / 100

  Status: AT RISK

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FINDINGS (Summary)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[SECURITY & TRUST — 4/10]  ⛔ CRITICAL
  + Tool permissions are scoped per department
  - Customer PII (name, email, order history) included in every prompt
    even for queries that only need order status
  - No audit log of which agent accessed which customer record
  - Indirect prompt injection not addressed: agent reads customer-supplied
    notes that could contain instructions

[MAINTAINABILITY — 3/10]  ⛔ CRITICAL
  + Code is in version control
  - System prompt is 3,400 tokens with no section headers or comments
  - Last prompt change has no commit message explaining the reason
  - No local test harness; every test requires a live API key
  - Zero documentation on what happens when the escalation tool fails

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
IMPROVEMENT ROADMAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  PHASE 1 — Fix Critical Gaps
  ─────────────────────────────────────────────────────────
  □ [Security] Implement data minimization: pass only required fields
    to each tool call, not full customer record
    Expected: 4/10 → 7/10 | Effort: 2 days

  □ [Security] Add audit log: record (timestamp, agent_id, customer_id,
    action, tool_called) for every customer data access
    Expected: audit coverage 0% → 100% | Effort: 1 day

  □ [Security] Sanitize customer-provided free-text before embedding
    in agent context (strip instruction-like patterns)
    Expected: eliminates prompt injection surface | Effort: 1 day

  □ [Maintainability] Break 3,400-token prompt into named sections with
    comments; commit with rationale; add to PR review checklist
    Expected: 3/10 → 6/10 | Effort: 4 hours

  □ [Maintainability] Create local mock server for tools; write 10
    regression tests for known failure cases
    Expected: test coverage 0% → basic | Effort: 3 days

  PHASE 2 — Address Significant Gaps
  ─────────────────────────────────────
  □ [Efficiency] Cache customer profile lookups within session;
    avoid re-fetching on every message
    Expected: 5/10 → 7/10 | Effort: 1 day

  □ [Reliability] Add retry with backoff on CRM tool failures;
    surface partial results when escalation tool is unavailable
    Expected: 6/10 → 8/10 | Effort: 2 days

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PROJECTED SCORE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Current:   53.1 / 100  [AT RISK]
  After P1:  ~68.0 / 100 [STABLE]
  After P2:  ~76.0 / 100 [STRONG]

╔══════════════════════════════════════════════════════════════╗
║  VERDICT: AT RISK                                            ║
║  Production Ready: NO — resolve Security and                 ║
║  Maintainability gaps before production traffic              ║
╚══════════════════════════════════════════════════════════════╝
```

---

## Score Dot Legend

```
[●●●●●] = 9–10  Excellent
[●●●●○] = 7–8   Good
[●●●○○] = 5–6   Acceptable
[●●○○○] = 3–4   At Risk       ← needs action
[●○○○○] = 1–2   Critical      ← blocks production
[○○○○○] = 0     Absent
```

## Quick Focus Mode Template

When assessing only one principle:

```
PRINCIPLE FOCUS: <Name> — <Agent/System>

Current Score: X / 10

Top Strengths:
  + <what's working>

Gaps Found:
  GAP-1: <description>
    Evidence: <what was observed>
    Impact:   <what goes wrong>
    Fix:      <concrete action>

  GAP-2: ...

Recommended Actions (priority order):
  1. <action> → expected score after: ~Y/10
  2. <action>
  3. <action>

Cross-principle effects:
  This improvement also affects: <Efficiency | Reliability | ...>
```
