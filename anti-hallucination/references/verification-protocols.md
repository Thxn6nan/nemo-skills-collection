# Verification Protocols — Claim-by-Claim Verification

Structured protocols for verifying each type of claim after agent output is produced.
Use in Mode 3 (VERIFY) or as part of an automated verification pipeline.

---

## Protocol V1: Numerical Verification

**Principle:** Every number must be independently computed. Never accept LLM arithmetic.

### Step-by-Step

```
INPUT: A numerical claim from agent output

STEP 1 — Identify claim type
  □ Simple arithmetic (A + B, A × B)
  □ Percentage calculation (X% of Y, change from A to B)
  □ Statistical measure (mean, median, standard deviation)
  □ Compound/multi-step (CAGR, weighted average, IRR)
  □ Aggregation (sum/count over a dataset)

STEP 2 — Extract all input values
  List every number the calculation depends on.
  For each: is it in the provided source data?
  Mark numbers NOT in source data as [UNVERIFIED INPUT] — high risk.

STEP 3 — Re-execute in code
  Python (preferred for precision):
  
    # Simple arithmetic
    result = 1200 + 1800 + 2100 + 1900   # = 7000, not 7000.something
    
    # Percentage change
    pct_change = (new - old) / old * 100  # use float division
    
    # Mean
    import statistics
    mean = statistics.mean([1200, 1800, 2100, 1900])
    
    # CAGR
    cagr = (end_value / start_value) ** (1/years) - 1

STEP 4 — Compare to agent's claim
  □ Agent claimed: [value]
  □ Verified result: [computed value]
  □ Match: YES / CLOSE (±X%) / NO
  
  Acceptable tolerance:
    Financial values: 0% tolerance (exact match required)
    Estimates/projections: ±1% tolerance
    Statistical approximations: document the tolerance

STEP 5 — Document
  NUMERICAL CLAIM: "<agent's exact claim>"
  INPUTS: [list all input values and their sources]
  COMPUTED: [your independent result]
  STATUS: ✅ VERIFIED / ⚠️ WITHIN TOLERANCE / ❌ CONTRADICTION
  IMPACT: [what decision or output depends on this number being correct]
```

### Common Pitfalls to Check

```python
# Percentage pitfall — agent may confuse these:
"Revenue grew 15%" could mean:
  absolute_new = old * 1.15    # correct: grew BY 15%
  absolute_new = old + 15      # wrong: grew BY $15 (if units are dollars)

# Percentage point vs percentage:
"Rate increased from 20% to 25%" =
  5 percentage points increase ≠ 25% increase
  actual % increase = (25-20)/20 * 100 = 25%

# Compounding vs simple:
"10% for 3 years" =
  compound: 1.10^3 = 1.331 (33.1% total)
  simple: 1.30 (30% total)
  Check which the agent used.

# Rounding errors in multi-step calculations:
Round only at the END, not at each intermediate step.
```

---

## Protocol V2: Factual Verification

**Principle:** Every specific fact must be traceable to a provided source or externally verified.

### Step-by-Step

```
INPUT: A factual claim from agent output
       e.g., "The company was founded in 2015 by Jane Smith"

STEP 1 — Classify the fact
  □ Person (name, role, affiliation)
  □ Organization (founding, structure, ownership)
  □ Event (date, participants, outcome)
  □ Specification (technical, regulatory, contractual)
  □ Statistical/empirical finding

STEP 2 — Check provided documents first
  Search provided context for every term in the claim.
  □ Found exact match → quote it → ✅ GROUNDED
  □ Found partial match → note discrepancy → ⚠️ VERIFY FURTHER
  □ Not found in documents → must verify externally → UNVERIFIED

STEP 3 — External verification (if not in documents)
  □ Primary source (official website, regulatory filing, peer-reviewed paper)
  □ Secondary source (reputable news, established database)
  □ No reliable source found → ❌ UNVERIFIABLE — do not use

STEP 4 — Temporal check
  Is this claim time-sensitive?
  □ Roles, prices, policies, product specs → verify as of the relevant date
  □ "Current" or "now" → agent's training data may be outdated
  □ Post-cutoff events → never trust agent memory; external source required

STEP 5 — Document
  FACTUAL CLAIM: "<agent's exact claim>"
  SOURCE: [document name + exact quote, OR external URL]
  VERIFIED DATE: [when you verified]
  STATUS: ✅ CONFIRMED / ⚠️ APPROXIMATE / ❌ INCORRECT / ❓ UNVERIFIABLE
```

