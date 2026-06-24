# Severity Classification Guide

Use this guide to classify every objection before presenting it.
When in doubt between two levels, ask: "Would a senior engineer lose sleep over this?"

---

## Severity Levels

### P1 — Fatal 🔴
**Definition:** If unaddressed, this will cause serious harm, data loss, security breach,
or complete failure. Must be fixed before any further progress.

**Decision:** STOP. Do not proceed until resolved.

**Examples:**
- SQL injection vulnerability in user-facing input
- Authentication bypass possible with crafted request
- Data will be permanently deleted without backup
- GDPR/legal compliance violation
- Race condition that causes double-charges in payments
- No rollback plan for irreversible database migration

---

### P2 — Major 🟠
**Definition:** Real risk that will likely surface in production or under realistic load.
Doesn't necessarily stop everything, but must be resolved before go-live.

**Decision:** PROCEED WITH CAUTION. Fix before shipping.

**Examples:**
- No retry/backoff on external API calls
- Missing input validation on critical fields
- Memory leak under sustained load
- Single point of failure with no fallback
- Error states not handled (silent failures)
- Hardcoded credentials in config files

---

### P3 — Minor 🟡
**Definition:** Real issue but limited blast radius. Won't cause outages but creates
technical debt, poor UX, or small reliability gaps.

**Decision:** APPROVED WITH NOTES. Fix in next iteration.

**Examples:**
- N+1 query problem (slow but not broken)
- Missing logging on non-critical paths
- Unclear variable names that will confuse future maintainers
- Lack of pagination on list endpoints (fine until scale grows)
- No timeout on internal service calls (low-traffic paths)

---

### P4 — Noise ⚪
**Definition:** Theoretically possible but requires extremely unlikely conditions,
or the concern is stylistic/preference-based.

**Decision:** LOG ONLY. Do not present as a finding. Do not block work.

**Examples:**
- "What if the database server physically catches fire?"
- "This variable name could be more descriptive"
- "The comment on line 42 is a bit terse"
- Risks that require 3+ simultaneous independent failures
- Issues already explicitly handled by the existing design

> ⚠️ P4 items must NOT appear in the final objection list.
> If everything is P4, the verdict is APPROVED.

---

## Classification Decision Tree

```
Is this a real risk (not hypothetical extremes)?
├── No → P4 Noise (drop it)
└── Yes ↓

Does the current design already handle it adequately?
├── Yes → P4 Noise (drop it)  
└── No ↓

Could this cause data loss, security breach, or complete failure?
├── Yes → P1 Fatal
└── No ↓

Will this likely surface in production or realistic load?
├── Yes → P2 Major
└── No ↓

Is this a real engineering concern (not style/preference)?
├── Yes → P3 Minor
└── No → P4 Noise (drop it)
```

---

## Probability × Impact Matrix

Use this to validate your severity classification:

|              | Low Impact | Medium Impact | High Impact | Critical Impact |
|--------------|-----------|---------------|-------------|-----------------|
| **High Prob**   | P3        | P2            | P2          | P1              |
| **Medium Prob** | P3        | P3            | P2          | P1              |
| **Low Prob**    | P4        | P3            | P3          | P2              |
| **Very Low**    | P4        | P4            | P4          | P3              |

If your matrix result is P1 but you can't describe a concrete mechanism → downgrade to P2.
