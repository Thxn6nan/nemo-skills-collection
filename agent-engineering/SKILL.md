---
name: agent-engineering
description: >
  Evaluate and improve agent systems against 8 core software engineering principles:
  Reliability, Efficiency, Scalability, Maintainability, Cost-effectiveness,
  Security & Trust, User Satisfaction, and Innovation. Use this skill whenever
  designing a new agent, reviewing an existing agent system, diagnosing why an
  agent underperforms, planning agent improvements, or establishing engineering
  standards for a multi-agent pipeline. Trigger phrases: "review this agent design",
  "why is the agent slow/unreliable/expensive", "improve agent quality",
  "engineer this agent properly", "set standards for the agent", "agent health check",
  "assess the agent system", "make the agent more robust/scalable/maintainable".
  Use alongside agent-qa (scope) and agent-audit (defects) for full agent governance.
---

# Agent Engineering Skill

Applies 8 software engineering principles to evaluate and improve agent systems.
Agents are software — and like all software, they degrade under pressure unless
engineered with discipline.

## Core Principle

> A well-engineered agent is not just one that works today.
> It is one that works reliably, efficiently, and safely at scale — 
> and can be understood, maintained, and improved tomorrow.

---

## The 8 Principles (Agent Context)

| # | Principle | Weight | Core Question |
|---|-----------|--------|---------------|
| 1 | **Reliability** | 20% | Does the agent produce correct, consistent output under real conditions? |
| 2 | **Security & Trust** | 15% | Can the agent be trusted with sensitive data and decisions? |
| 3 | **Efficiency** | 15% | Does the agent use the minimum resources to accomplish the task? |
| 4 | **Scalability** | 12% | Does the design hold up at 10× or 100× the current load? |
| 5 | **Maintainability** | 12% | Can the agent be understood, debugged, and updated by others? |
| 6 | **Cost-effectiveness** | 10% | Are the resources spent proportional to the value delivered? |
| 7 | **User Satisfaction** | 10% | Does the agent actually serve the user's real needs well? |
| 8 | **Innovation** | 6% | Is the agent leveraging modern capabilities — not fighting them? |

Weights reflect engineering priority: a fast but unreliable or insecure agent
ships negative value.

---

## Assessment Workflow

### Step 1 — Define Scope

Determine what is being assessed:
- **Single agent** — evaluate one agent end-to-end
- **Agent design/plan** — evaluate before building
- **Multi-agent pipeline** — evaluate interactions and handoffs
- **Specific principle** — deep-dive on one area only

Ask: Which principles are most relevant to this context?
(e.g., a batch processing agent → weight Efficiency and Cost more;
a customer-facing agent → weight Reliability and User Satisfaction more)

### Step 2 — Score Each Principle (0–10)

For each principle in scope, read its criteria section in
`references/principles-guide.md` and score 0–10:

```
0–2   Absent or critically broken
3–4   Partial — major gaps that will cause real problems
5–6   Acceptable — works but has meaningful weaknesses
7–8   Good — solid foundation with minor gaps
9–10  Excellent — model implementation
```

Be honest. A score of 6 is not a failure — it means "works but can improve."
Only score 9–10 if you can defend it against a senior engineer's scrutiny.

### Step 3 — Calculate the SE Health Score

```
Health Score = Σ (principle_score × weight)

Example:
  Reliability     7/10 × 20% = 14.0
  Security        8/10 × 15% = 12.0
  Efficiency      5/10 × 15% =  7.5
  Scalability     6/10 × 12% =  7.2
  Maintainability 4/10 × 12% =  4.8
  Cost            6/10 × 10% =  6.0
  User Sat.       7/10 × 10% =  7.0
  Innovation      5/10 ×  6% =  3.0
  ────────────────────────────────────
  SE Health Score             = 61.5 / 100
```

### Step 4 — Identify Critical Gaps

A **critical gap** is any principle scored ≤ 4.
Critical gaps override the overall score — a system with a critical gap
in Security or Reliability is not production-ready regardless of total score.

### Step 5 — Build the Improvement Roadmap

For each gap, generate concrete actions ranked by:
1. Critical gaps first (score ≤ 4)
2. Highest-weight principles next
3. Quick wins (high impact, low effort) before long-term changes

### Step 6 — Deliver the SE Assessment Report

See `references/se-report-format.md` for full template.

```
SE HEALTH SCORE: <X> / 100  [CRITICAL | AT RISK | STABLE | STRONG | EXCELLENT]

Critical gaps:    <list — must fix>
Significant gaps: <list — should fix>
Strengths:        <list — maintain>

Top 3 recommended actions:
  1. <action> → improves <principle> from X to ~Y
  2. <action> → improves <principle> from X to ~Y
  3. <action> → improves <principle> from X to ~Y
```

---

## Score Interpretation

| SE Health Score | Status | Meaning |
|----------------|--------|---------|
| 85–100 | **EXCELLENT** | Production-grade. Maintain and monitor. |
| 70–84 | **STRONG** | Solid system. Address remaining gaps in next cycle. |
| 55–69 | **STABLE** | Works under normal conditions. Known risks to manage. |
| 40–54 | **AT RISK** | Will fail under pressure. Fix before scale. |
| 0–39 | **CRITICAL** | Not production-ready. Redesign required. |

---

## Quick-Mode (Single Principle)

If the user asks about a specific principle only:
1. Skip scoring other principles
2. Deep-dive using `references/principles-guide.md` for that principle
3. Deliver a focused gap analysis and action list
4. Note which other principles this improvement would also affect

---

## Integration with Other Skills

This skill works alongside the agent governance stack:

```
agent-engineering  ← Is the system well-engineered? (design quality)
agent-qa           ← Is the agent doing only what it should? (scope)
agent-audit        ← Is the agent's output correct? (defect quality)
tenth-man          ← Are we missing risks? (adversarial review)
```

For production agent systems, run all four.

---

## Reference Files

- `references/principles-guide.md` — Criteria, anti-patterns, and fixes for all 8 principles
- `references/se-report-format.md` — Full assessment report template + worked example

---

## Example Invocations

```
"Do an SE health check on this agent"
"Why is our agent so slow and expensive?"
"Review this agent design before we build it"
"The agent keeps failing in production — what's the engineering problem?"
"Score our agent pipeline against software engineering standards"
"We need to make the agent more maintainable — where do we start?"
"Is this agent design production-ready?"
```
