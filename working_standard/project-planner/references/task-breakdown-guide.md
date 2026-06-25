# Task Breakdown Guide — Decomposition Patterns by Project Type

---

## Pattern 1: Feature / Capability Build

Use when: building a new piece of functionality that doesn't exist yet.

```
Standard decomposition order:

1. DEFINE (before any code)
   □ Define the interface/contract first (API spec, function signatures, schema)
   □ Write acceptance criteria — what does "working" look like exactly?
   □ Identify all input types and edge cases upfront
   → Skipping this produces rework. 1-2 hours here saves days later.

2. DATA / SCHEMA (if data is involved)
   □ Define data model or schema changes
   □ Write migration (if modifying existing data)
   □ Seed test data that covers edge cases
   → Data decisions made during coding are always more expensive.

3. CORE IMPLEMENTATION
   □ Happy path first — get the main flow working
   □ One unit at a time — function/class/module level
   □ Each unit testable in isolation

4. ERROR HANDLING (not optional, not last)
   □ Every external call: what if it fails?
   □ Every input: what if it's malformed?
   □ Every state: what if it's in an unexpected state?
   → Error handling is approximately 30-40% of the real work. Plan for it.

5. INTEGRATION
   □ Connect components together
   □ Integration tests (end-to-end flow)
   □ Test with realistic data volumes (not just toy examples)

6. HARDENING
   □ Performance: is it fast enough at realistic load?
   □ Security: does it follow security principles from working-standards?
   □ Observability: are there sufficient logs to debug failures?
```

---

## Pattern 2: System / Architecture Design

Use when: designing how components connect before building them.

```
Decomposition order:

1. REQUIREMENTS CLARIFICATION
   □ What does the system need to do? (functional requirements)
   □ What constraints must it respect? (non-functional: speed, scale, cost)
   □ What does it NOT need to do? (explicit exclusions)

2. COMPONENT IDENTIFICATION
   □ What are the major components?
   □ What does each component own?
   □ What are the boundaries between components?

3. INTERFACE DESIGN
   □ How do components communicate? (API contracts, event schemas, data formats)
   □ What is each component's input and output?
   □ Document before implementing — interfaces are expensive to change later.

4. FAILURE MODE ANALYSIS
   □ What happens when component X fails?
   □ What happens when the network between X and Y is slow?
   □ What happens when data is malformed at the boundary?

5. PROOF OF CONCEPT (for uncertain parts)
   □ Identify the highest-uncertainty design decision
   □ Build the smallest possible thing that answers whether it works
   □ Decide before full implementation

6. DOCUMENTATION
   □ Architecture diagram
   □ Decision log: why this design, not alternatives
   □ Runbook: what to do when it breaks
```

---

## Pattern 3: Data Pipeline

Use when: building a process that transforms, moves, or processes data.

```
Decomposition order:

1. DATA AUDIT (before any code)
   □ What does the input data actually look like? (sample it, don't assume)
   □ What is the null/missing rate on critical fields?
   □ What are the edge cases in the real data?
   → Real data is always messier than expected. Audit first.

2. SCHEMA DEFINITION
   □ Define input schema (with validation)
   □ Define output schema
   □ Define transformation rules explicitly

3. TRANSFORMATION LOGIC (one step at a time)
   □ Build and test each transformation step independently
   □ Log input/output of each step for debugging
   □ Handle bad records explicitly (skip? error? quarantine?)

4. IDEMPOTENCY
   □ Can the pipeline be re-run safely if it fails mid-way?
   □ Are writes protected against duplication?
   □ Test: run it twice, confirm output is identical

5. VOLUME TESTING
   □ Test at 10× expected volume before declaring done
   □ Identify where performance degrades
   □ Add pagination/streaming where needed

6. MONITORING
   □ Record count at each step (easy to detect where records are lost)
   □ Error rate per step
   □ Run duration alerts (detect when pipeline slows abnormally)
```

---

## Pattern 4: Agent / Automation Build

Use when: building an agent workflow or automated process.

```
Decomposition order:

1. TASK DEFINITION
   □ What is the exact task the agent will perform?
   □ What are the inputs? (format, source, validation)
   □ What are the outputs? (format, destination, quality criteria)
   □ What tools does the agent need? (list them; don't over-provision)

2. TOOL IMPLEMENTATION (before the agent prompt)
   □ Build and test each tool independently
   □ Each tool should work correctly when called directly
   □ Tools should return structured, consistent output
   → Never build the prompt before the tools work.

3. PROMPT DEVELOPMENT (iterative)
   □ Start with the simplest prompt that could work
   □ Test on 5-10 representative examples
   □ Add complexity only when simple version fails
   □ Document why each prompt element is there

4. SCOPE BOUNDARY DEFINITION
   □ What should the agent never do? (explicit prohibitions)
   □ What requires human approval before the agent acts?
   □ Define using project-qa PRE mode

5. FAILURE HANDLING
   □ What does the agent do when a tool returns an error?
   □ What does the agent do when it's uncertain?
   □ How does the agent signal that it needs help?

6. EVALUATION
   □ Define a test set: 20+ examples covering edge cases
   □ Establish baseline metrics before optimizing
   □ Run anti-hallucination checks on outputs with data/numbers
   □ Run work-audit on sample outputs before production use

7. MONITORING
   □ Log all tool calls with inputs and outputs
   □ Track success rate, error rate, and hallucination rate
   □ Set alerts for anomalous behavior
```

---

## Pattern 5: Refactor / Technical Debt

Use when: improving existing code without changing external behavior.

```
Decomposition order:

1. CHARACTERIZATION (before touching anything)
   □ Write tests that describe the CURRENT behavior (even if it's wrong)
   □ These tests are your safety net — they tell you if you break something
   → Never refactor without a test suite. This is non-negotiable.

2. SCOPE DEFINITION
   □ What exactly will change? (be specific — a list of files/functions)
   □ What will NOT change? (explicit exclusions)
   □ Use project-qa to define and enforce scope

3. SMALL INCREMENTS
   □ One change at a time, tests passing at each step
   □ Commit after each working increment
   □ Never have two refactors in flight simultaneously

4. VERIFICATION AT EACH STEP
   □ Run tests after each change
   □ Confirm external behavior is unchanged
   □ Run work-audit on changed code

5. DOCUMENTATION UPDATE
   □ Update comments that described the old approach
   □ Note what changed and why in commit messages
   □ Update any architecture diagrams if structure changed
```

---

## Hidden Task Checklist (Apply to Any Project Type)

These are the tasks most often missing from initial plans:

```
ENVIRONMENT & ACCESS
□ Dev environment setup for any new developer joining
□ Required credentials, API keys, or permissions (and who approves them)
□ External service accounts or sandbox environments

DECISIONS THAT MUST HAPPEN BEFORE CODING
□ Naming conventions for new concepts
□ Data model decisions (changing these later is expensive)
□ API contract decisions (changing these breaks callers)

TESTING (typically underestimated by 50%)
□ Unit tests for each component
□ Integration tests for each connection
□ Edge case tests for each input type
□ Load/performance test if scale matters

REVIEW AND ITERATION
□ Code review time (typically 4-8 hours per major component)
□ Time to address review feedback
□ Second review if significant changes made

COORDINATION
□ Meetings/syncs required with other teams
□ Approval cycles if changes affect external systems
□ Dependency waits (external API access, data from another team)

POST-COMPLETION
□ Documentation for the next person
□ Handoff walkthrough if someone else will maintain this
□ Monitoring setup and alert configuration
```
