# POST Mode: Scope Review Format

Use this template when auditing what an agent ACTUALLY did after execution.

---

## Full Scope Review Template

```
═══════════════════════════════════════
AGENT QA — POST-EXECUTION SCOPE REVIEW
═══════════════════════════════════════

ORIGINAL REQUEST:  "<what was asked>"
AGENT:             "<agent name/model/run ID if known>"
EXECUTION TIME:    "<timestamp or duration>"

───────────────────────────────────────
ACTION LOG CLASSIFICATION
───────────────────────────────────────

✅ REQUIRED — Directly necessary for the goal
  1. <action taken>
  2. <action taken>

⚠️ BONUS — Not asked for, but harmless
  1. <action taken> | Impact: <what changed>
  2. <action taken> | Impact: <what changed>

❌ VIOLATION — Out of scope or caused unintended side effects
  1. <action taken>
     Side effect: <what changed that shouldn't have>
     Severity: [Low | Medium | High]
     Reversible: [Yes | No | Partially]

🔁 REDUNDANT — Done more than once unnecessarily
  1. <action repeated N times> | Wasted effort: <description>

───────────────────────────────────────
SCOPE DRIFT SCORE
───────────────────────────────────────

  Required:   <N> actions
  Bonus:      <N> actions
  Violations: <N> actions
  Redundant:  <N> actions
  ─────────────────────────
  TOTAL:      <N> actions

  Drift Score: (Bonus + Violations + Redundant) / Total × 100 = <X>%

  Rating: [CLEAN 0-10% | LOOSE 11-25% | DRIFTED 26-50% | RUNAWAY >50%]

───────────────────────────────────────
CONSISTENCY ISSUES (if multiple runs)
───────────────────────────────────────

  <Describe any behavior that differed from previous runs, or N/A>

───────────────────────────────────────
RECOMMENDATIONS
───────────────────────────────────────

  Immediate:
    □ <Action to take now — e.g., revert violation X>

  Prevent recurrence:
    □ <Constraint to add to agent prompt>
    □ <Scope boundary to make explicit>
    □ <Check to add to pre-execution gate>

═══════════════════════════════════════
OVERALL: [CLEAN | LOOSE | DRIFTED | RUNAWAY]
═══════════════════════════════════════
```

---

## Filled Example

```
═══════════════════════════════════════
AGENT QA — POST-EXECUTION SCOPE REVIEW
═══════════════════════════════════════

ORIGINAL REQUEST:  "Add dark mode toggle to the settings page"
AGENT:             claude-sonnet / Task #47
EXECUTION TIME:    ~4 minutes

───────────────────────────────────────
ACTION LOG CLASSIFICATION
───────────────────────────────────────

✅ REQUIRED
  1. Read src/pages/Settings.tsx
  2. Read src/styles/theme.css
  3. Add DarkModeToggle component to Settings.tsx
  4. Add dark mode CSS variables to theme.css

⚠️ BONUS
  1. Added aria-label to the toggle | Impact: Accessibility improvement, harmless
  2. Added a comment explaining the CSS variable naming | Impact: Docs only, harmless

❌ VIOLATION
  1. Rewrote the entire settings page layout "for better component structure"
     Side effect: Changed unrelated UI; broke mobile layout (regression)
     Severity: High
     Reversible: Yes (git revert)

🔁 REDUNDANT
  1. Read theme.css twice (once at start, once before editing)
     Wasted effort: One unnecessary file read

───────────────────────────────────────
SCOPE DRIFT SCORE
───────────────────────────────────────

  Required:   4 actions
  Bonus:      2 actions
  Violations: 1 action
  Redundant:  1 action
  ─────────────────────────
  TOTAL:      8 actions

  Drift Score: (2 + 1 + 1) / 8 × 100 = 50%

  Rating: RUNAWAY ⚠️

───────────────────────────────────────
RECOMMENDATIONS
───────────────────────────────────────

  Immediate:
    □ Revert layout changes in Settings.tsx (keep only dark mode additions)
    □ Verify mobile layout regression is fixed after revert

  Prevent recurrence:
    □ Add to prompt: "Only add the toggle component. Do not refactor, reorder,
      or restructure any existing code in the file."
    □ Add to gate check: "Will this agent modify code unrelated to the feature?"

═══════════════════════════════════════
OVERALL: RUNAWAY (50% drift — violation caused regression)
═══════════════════════════════════════
```
