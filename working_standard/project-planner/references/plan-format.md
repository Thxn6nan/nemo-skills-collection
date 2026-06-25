# Plan Format — Template and Worked Example

---

## Full Plan Template

```
╔══════════════════════════════════════════════════════════════╗
║                  EXECUTION PLAN                              ║
╚══════════════════════════════════════════════════════════════╝

GOAL:           <one sentence — from goal-evaluator>
SUCCESS:        <measurable definition of done>
TOTAL EFFORT:   <N person-hours or person-days>
CALENDAR TIME:  <N weeks at <N> people>
CRITICAL PATH:  <T-ID> → <T-ID> → <T-ID> → Done

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TASK BREAKDOWN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[COMPONENT A — <name>]

  T01  <task name>
       What:     <exactly what this task produces>
       Done when: <observable completion condition>
       Estimate:  O:<Xh> R:<Xh> P:<Xh> → Weighted: <Xh> × <multiplier> = <Xh>
       Depends:   <none | T-ID, T-ID>
       Owner:     <person | agent | TBD>
       Risk:      [Low|Med|High] — <if medium/high: what could go wrong>

  T02  <task name>
       What:     <...>
       Done when: <...>
       Estimate:  O:<Xh> R:<Xh> P:<Xh> → Weighted: <Xh> × <multiplier> = <Xh>
       Depends:   T01
       Owner:     <...>
       Risk:      [Low|Med|High]

[COMPONENT B — <name>]

  T03  <task name>
       ...

[COMPONENT C — Testing / Integration / Docs]

  T0N  <task name>
       ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DEPENDENCY MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  T01 ──→ T02 ──→ T05 ──→ T07  [critical path]
           ↓
          T03 ──→ T06           [parallel stream]
  T04 ────────────────→ T07     [independent, joins at T07]

  Critical path tasks: T01, T02, T05, T07
  Float tasks: T03, T04, T06 (can slip without delaying finish)
  Parallelizable: T03+T04 (no dependency between them)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
MILESTONES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  M1 [Day/Week X] — Core assumption validated
     Observable: <what can be seen or tested>
     "If we stop here, we have: <what exists>"
     If M1 is not reached by <date> → flag and reassess

  M2 [Day/Week X] — Critical path halfway
     Observable: <what can be seen or tested>
     "If we stop here, we have: <what exists>"

  M3 [Day/Week X] — Feature complete (unhardened)
     Observable: <all functionality present; QA remaining>

  M4 [Day/Week X] — DONE
     Observable: <matches success definition above>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RISK REGISTER
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  ⚠️  T0N — <risk description>
      Trigger:  <how you know this risk is happening>
      Response: <what to do when it triggers>
      Impact:   <how many days does this add if it hits>

  ⚠️  T0N — <risk description>
      Trigger:  <...>
      Response: <...>
      Impact:   <...>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SKILL TRIGGER POINTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Before T__: Run project-qa (PRE) to define scope boundary
  During execution: Apply working-standards working standards
  After T__: Run work-audit on output
  After T__: Run anti-hallucination if output contains data/numbers
  At M2: Run tenth-man to challenge assumptions before committing to M3

╔══════════════════════════════════════════════════════════════╗
║  START TODAY:  T01 — <task name>                             ║
║  WATCH FIRST:  <highest-risk task to front-load>             ║
║  REVIEW AT:    M1 on <date>                                  ║
╚══════════════════════════════════════════════════════════════╝
```

---

## Worked Example — "Add RAG grounding to resolution agent"

