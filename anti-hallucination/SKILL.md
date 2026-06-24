---
name: anti-hallucination
description: >
  Detect, prevent, and verify hallucinations in agent outputs — especially numerical
  calculations, factual claims, data references, citations, and reasoning chains.
  Use this skill whenever an agent produces numbers, statistics, dates, names, quotes,
  or data-derived conclusions that could be fabricated or wrong. Trigger when the user
  says "verify this output", "the agent made up numbers", "check these calculations",
  "fact-check the agent", "the agent cited something wrong", "I don't trust this output",
  or when reviewing any agent output that will be used for decisions, reports, or
  communications. Also trigger proactively when designing agents that handle financial
  data, medical information, legal content, or any high-stakes numerical work.
  Anti-hallucination has three modes: PREVENT (before), DETECT (during), VERIFY (after).
---

# Anti-Hallucination Skill

Addresses the most dangerous failure mode in agent systems: outputs that are
confidently wrong. Hallucinations are not random errors — they follow patterns,
and those patterns can be anticipated, detected, and reduced.

## Core Principle

> An agent that admits uncertainty is more trustworthy than one that
> confidently fabricates. The goal is not zero uncertainty — it is
> zero unacknowledged uncertainty.

**The fundamental rule:** Never trust an agent's internal computation.
Verify externally. Always.

---

## Hallucination Taxonomy

Six types, each with a different cause and mitigation:

| Code | Type | Description | Highest Risk Contexts |
|------|------|-------------|----------------------|
| **H1** | **Numerical** | Wrong calculations, made-up statistics, arithmetic errors | Finance, analytics, reporting |
| **H2** | **Factual** | Invented names, dates, events, specifications | Research, documentation, support |
| **H3** | **Data Drift** | Misrepresenting source data — saying it says X when it says Y | Summarization, analysis, extraction |
| **H4** | **Reasoning** | Valid-sounding logic chain that reaches a wrong conclusion | Planning, diagnosis, recommendations |
| **H5** | **Citation** | Non-existent sources, wrong page numbers, wrong authors | Academic, legal, compliance |
| **H6** | **Context Mix** | Confusing information from different parts of a long context | Multi-document, multi-step tasks |

> ⚠️ H1 (Numerical) and H3 (Data Drift) are the most common in agent systems
> because agents are frequently asked to calculate and summarize.

---

## Three Operating Modes

```
PREVENT → design the agent to hallucinate less (before building/running)
DETECT  → catch hallucinations as they happen (during execution)
VERIFY  → audit output for hallucinations after the fact
```

Use the mode that matches when you're engaging with the agent task.

---

## Mode 1: PREVENT

Use when designing a new agent or improving an existing one.

### Step 1 — Classify the Risk Profile

What types of output will this agent produce?

```
HALLUCINATION RISK PROFILE:
□ Produces numbers or calculations?        → H1 risk — use code execution
□ States facts, names, dates?              → H2 risk — require grounding
□ Summarizes or extracts from documents?   → H3 risk — enforce citation
□ Makes recommendations or diagnoses?      → H4 risk — require reasoning trace
□ Cites external sources?                  → H5 risk — verify sources
□ Works across many documents or steps?    → H6 risk — use context anchoring
```

### Step 2 — Apply Prevention Patterns

For each risk identified, apply the relevant pattern from
`references/prevention-patterns.md`.

Key patterns by type:
- **H1** → Never compute in the LLM; route all math to a code execution tool
- **H2** → Ground every factual claim in a retrieved source before stating it
- **H3** → Require exact quotes before summaries; no paraphrase without source
- **H4** → Force chain-of-thought with explicit assumption declaration
- **H5** → Verify source exists before citing; include DOI/URL in citation
- **H6** → Tag data by source; prohibit cross-source inference without flagging

### Step 3 — Add Verification Gates

Build verification into the agent workflow, not as an afterthought:

```python
# Architecture pattern:
# 1. Agent generates output
# 2. Verification step runs before output is used
# 3. Only verified output proceeds downstream

agent_output = agent.run(task)
verified = verifier.check(agent_output, source_data)
if not verified.passed:
    raise HallucinationDetected(verified.findings)
```

---

## Mode 2: DETECT

Use during agent execution to catch hallucinations in real time.

### Detection Signals

Apply these checks to agent output as it is produced:

