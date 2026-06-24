---
name: tenth-man
description: >
  Apply the 10th Man Rule to stress-test plans, proposals, code architectures,
  decisions, or any output where groupthink or blind spots could cause failure.
  Use this skill whenever the user asks to "challenge", "stress-test", "red-team",
  "find flaws in", "critique", or "play devil's advocate" on any proposal.
  Also trigger when the user seems too confident about a plan ("this should work, right?"),
  when multiple agents/team members have agreed on something, or when the stakes are high
  (production deployments, architecture decisions, business strategy, security reviews).
  Do NOT use for purely stylistic feedback or minor wording changes.
---

# 10th Man Skill

Implements the **10th Man Rule**: when consensus is reached, one voice must actively
seek evidence that the consensus is *wrong* — not to be contrarian, but to surface
genuine risks before they become failures.

## Core Principle

The 10th Man's job is **not** to disagree for the sake of it.  
The job is to **find real failure modes** that optimistic thinkers overlooked.  
If no genuine flaw exists → say so clearly. That is also a valid output.

---

## Workflow

### Step 1 — Understand the Proposal
Read the full proposal/plan/code. Identify:
- What is the **goal**?
- What **assumptions** does it rely on?
- What does **success** look like?

### Step 2 — Run the 10th Man Analysis
For each dimension in scope, generate objections **only if backed by evidence**.
See `references/critique-format.md` for the structured format.

Dimensions to check (select relevant ones):
- **Assumption gaps** — What must be true for this to work? Is it actually true?
- **Failure modes** — What specific mechanism could cause this to break?
- **Security / safety** — What can be exploited or misused?
- **Scalability** — Where does this stop working at larger scale?
- **Dependencies** — What external things could fail or change?
- **Edge cases** — What inputs or conditions weren't accounted for?
- **Reversibility** — How hard is it to undo if this goes wrong?

### Step 3 — Classify Each Objection
Use the severity system in `references/severity-guide.md`:
- **P1 Fatal** — Stop and fix before proceeding
- **P2 Major** — Must address; blocks go-live
- **P3 Minor** — Should fix; doesn't block
- **P4 Noise** — Log only; not actionable

> ⚠️ Every objection must have a severity. P4 items are NOT presented as real findings.

### Step 4 — Deliver the Verdict

After all objections are classified, deliver a final verdict:

```
VERDICT: [FATAL FLAW | PROCEED WITH CAUTION | APPROVED]

Summary of findings:
- P1: <count> — <brief description>
- P2: <count> — <brief description>  
- P3: <count> — <brief description>

Recommended next step: <concrete action>
```

If no P1/P2 found → verdict is APPROVED, and say so confidently.  
Do not manufacture problems to seem thorough.

---

## Anti-Noise Rules

These rules prevent the skill from generating noise instead of signal:

1. **No evidence, no objection** — Every objection needs a mechanism, precedent, or logical chain
2. **No style critiques** — Grammar, naming, formatting are out of scope unless explicitly asked
3. **No hypothetical extremes** — "What if a meteor hits the server" is P4 noise
4. **Verdict must be honest** — If the plan is solid, say "APPROVED" with confidence
5. **One root cause per objection** — Don't pad by restating the same risk in different words

---

## Reference Files

- `references/critique-format.md` — Full structured format for each objection
- `references/severity-guide.md` — Severity classification guide with examples

Read these when you need detailed formatting or are unsure how to classify severity.

---

## Example Invocations

```
"Red-team this deployment plan"
"What could go wrong with this architecture?"
"Play devil's advocate on our go-to-market strategy"
"Stress-test this API design before we build it"
"Is there anything we're missing about this approach?"
"Challenge this — we all agreed but I want a second opinion"
```