---

## Protocol V3: Data Summary Verification

**Principle:** Every claim derived from a document must be locatable as a direct
quote or unambiguous inference from the source text.

### Step-by-Step

```
INPUT: A summary or data-extraction claim from agent output
       e.g., "The report recommends increasing headcount by 15%"

STEP 1 — Locate the source passage
  For the claim: find the exact sentence(s) in the source document
  that the agent used to make this claim.
  
  If you CANNOT find a source sentence → HIGH HALLUCINATION RISK → ❌

STEP 2 — Apply the drift test
  Compare the source passage to the agent's claim:
  
  SOURCE: "The analysis suggests considering a headcount expansion 
           of approximately 10-20% subject to Q3 results"
  
  CLAIM:  "The report recommends increasing headcount by 15%"
  
  Drift analysis:
    "suggests considering" → "recommends"              DRIFT: conditional → definitive
    "approximately 10-20%" → "15%"                     DRIFT: range → single point
    "subject to Q3 results" → [omitted]                DRIFT: key qualifier removed
  
  VERDICT: ❌ DATA DRIFT — agent strengthened and narrowed the claim

STEP 3 — Classify the drift (if found)
  □ Omission of qualifier (e.g., "possibly" dropped) — MEDIUM risk
  □ Scope expansion ("some" → "all") — HIGH risk
  □ Conditionality removed ("if X then Y" → "Y") — HIGH risk
  □ Direction reversed ("may decrease" → "increases") — CRITICAL
  □ Inference presented as direct finding — MEDIUM risk

STEP 4 — Document
  AGENT CLAIM: "<exact text>"
  SOURCE PASSAGE: "<exact quote from source document, with location>"
  DRIFT FOUND: YES / NO
  DRIFT TYPE: [omission / expansion / reversal / inference / none]
  CORRECTED CLAIM: "<what the claim should say, faithful to the source>"
```

---

## Protocol V4: Reasoning Chain Verification

**Principle:** Every logical step in a conclusion must be individually valid,
and no step may assume what it's trying to prove.

### Step-by-Step

```
INPUT: A conclusion or recommendation from agent output

STEP 1 — Make the reasoning explicit
  If the agent didn't show reasoning steps, reconstruct them:
  "The agent concluded X. To get there, it must have assumed/inferred..."
  
  Explicit reasoning is easier to verify. If you can't reconstruct the
  reasoning chain, the conclusion is unverifiable.

STEP 2 — Verify each step
  For each reasoning step:
  
    PREMISE: What is this step starting from?
      □ Is the premise established by evidence? Or assumed?
      □ If assumed: is the assumption stated? Is it reasonable?
    
    INFERENCE: What logical move is made?
      □ Deductive: if premises are true, conclusion must be true
      □ Inductive: if premises are true, conclusion is likely
      □ Abductive: this explanation best fits the evidence
      Note which type — inductive and abductive can be wrong even if valid
    
    CONCLUSION: What does this step establish?
      □ Does the conclusion follow from the premise + inference?
      □ Is the conclusion stronger than the inference warrants?

STEP 3 — Check for fallacies
  Common reasoning hallucinations:
  □ Correlation → causation: "X and Y occurred together, so X caused Y"
  □ Hasty generalization: "This happened in 2 cases, so it always happens"
  □ False dichotomy: "Either X or Y, and not X, so Y" (ignores other options)
  □ Appeal to sample size: "Our survey of 10 users shows..."
  □ Base rate neglect: conclusion ignores how common/rare the prior is

STEP 4 — Document
  CONCLUSION CLAIMED: "<agent's conclusion>"
  REASONING CHAIN: [list each step]
  STEP WITH ISSUE: [which step is broken]
  ISSUE TYPE: [gap / assumption / fallacy / overgeneralization]
  CORRECTED CONCLUSION: "<what can actually be concluded from the evidence>"
```

