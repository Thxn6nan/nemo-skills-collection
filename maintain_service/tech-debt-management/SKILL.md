---
name: tech-debt-management
description: >
  Identify, classify, and strategically manage technical debt — deciding what
  to fix now, what to schedule, and what to live with. Covers debt inventory,
  refactor vs rebuild decisions, and debt reduction planning. Use this skill
  when technical debt is slowing down development, when deciding whether to
  refactor or rewrite, when planning a debt reduction sprint, or when a system
  is becoming hard to change. Trigger phrases: "technical debt", "the codebase
  is a mess", "should we refactor or rewrite?", "this is getting hard to
  change", "we're slowing down", "legacy code", "clean this up", "plan a
  refactor", "how do we manage debt?", "is it worth cleaning this up?".
---

# Tech Debt Management Skill

Helps teams make rational decisions about technical debt rather than
accumulating it unconsciously or addressing it reactively in a crisis.

## Core Principle

> Technical debt is not always bad — it is a trade-off.
> Incurring debt deliberately to ship faster can be the right decision.
> What is always bad: incurring debt unconsciously, accumulating it
> without a plan, or not paying it back before it compounds.

---

## Debt Classification

Not all technical debt is the same. Classify before prioritizing.

```
TYPE A — DELIBERATE & PRUDENT ("we know better but chose speed now")
  Example: "We hardcoded the config to ship this week. We'll make it 
            configurable next sprint."
  Status: acceptable if tracked and paid back on schedule
  Action: schedule repayment before taking on more of this debt

TYPE B — DELIBERATE & RECKLESS ("we know better and didn't care")
  Example: "No tests because testing is boring."
  Status: not acceptable — creates ongoing risk
  Action: treat as highest priority for remediation

TYPE C — INADVERTENT ("we didn't know better at the time")
  Example: Design from 3 years ago that doesn't fit current scale.
  Status: normal, expected — systems outgrow their designs
  Action: address when the cost of carrying it exceeds the cost of fixing

TYPE D — BIT ROT ("was fine, became debt as the world changed")
  Example: Library that was maintained and is now abandoned.
  Status: low urgency unless it causes problems
  Action: monitor; address proactively before it becomes a crisis
```

---

## Debt Inventory

Before prioritizing, know what you have.

```
DEBT INVENTORY FORMAT (one row per item):
  ID:          DEBT-N
  Description: <what the debt is>
  Type:        [A | B | C | D]
  Location:    <codebase area, module, service>
  Impact:      <how this debt affects the team today>
    - Developer velocity: [High | Medium | Low | None]
    - Defect risk:        [High | Medium | Low | None]
    - Scaling risk:       [High | Medium | Low | None]
    - Security risk:      [High | Medium | Low | None]
  Cost to fix: [Days | Weeks | Months]
  Cost to carry (per sprint): <estimated drag on velocity>
  Age:         <how long has this existed>

COST TO CARRY = the hidden tax of not fixing
  "This debt costs us X hours of extra work per sprint because..."
  Make this concrete — vague debt claims don't get prioritized.
```

---

## Prioritization Framework

```
PRIORITY SCORE = (Impact × Urgency) / Fix Cost

IMPACT (1-5):
  5 = Blocking critical work or creating security risk
  4 = Causing regular bugs or significant velocity drag
  3 = Causing occasional problems or moderate drag
  2 = Mildly annoying, occasional impact
  1 = Cosmetic, no real impact

URGENCY (1-5):
  5 = Getting worse every sprint (compounding debt)
  4 = Causing incidents or blocking roadmap items
  3 = Slowing down but stable
  2 = Stable, low urgency
  1 = Could be left indefinitely without major consequence

FIX COST (1-5):
  1 = Hours (quick win)
  2 = 1-2 days
  3 = 1 week
  4 = 2-4 weeks
  5 = Month+ (major project)

RESULT TIERS:
  Score ≥ 4.0 → Fix this sprint (high impact, high urgency, manageable cost)
  Score 2.5-3.9 → Schedule in next 1-3 sprints
  Score 1.0-2.4 → Backlog (monitor; address when convenient)
  Score < 1.0  → Accept and live with it (not worth fixing)
```

---

## Refactor vs Rebuild Decision

The most consequential decision in debt management.

```
REFACTOR (incremental improvement):
  Change the internals without changing the behavior.
  Safer: smaller changes, easier to test, can stop at any point.
  
REBUILD (start from scratch):
  Replace the system with a new one.
  Higher risk: "second system syndrome," long transition period.

REBUILD IS ALMOST ALWAYS HARDER THAN IT LOOKS:
  The old system contains implicit knowledge not in any document.
  The new system will rediscover all the same edge cases.
  The transition period is painful and long.
  Plan for it to take 2-3× longer than estimated.

CHOOSE REBUILD ONLY WHEN:
  □ Refactoring requires changing >80% of the code anyway
  □ The architecture cannot evolve to meet requirements (not just hard — impossible)
  □ The technology is deprecated and the migration path is a rebuild
  □ The team spends more time working around the system than with it

CHOOSE REFACTOR WHEN:
  □ The architecture is sound — only the implementation needs improvement
  □ Tests exist (or can be written) that verify behavior is preserved
  □ You can incrementally improve without a long transition period
  □ The Strangler Fig pattern applies (wrap old, incrementally replace)

THE STRANGLER FIG PATTERN:
  1. Build the new system alongside the old
  2. Route a small amount of traffic to new system
  3. Incrementally migrate more functionality
  4. Eventually decommission the old system
  Best for: systems that can be split by feature or endpoint
```

---

## Debt Reduction Planning

```
SPRINT ALLOCATION MODEL:
  70% new feature work
  20% debt reduction (sustainable pace)
  10% unplanned/emergencies

  Avoid: "debt reduction sprint" (one big push, then back to accumulating)
  Prefer: 20% ongoing (sustainable, builds the habit)

DEBT REDUCTION SPRINT (when needed):
  Justified when: debt is actively blocking roadmap items.
  Scope: fix the debt that is blocking work; nothing else.
  Danger: debt sprints often expand scope ("while we're here...").
  Apply project-qa PRE mode to define strict scope before starting.

PAYING BACK DELIBERATE DEBT:
  Type A debt (deliberate) must have a payback date at time of incurring.
  "We're taking this shortcut and we will fix it by [sprint/date]."
  Track Type A debt as first-class items in the backlog, not footnotes.
```

---

## Debt Review Cadence

```
MONTHLY (15 minutes):
  □ Review top 5 debt items: status, still prioritized correctly?
  □ Any new high-priority debt identified this month?
  □ Any debt paid back this month? Remove from inventory.

QUARTERLY (30 minutes):
  □ Full inventory review: re-score all items
  □ Is the 20% allocation being honored?
  □ Is debt growing, stable, or shrinking overall?
  □ Are any items escalating in urgency?

BEFORE MAJOR PROJECTS:
  □ Does this project touch heavily-indebted areas?
  □ Should debt be addressed before or during the project?
  □ Does the project incur new debt? If so, plan payback now.
```

---

## Reference Files

- `references/refactor-patterns.md` — Safe refactoring patterns and techniques
