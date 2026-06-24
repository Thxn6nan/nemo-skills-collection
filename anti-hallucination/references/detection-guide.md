# Detection Guide — Hallucination Signal Patterns

Detailed detection criteria for each of the 6 hallucination types.
Use this when running Mode 2 (DETECT) or reviewing agent output in Mode 3 (VERIFY).

---

## H1 — Numerical Hallucination

**Root cause:** LLMs are not calculators. Arithmetic is done via token prediction,
not actual computation. Multi-step math, percentages, and aggregations are especially
unreliable.

### Detection Signals

**Arithmetic signals**
- Result stated without intermediate steps shown
- Multi-step calculation where only the final answer appears
- Division involving non-round numbers (e.g., 7/13 = 0.5385 — verify)
- Compound percentage calculations (e.g., "grew 15% then 20%" → result is suspect)
- Running totals that don't match sum of parts

**Statistical signals**
- Percentage breakdown where parts don't sum to 100% (±0.5%)
- Average/median stated without showing data points
- Growth rate that implies different start/end values than stated elsewhere
- "Approximately X" where X is suspiciously round or convenient
- Percentile claims without reference to distribution

**Consistency signals**
- Same metric stated with different values in the same document
- Number contradicts a number in the provided source data
- Unit mismatch (thousands vs millions; quarterly vs annual)
- Number of items in a list that doesn't match stated count

### Verification Method for H1

```
ALWAYS re-compute in a code execution tool. Never accept LLM arithmetic.

Step 1: Extract the claim
  "Revenue grew from $4.2M to $5.1M, a 21.4% increase"

Step 2: Re-compute independently
  python: (5.1 - 4.2) / 4.2 * 100 = 21.43%  ✅ confirmed

Step 3: Check consistency
  4.2 * 1.214 = 5.098 ≈ 5.1  ✅ internally consistent

Step 4: Verify against source
  Is $4.2M and $5.1M from the provided data? Or generated?
```

### High-Risk Patterns

```python
# These agent outputs are almost always wrong — verify every time:
"The total across all categories is..."      # aggregation
"This represents a X% change from..."        # percentage change
"On average, users spend..."                 # mean calculation
"Approximately N times more than..."         # ratio/comparison
"The compound annual growth rate is..."      # CAGR (complex formula)
"Weighted average of..."                     # weighted mean
```

---

## H2 — Factual Hallucination

**Root cause:** LLMs interpolate between training data. When asked about specific
facts (exact dates, precise specifications, real names), they generate plausible-sounding
answers that may be entirely fabricated.

### Detection Signals

**Specificity signals** (paradoxically, high specificity = higher risk)
- Exact publication date for an obscure document
- Specific version numbers of software/standards
- Precise dimensions, weights, technical specifications
- Name of a specific person in a specific role at a specific time
- Exact regulatory code or statute number

**Temporal signals**
- Present-tense claims about things that change (prices, org structures, policies)
- "Currently" or "as of today" statements
- Recent events (post-training-cutoff period)
- Claims that a person "is" in a role (they may have left)

**Source absence signals**
- Fact stated without any attribution
- "It is known that..." with no reference
- "Studies show..." with no specific study named
- "Experts agree..." with no experts named

### Verification Method for H2

```
Decision tree for each factual claim:

Is this claim in the provided context/documents?
├── YES → quote the exact source text and confirm match
└── NO  ↓

Is this a verifiable external fact?
├── YES → web search or database lookup to confirm
└── NO (subjective/predictive) → flag as unverifiable, not a fact

Does the verification match the agent's claim?
├── YES → mark confirmed ✅
├── CLOSE → mark as approximate ⚠️ (note discrepancy)
└── NO  → mark as hallucination ❌
```

### Grounding Prompt Pattern

Add to agent system prompt for factual tasks:

```
FACTUAL GROUNDING RULES:
1. Every specific fact (name, date, number, specification) must be
   followed by [SOURCE: <document name>, <section/page>]
2. If you cannot cite a source, write [UNVERIFIED] instead of the fact
3. Do not infer facts from patterns in your training data
4. If uncertain: write "I don't have verified information on this"
   rather than providing an approximation
```

---

## H3 — Data Drift Hallucination

**Root cause:** When summarizing or extracting from source documents, agents
add, remove, or subtly alter meaning. The output sounds like the source but
no longer accurately represents it.

### Detection Signals

**Addition signals** (agent added content not in source)
- Summary contains specific claims not present in the original
- "The report recommends X" but source only describes X, doesn't recommend it
- Agent inferred causation from correlation in the source
- Conclusion that extends beyond what source data supports

**Alteration signals** (agent changed the meaning)
- Negation changed ("not recommended" → "recommended")
- Scope changed ("some users" → "most users" or "all users")
- Conditionality removed ("may cause" → "causes")
- Temporal shift ("was" → "is" or vice versa)
- Qualifier removed ("significantly higher in X conditions" → "higher")

**Omission signals** (agent dropped critical qualifiers)
- Important caveat missing ("effective only when combined with Y")
- Sample size or confidence interval not mentioned
- Study limitation not surfaced
- Exception case omitted

### Verification Method for H3

```
Line-by-line extraction check:

For each claim in agent summary:
  1. Find the specific sentence(s) in source that support this claim
  2. Compare: does the source sentence actually say what the claim says?
  3. Check for: additions, alterations, omissions

Scoring:
  Exact match (same meaning)         → ✅ ACCURATE
  Same meaning, different words       → ✅ ACCURATE (paraphrase ok)
  Meaning narrowed/qualified          → ⚠️ CHECK INTENT
  Meaning broadened/generalized       → ❌ DATA DRIFT
  Meaning reversed or inverted        → ❌ CRITICAL DRIFT
  No matching source sentence found   → ❌ FABRICATION
```