---

## Protocol V5: Citation Verification

**Principle:** Every citation must be confirmed to exist and to support the claim made.

### Step-by-Step

```
INPUT: A citation from agent output
       e.g., Smith, J. & Lee, K. (2021). "AI Governance Frameworks." 
             Journal of AI Policy, 14(2), 45-67.

STEP 1 — Check if source was provided
  □ Is this document in the provided context? → retrieve and verify
  □ Not in provided context → external verification required

STEP 2 — Verify existence
  □ Search Google Scholar / DOI lookup / publisher site
  □ Author name exists and publishes in this field?
  □ Journal exists and covers this topic?
  □ Year and volume/issue are consistent?
  □ Title matches?
  
  If source NOT FOUND → ❌ CITATION HALLUCINATION

STEP 3 — Verify content (if source exists)
  □ Find the specific claim being cited
  □ Does the cited work actually say what the agent claims?
  □ Is the cited section in the page range given?
  □ Is the claim the main finding, or a minor caveat?

STEP 4 — Document
  CITATION: "<agent's citation>"
  EXISTS: YES / NO / COULD NOT VERIFY
  SUPPORTS CLAIM: YES / PARTIALLY / NO / NOT VERIFIABLE
  STATUS: ✅ VALID / ⚠️ INACCURATE / ❌ HALLUCINATED
  
  If hallucinated: note whether the claim itself might still be supportable
  with a different (real) source.
```

---

## Verification Report Template

Use this to summarize all verification findings for a single agent output:

```
╔═══════════════════════════════════════════════════════════╗
║  HALLUCINATION VERIFICATION REPORT                        ║
╚═══════════════════════════════════════════════════════════╝

OUTPUT REVIEWED: <description of agent output>
PROTOCOLS USED:  V1 Numerical / V2 Factual / V3 Data / V4 Reasoning / V5 Citation

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CLAIMS INVENTORY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Numerical claims:   <N total>
Factual claims:     <N total>
Data summaries:     <N total>
Reasoning chains:   <N total>
Citations:          <N total>
─────────────────────────────
Total verifiable:   <N>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
VERIFICATION RESULTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ CONFIRMED:     <N> claims verified against source or computation
⚠️ UNVERIFIABLE:  <N> claims (no source to check against)
⚠️ APPROXIMATE:   <N> claims (correct direction, imprecise value)
❌ CONTRADICTED:  <N> claims verified as WRONG

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HALLUCINATION FINDINGS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

FINDING-1 [H<type>] <short title>
  Claim:     "<agent's text>"
  Evidence:  "<source text or computation result>"
  Impact:    <what downstream use is affected>
  Severity:  [Critical / High / Medium / Low]
  Fix:       <corrected claim>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TRUST RATING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Hallucination Rate: <contradicted / total verifiable × 100>%

  0%      → ✅ HIGH TRUST — output can be used
  1–5%    → ⚠️ MODERATE — review flagged items before use
  6–15%   → ⚠️ LOW TRUST — correct all findings before use
  >15%    → ❌ REJECT — do not use this output; rerun with stronger constraints

╔═══════════════════════════════════════════════════════════╗
║  VERDICT: [HIGH TRUST | MODERATE | LOW TRUST | REJECT]   ║
╚═══════════════════════════════════════════════════════════╝
```
