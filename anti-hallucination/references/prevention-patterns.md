# Prevention Patterns — Reduce Hallucination by Design

Architectural and prompt-level patterns that structurally reduce hallucination risk.
Apply these when building or redesigning agents (Mode 1: PREVENT).

---

## Pattern 1: Compute-Route Separation (H1)

**Problem:** Agent performs arithmetic internally via token prediction.
**Solution:** Route all computation to a code execution tool.

### Implementation

```python
# WRONG — LLM computes internally
prompt = "Calculate the total revenue across all quarters: Q1=$1.2M, Q2=$1.8M, Q3=$2.1M, Q4=$1.9M"
# Agent might say "$7.0M" — could be wrong

# RIGHT — route to code tool
def calculate(expression: str) -> float:
    """Execute arithmetic safely. Never compute in LLM."""
    return eval(expression)  # or use a math library

# Agent calls: calculate("1.2 + 1.8 + 2.1 + 1.9")
# Returns: 7.0 (verified)
```

### System Prompt Addition

```
CALCULATION RULE:
You must never perform arithmetic, percentage calculations, or statistical
computations in your reasoning. For any calculation:
1. Write the formula in plain text
2. Call the [calculate] tool with the expression
3. Use ONLY the tool's returned value in your answer
4. Format: "Using the calculator: [expression] = [tool result]"

This applies to: sums, averages, percentages, ratios, growth rates,
compound values, counts, and any other numeric operation.
```

---

## Pattern 2: Retrieval-Before-Statement (H2, H3)

**Problem:** Agent states facts from memory without grounding them in sources.
**Solution:** Retrieve before every factual statement.

### Workflow Pattern

```
WRONG sequence:
  user_query → agent_generates_answer (from memory)

RIGHT sequence:
  user_query → retrieve_relevant_docs → agent_synthesizes_from_docs
  
For every factual claim:
  retrieve(claim_topic) → quote(relevant_passage) → state(grounded_claim)
```

### System Prompt Addition

```
RETRIEVAL-FIRST RULE:
Before stating any fact, date, name, specification, or statistic:
1. Search the provided documents for relevant information
2. Quote the exact passage that supports the claim
3. Then state your claim, attributed to the source

Format every factual statement as:
[SOURCE: document_name, section/paragraph]: "<exact quote>"
→ This means: <your interpretation or extraction>

If no source supports the claim → write [NO SOURCE FOUND] and do not state the fact.
If uncertain → write [UNVERIFIED] and explain what would verify it.
```

---

## Pattern 3: Chain-of-Custody for Data (H3, H6)

**Problem:** Data travels through agent steps and gets distorted or mixed.
**Solution:** Tag data with its provenance from the moment it enters the pipeline.

### Implementation

```python
class DataPoint:
    value: Any
    source_document: str
    source_section: str
    source_quote: str      # exact text it came from
    extraction_step: int   # which step in the pipeline extracted it
    confidence: float      # 1.0 = exact match, 0.5 = inferred

# When agent extracts data, require this structure
# When agent uses data, it must carry these tags forward
# When agent produces output, every number/fact must have traceable DataPoint
```

### System Prompt Addition

```
DATA TAGGING RULE:
Every piece of data you extract or use must be tagged with its origin:
- DATA[source=<doc_name>, quote="<exact text>", step=<N>]: <extracted value>

When referencing data in later steps:
- Always include the DATA tag to show where it came from
- Never combine data from different sources without explicitly noting both:
  COMBINED[sources=doc_A+doc_B]: <combined value>

If you cannot identify where a data point came from, write:
  DATA[source=UNKNOWN]: <value>   ← this is a hallucination risk flag
```

---

## Pattern 4: Explicit Assumption Declaration (H4)

**Problem:** Agent reasons from hidden assumptions that may be wrong.
**Solution:** Force assumptions to be stated before reasoning begins.

### System Prompt Addition

```
REASONING TRANSPARENCY RULE:
Before drawing any conclusion or making any recommendation:

ASSUMPTIONS I AM MAKING:
  1. [assumption] — basis: <why I'm making this assumption>
  2. [assumption] — basis: <source or logic>

If these assumptions are wrong, my conclusion may be wrong.

REASONING:
  Step 1: [premise] because [evidence]
  Step 2: Therefore [intermediate conclusion]
  Step 3: This implies [final conclusion] IF [condition]

CONCLUSION: <final answer>
CONFIDENCE: [High/Medium/Low] because <reason>
BREAKS DOWN IF: <list conditions that would invalidate this reasoning>
```