### Detection Prompt for Real-Time Use

Before accepting a data summary from an agent, probe with:

```
"For each conclusion in your summary, quote the exact sentence from 
the source document that supports it. Use the format:
CLAIM: <your summary claim>
SOURCE: '<exact quote from document>' (document name, section)"

If the agent cannot provide exact source quotes → hallucination risk is HIGH.
```

---

## H4 — Reasoning Hallucination

**Root cause:** LLMs generate plausible-sounding reasoning chains that may contain
hidden logical gaps, circular reasoning, or unwarranted assumptions. The output
sounds rigorous but the logic is broken.

### Detection Signals

**Gap signals**
- "Therefore" or "thus" with an unexplained jump between premise and conclusion
- Conclusion that is stronger than the premises support
- Missing step that the reader is expected to accept implicitly

**Assumption signals**
- Comparison without stating what's being compared to ("X is high")
- Relative claims without reference point ("significantly faster")
- Causal claim where only correlation was shown
- Assumes steady-state conditions without checking

**Circular signals**
- Conclusion used as evidence for itself at some point
- "This is the best approach because it achieves the best results"
- Definition smuggled into the reasoning ("since X is by definition Y...")

**Scope signals**
- Generalization from single case to all cases
- Conclusion about a population from a sample, without noting this
- Temporal conclusion from a snapshot ("users prefer X" from one-week data)

### Verification Method for H4

```
Reasoning chain decomposition:

1. List every reasoning step explicitly (the agent may have skipped some)
2. For each step, identify:
   - The premise (what is assumed or previously established)
   - The inference (what logical move is made)
   - The conclusion (what this step establishes)
3. Ask: Is the inference valid given the premise?
4. Ask: Is the premise actually established, or assumed?

RED FLAG: Any step where you must add words to make the logic work
          means the agent left a gap there.
```

### Chain-of-Thought Enforcement Prompt

```
For any recommendation or conclusion, structure your response as:

GIVEN: <explicitly stated premises — what facts you're starting from>
ASSUMING: <implicit assumptions you're making — state them explicitly>
REASONING:
  Step 1: [premise] → [inference] → [conclusion]
  Step 2: [previous conclusion] → [inference] → [new conclusion]
  ...
THEREFORE: <final conclusion>
CAVEAT: <conditions under which this reasoning breaks down>

If you cannot fill in ASSUMING without it seeming circular,
flag the reasoning as uncertain.
```

---

## H5 — Citation Hallucination

**Root cause:** LLMs were trained on text that contains citations, so they
generate citation-formatted text very convincingly. The citations may look
real but lead nowhere.

### Detection Signals

**Existence signals**
- Author name that sounds real but isn't searchable
- Journal name that doesn't exist or doesn't publish on this topic
- DOI or URL that was not in the retrieved documents
- Year of publication inconsistent with author's active period

**Content signals**
- Page number cited (LLMs don't reliably track page numbers)
- Exact quote that can't be found in the source
- Summary of cited work that contradicts what the work actually says
- Citation for a claim the cited work doesn't address

**Format signals**
- Citation format that mixes conventions (APA name with MLA structure)
- Volume/issue numbers that don't exist for that journal
- "et al." used for a paper with only 2 authors

### Verification Method for H5

```
For every citation:
1. Was this source in the documents provided to the agent?
   YES → locate and quote the relevant passage
   NO  → HIGH hallucination risk; verify externally before trusting

2. Does the source exist? (DOI lookup, Google Scholar, publisher site)
   EXISTS → continue
   NOT FOUND → CITATION HALLUCINATION confirmed ❌

3. Does the source say what the agent claims?
   MATCHES → ✅ confirmed
   DIFFERENT CLAIM → ❌ misattribution
   OPPOSITE CLAIM → ❌ critical error
```

**Mitigation rule:** For any agent doing citation work, provide source documents
explicitly. Prohibit citation of sources not provided. Prompt:

```
"Cite ONLY sources from the documents I have provided.
If you need a source that is not in the provided documents,
write [CITATION NEEDED] and describe what type of source would
support this claim. Never generate a citation from memory."
```

---

## H6 — Context Mix Hallucination

**Root cause:** In long contexts with multiple documents or multi-step tasks,
agents conflate information from different sources or assign attributes to the
wrong entity.

### Detection Signals

**Attribution signals**
- Statistic from Document A attributed to entity from Document B
- Quote from Person X attributed to Person Y
- Characteristic of Product A applied to Product B
- Finding from Quarter 1 presented as a Quarter 2 finding

**Entity confusion signals**
- Two people/companies/products with similar names treated as one
- Parent/subsidiary conflated
- Historical and current versions of the same entity conflated
- Aggregate attributed to a sub-component

**Step confusion signals**
- Output from Step 3 described as if it came from Step 1
- Earlier draft version cited instead of final version
- "As I mentioned" referring to something from a different task run

### Verification Method for H6

```
Source tagging check:
1. For each data point in the output, identify its claimed source
2. Locate that exact data point in the named source
3. Confirm the entity, date, and context match

Entity disambiguation check:
- List all similar entities in the context
- For each mention in the output, confirm which entity is meant
- Check that attributes assigned match the correct entity
```

### Prevention Prompt Pattern

For multi-document tasks, add to system prompt:

```
CONTEXT SEPARATION RULES:
- When referencing data, always name the source: "According to [Document Name]..."
- Never merge findings from different documents without explicitly noting this
- If two sources conflict, report both: "Document A says X; Document B says Y"
- Tag entities with identifiers: use full names, not pronouns, across documents
- If you're uncertain which document a piece of information came from, say so
```