*(This is the "One Thing" from project-compass's worked example)*

```
╔══════════════════════════════════════════════════════════════╗
║                  EXECUTION PLAN                              ║
╚══════════════════════════════════════════════════════════════╝

GOAL:          Add retrieval-augmented generation to the resolution agent
               so it answers from policy documents instead of memory
SUCCESS:       Resolution rate on 50-ticket test set moves from 40% to ≥60%.
               Zero incorrect policy citations in manual review of 20 tickets.
TOTAL EFFORT:  ~30 person-hours (3–4 working days, 1 person)
CALENDAR TIME: 1 week
CRITICAL PATH: T01 → T02 → T04 → T06 → T07

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TASK BREAKDOWN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[COMPONENT A — Document Preparation]

  T01  Audit and prepare the 3 policy documents
       What:     Clean PDFs → structured text chunks; identify gaps in coverage
       Done when: All 3 docs chunked; chunk quality verified on 10 test queries
       Estimate:  O:2h R:4h P:8h → Weighted: 4.3h × 1.5 = 6.5h
       Depends:   none
       Owner:     Engineer A
       Risk:      HIGH — documents may be poorly structured (scanned PDFs, tables)
                  This is the most uncertain task; do it first.

  T02  Build and test the vector index
       What:     Embed chunks → store in vector DB → verify retrieval accuracy
       Done when: Top-3 retrieval hits correct policy for 8/10 test queries
       Estimate:  O:2h R:3h P:6h → Weighted: 3.3h × 1.5 = 5h
       Depends:   T01
       Owner:     Engineer A
       Risk:      MED — retrieval quality depends on chunk size decisions in T01

[COMPONENT B — Agent Integration]

  T03  Define scope boundary for modified agent
       What:     Explicitly define what agent WILL and WON'T do differently
       Done when: project-qa PRE report approved
       Estimate:  O:0.5h R:1h P:2h → Weighted: 1h × 1.2 = 1.2h
       Depends:   none (parallel with T01/T02)
       Owner:     Engineer A + reviewer
       Risk:      LOW

  T04  Modify resolution agent: replace memory recall with retrieval call
       What:     Add retrieve() call before answer generation;
                 update prompt to use retrieved context only
       Done when: Agent answers using only retrieved text (verified on 5 examples)
       Estimate:  O:2h R:4h P:8h → Weighted: 4.3h × 1.5 = 6.5h
       Depends:   T02, T03
       Owner:     Engineer A
       Risk:      MED — prompt changes may affect other behaviors; test carefully

[COMPONENT C — Evaluation]

  T05  Build evaluation harness
       What:     Script to run 50-ticket test set and score resolution rate
       Done when: Script produces resolution rate metric automatically
       Estimate:  O:1h R:2h P:4h → Weighted: 2.2h × 1.2 = 2.6h
       Depends:   none (parallel with T01-T04)
       Owner:     Engineer A
       Risk:      LOW

  T06  Run evaluation + anti-hallucination check
       What:     Baseline (current agent) → new agent → compare;
                 manual review of 20 tickets for citation accuracy
       Done when: Resolution rate ≥60%; zero incorrect citations in sample
       Estimate:  O:2h R:3h P:5h → Weighted: 3.2h × 1.2 = 3.8h
       Depends:   T04, T05
       Owner:     Engineer A
       Risk:      MED — if result is 50-58%, must decide: ship or iterate?

  T07  Update agent documentation and handoff
       What:     Update README; document retrieval configuration; known limitations
       Done when: Another engineer could modify the retrieval setup from the docs
       Estimate:  O:1h R:2h P:3h → Weighted: 2h × 1.2 = 2.4h
       Depends:   T06
       Owner:     Engineer A
       Risk:      LOW

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DEPENDENCY MAP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  T01 ──→ T02 ──→ T04 ──→ T06 ──→ T07  [critical path]
  T03 ─────────→ T04
  T05 ─────────────────→ T06

  Critical path: T01 → T02 → T04 → T06 → T07 (~24h total)
  Float tasks:   T03 (3h float), T05 (8h float)
  Parallel opportunity: T03 and T05 can run alongside T01/T02

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
MILESTONES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  M1 [Day 2] — Document retrieval works
     Observable: 8/10 test queries return correct policy chunk in top-3 results
     "If we stop here, we have: a working retrieval index + evidence the
      documents are usable. We know the hardest part is feasible."
     If NOT reached by Day 2 → documents likely need more prep work; re-estimate T01

  M2 [Day 3] — Modified agent working on examples
     Observable: Agent answers 5 test tickets using retrieved text, not memory
     "If we stop here, we have: a working prototype, not yet evaluated at scale"

  M3 [Day 4] — Evaluation complete
     Observable: Resolution rate measured on full 50-ticket test set
     "If we stop here, we have: the answer to whether this approach works"
     Decision point: if rate is <55%, stop and run project-compass before continuing

  M4 [Day 5] — DONE
     Observable: Resolution rate ≥60%, citations clean, docs updated

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RISK REGISTER
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  ⚠️  T01 — Policy docs are scanned PDFs with poor text extraction
      Trigger:  Text extraction produces garbled output on test chunk
      Response: Use OCR tool (Textract/Tesseract); add 1 day to T01
      Impact:   +1 day to calendar time

  ⚠️  T06 — Resolution rate lands between 55-59% (below 60% target)
      Trigger:  Evaluation result < 60%
      Response: Do not declare done. Analyze which ticket types are failing.
                If fixable in <4h: fix and re-evaluate.
                If not: run project-compass before further investment.
      Impact:   +0.5–2 days if hit

  ⚠️  T04 — Prompt changes degrade routing or escalation behavior
      Trigger:  Existing test cases for routing/escalation start failing
      Response: Revert prompt changes; isolate the change that caused regression
      Impact:   +0.5–1 day

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SKILL TRIGGER POINTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Before T04:    project-qa (PRE) — define scope of prompt changes
  During all:    working-standards standards (Reliability, Maintainability)
  After T04:     work-audit on modified agent code
  During T06:    anti-hallucination check on resolution outputs
  At M3 if <60%: project-compass — reassess before continuing

╔══════════════════════════════════════════════════════════════╗
║  START TODAY:  T01 — Audit and prepare policy documents      ║
║                (highest risk; do first to validate feasibility)
║  WATCH FIRST:  T01 risk (PDF quality)                        ║
║  REVIEW AT:    M1 on Day 2                                   ║
╚══════════════════════════════════════════════════════════════╝
```