```
NUMERICAL (H1):
  🚩 Number appears without showing calculation
  🚩 Percentage that doesn't add up to 100 when it should
  🚩 Number contradicts a number seen earlier in the same context
  🚩 Suspiciously round number where precision is expected

FACTUAL (H2):
  🚩 Specific claim (name, date, spec) with no source reference
  🚩 Claim that contradicts provided source material
  🚩 Detail that is too specific to be from memory but too vague to verify
  🚩 Confident present-tense statement about things that change over time

DATA DRIFT (H3):
  🚩 Summary that goes beyond what the source says
  🚩 Paraphrase that changes the meaning of the original
  🚩 Agent says "the report shows X" but X is not in the report
  🚩 Inference presented as a direct finding

REASONING (H4):
  🚩 Conclusion stated without showing the reasoning steps
  🚩 Reasoning step that assumes what it's trying to prove
  🚩 Implicit comparison without stating what's being compared to
  🚩 "Therefore" or "thus" with a logical gap before it

CITATION (H5):
  🚩 Author name + title that sounds plausible but isn't verifiable
  🚩 Exact page number for a finding (LLMs don't reliably track pages)
  🚩 "According to [source]" where source wasn't in provided context
  🚩 URL that was not in the retrieved documents

CONTEXT MIX (H6):
  🚩 Attribute from Document A assigned to entity in Document B
  🚩 Agent merges two similar entities into one
  🚩 Finding from Step 3 incorrectly attributed to Step 1
  🚩 Cross-document conclusion stated as if from a single source
```

### Detection Output Format

```
HALLUCINATION ALERT [H<type>]
  Claim:    "<exact text from agent output>"
  Type:     H1 Numerical / H2 Factual / H3 Data Drift / H4 Reasoning / H5 Citation / H6 Context Mix
  Signal:   <which detection signal triggered>
  Risk:     [Low | Medium | High | Critical]
  Action:   [Verify | Flag | Reject | Rerun with constraint]
```

---

## Mode 3: VERIFY

Use after agent has produced output. Run systematic verification.

### Step 1 — Identify Verifiable Claims

Extract every claim from the output that could be verified:
- All numbers, percentages, totals, rates
- All factual statements (X happened, Y said Z, A is B)
- All data-derived conclusions ("the data shows...")
- All citations and source references

### Step 2 — Apply Verification Protocol

For each claim type, apply the protocol from
`references/verification-protocols.md`:

| Claim Type | Verification Method |
|------------|-------------------|
| Calculation | Re-run in code execution tool |
| Statistic | Trace back to source document |
| Factual claim | Cross-reference with provided sources |
| Data summary | Compare to original data line by line |
| Citation | Fetch and confirm source exists and says this |
| Reasoning chain | Re-derive conclusion from first principles |

### Step 3 — Score and Report

```
VERIFICATION SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Claims verified:   <N> / <Total>
Confirmed:         <N>  ✅
Unverifiable:      <N>  ⚠️ (no source to check against)
Contradicted:      <N>  ❌ (hallucination confirmed)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Hallucination Rate: <contradicted/total × 100>%

TRUST RATING:
  0%    confirmed contradictions → HIGH TRUST
  1–5%  contradictions → MODERATE (flag for review)
  6–15% contradictions → LOW (don't use without correction)
  >15%  contradictions → REJECT (agent output unreliable for this task)
```

---

## Severity by Use Case

Not all hallucinations have equal impact. Calibrate response accordingly:

| Use Case | Tolerable Rate | Action on Detection |
|----------|---------------|---------------------|
| Internal brainstorm | ~10% | Flag, continue |
| Customer-facing content | <2% | Verify all claims before publish |
| Financial reporting | 0% | Reject and rerun |
| Medical / Legal | 0% | Reject; require human review |
| Code generation | <1% logic errors | Test all code; never trust untested |

---

## Reference Files

- `references/detection-guide.md` — Detailed signal patterns for each H-type
- `references/prevention-patterns.md` — Prompt and architecture patterns that reduce hallucination
- `references/verification-protocols.md` — Step-by-step verification for each claim type

---

## Integration with Agent Governance Stack

```
anti-hallucination  ← Is the output truthful and accurate? (trust)
agent-engineering   ← Is the system well-engineered? (design quality)
agent-qa            ← Is the agent in scope? (behavior)
agent-audit         ← Are there defects? (correctness)
tenth-man           ← Are we missing risks? (adversarial)
```

---

## Example Invocations

```
"Verify the numbers in this agent report"
"The agent cited a paper I can't find — check it"
"Fact-check this summary the agent produced"
"Agent calculated the wrong total — set up verification"
"Design this financial agent to not make up numbers"
"The agent's conclusion doesn't match the data it summarized"
"Check if the agent mixed up data from different documents"
```
