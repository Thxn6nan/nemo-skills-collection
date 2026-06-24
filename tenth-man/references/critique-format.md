# Critique Format Reference

Each objection must follow this structure exactly.
Do not present an objection unless all required fields can be filled honestly.

---

## Objection Template

```
### [OBJECTION-N] <Short Title>

**Dimension:** <Assumption Gap | Failure Mode | Security | Scalability | Dependency | Edge Case | Reversibility>
**Severity:** <P1 Fatal | P2 Major | P3 Minor | P4 Noise>

**What could go wrong:**
<1-3 sentences describing the specific problem>

**Mechanism / Why this happens:**
<Explain the causal chain. How does condition X lead to failure Y?>

**Evidence or precedent:**
<Historical example, known pattern, or logical proof. 
 If none exists, this objection should be downgraded to P3 or dropped.>

**Probability:** <Low | Medium | High>
**Impact if it occurs:** <Low | Medium | High | Critical>

**Suggested mitigation:**
<Concrete action to reduce or eliminate this risk.
 If no mitigation exists → escalate severity.>
```

---

## Filled Example — API Rate Limiting

```
### [OBJECTION-1] No backoff strategy on third-party API calls

**Dimension:** Failure Mode
**Severity:** P2 Major

**What could go wrong:**
When the external payment API returns 429 (rate limit), the service will
retry immediately in a tight loop, compounding the problem and potentially
triggering a ban or extended lockout.

**Mechanism / Why this happens:**
The current code catches HTTP errors and retries with no delay. Under load,
multiple concurrent requests hitting the rate limit will all retry simultaneously,
creating a thundering herd that makes the problem worse with each cycle.

**Evidence or precedent:**
This pattern caused a 4-hour outage at Stripe's integration in 2021 (Stripe status page).
Exponential backoff with jitter is the industry-standard mitigation (AWS architecture blog).

**Probability:** High
**Impact if it occurs:** High — payment failures during peak load

**Suggested mitigation:**
Implement exponential backoff with jitter (e.g., wait 2^n * random(0,1) seconds).
Add a circuit breaker that stops retrying after 5 consecutive failures.
```

---

## Multi-Objection Summary Format

After listing all individual objections, close with:

```
## Summary

| # | Title | Severity | Probability | Impact |
|---|-------|----------|-------------|--------|
| 1 | No backoff on API calls | P2 Major | High | High |
| 2 | Missing input validation on user_id | P2 Major | Medium | High |
| 3 | Logs contain PII in plaintext | P1 Fatal | High | Critical |

## VERDICT: FATAL FLAW

**Blocker:** [OBJECTION-3] must be resolved before any production deployment.

**Recommended next step:** Strip PII from logs immediately; review all logging
calls in the auth module for similar issues before re-review.
```

---

## When NOT to Write an Objection

Skip the objection entirely if:
- The risk exists but the plan already handles it
- The concern is purely stylistic (naming, formatting, tone)
- The scenario requires multiple simultaneous unlikely failures (P4 noise)
- You cannot articulate a specific mechanism — only a vague "this might fail"
- The mitigation is already built in and adequate