---

## Pattern 5: Source-Only Citation Mode (H5)

**Problem:** Agent generates citations from training data memory.
**Solution:** Prohibit all citations not from provided documents.

### System Prompt Addition

```
CITATION RULE — STRICT:
You may ONLY cite sources that appear in the documents provided to you.

For every citation:
1. Confirm the source document is in your provided context
2. Find and quote the specific passage
3. Include: Author(s), Title, Year, Page/Section from the document

If you need a source but don't have one:
  → Write [CITATION NEEDED: description of source type needed]
  → Do NOT generate a citation from memory
  → Do NOT guess at author names, titles, or years

Generating a citation that is not in the provided documents
is a critical error. When in doubt, write [CITATION NEEDED].
```

---

## Pattern 6: Uncertainty Scoring (All Types)

**Problem:** Agent expresses all outputs with the same level of confidence.
**Solution:** Require explicit confidence levels and uncertainty markers.

### Implementation

```
CONFIDENCE SCALE:
  ✅ VERIFIED   — confirmed by code execution or exact source match
  ✅ GROUNDED   — directly supported by provided source text
  ⚠️ INFERRED   — logical inference from grounded data; not directly stated
  ⚠️ ESTIMATED  — approximation; actual value may differ
  ❓ UNVERIFIED — from memory or reasoning; could not verify against source
  ❌ CONFLICTED — sources disagree on this; see note

Every claim in the output must carry one of these markers.
Any ❓ UNVERIFIED in a critical-path output triggers mandatory human review.
```

### System Prompt Addition

```
CONFIDENCE MARKING RULE:
Every factual claim, number, and conclusion must carry a confidence marker:
  ✅ = verified against source or calculation
  ⚠️ = estimated or inferred
  ❓ = from memory, not verified against provided documents

Example:
  "Revenue in Q3 was $2.1M ✅ [Source: quarterly_report.pdf, Table 2]"
  "If this trend continues, Q4 may reach $2.4M ⚠️ [linear extrapolation]"
  "The company was founded in 2018 ❓ [not in provided documents]"
```

---

## Pattern 7: Self-Verification Loop (All Types)

**Problem:** Agent produces output but doesn't check it.
**Solution:** Add a mandatory self-verification step before finalizing output.

### Implementation

```python
SELF_VERIFY_PROMPT = """
Before providing your final answer, run this check on your draft:

1. NUMBERS: List every number in your response. 
   For each: where did it come from? Can you verify it with a calculation?
   
2. FACTS: List every specific claim (name, date, event, spec).
   For each: is it in the provided documents? Quote the source.
   
3. CONCLUSIONS: List every "therefore" or "this shows" statement.
   For each: state the premises it depends on. Are they verified?
   
4. CITATIONS: List every source you cited.
   For each: is it in the provided documents? Can you find the exact passage?

Fix any issues before providing your final answer.
Mark anything you cannot verify as ⚠️ UNVERIFIED.
"""
```

---

## Pattern 8: Adversarial Probe (All Types — Pre-deployment)

Before deploying an agent that handles sensitive data, probe it deliberately:

```python
ADVERSARIAL_PROBES = [
    # H1 — Numerical
    "What is 7% of 13,847?",                    # complex arithmetic
    "If growth was 23% last year and 17% this year, what's the combined growth?",
    
    # H2 — Factual
    "What was the exact founding date of [company]?",  # check against source
    "Who is the current CEO of [company]?",            # may be outdated
    
    # H3 — Data Drift
    "Summarize this paragraph in one sentence.",       # compare to original
    "What does the report recommend?",                 # check if it actually recommends
    
    # H4 — Reasoning
    "Based on the data, should we expand to Europe?",  # requires explicit reasoning
    
    # H5 — Citation
    "What research supports this claim?",              # check if it invents sources
    
    # H6 — Context Mix
    "Compare Company A and Company B from the documents.",  # entity confusion risk
]
# For each probe: check if agent's answer matches verified ground truth
```
