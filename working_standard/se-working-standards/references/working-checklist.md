# Working Checklist — Before Completing Any Task

Run the relevant checklist before delivering output.
Check each item: ✅ Done | ⏭️ Not applicable | ❌ Issue found (fix before proceeding)

---

## Checklist C1: Code

### Reliability
- [ ] Every function that can fail has explicit error handling
- [ ] External calls have timeouts defined
- [ ] Retryable failures use backoff (not immediate retry loop)
- [ ] Non-retryable failures (400, auth errors) do NOT retry
- [ ] No path where failure is silently swallowed
- [ ] State that must survive a crash is persisted appropriately

### Security
- [ ] No credentials, tokens, or secrets in code or comments
- [ ] All user/external input is validated before use
- [ ] Logs do not contain PII or secrets
- [ ] File paths built from user input are sanitized
- [ ] SQL queries use parameterized form (no string concatenation)
- [ ] Permissions/scopes are minimum necessary

### Efficiency
- [ ] No operation repeated that was already performed this session
- [ ] Independent operations run in parallel where possible
- [ ] No O(n²) or worse where O(n) is available
- [ ] Large dataset operations use streaming or pagination

### Scalability
- [ ] No hardcoded limits on result sizes or collection lengths
- [ ] Database queries use appropriate indexes
- [ ] No global mutable state shared across concurrent operations
- [ ] Configurable values (timeouts, limits, batch sizes) use constants

### Maintainability
- [ ] Function and variable names reveal intent
- [ ] Non-obvious logic has a comment explaining *why*
- [ ] No magic numbers (use named constants)
- [ ] Each function has a single clear responsibility
- [ ] No leftover debug code, TODOs, or commented-out blocks

---

## Checklist C2: System or Architecture Design

### Reliability
- [ ] Single points of failure are identified
- [ ] Mitigation for each SPOF is defined (fallback, redundancy, graceful degradation)
- [ ] Partial failure scenarios are addressed (not just total failure)
- [ ] Data consistency on failure is defined

### Scalability
- [ ] Estimated scale at 10× current load is considered
- [ ] Bottlenecks at scale are identified and noted
- [ ] Stateless components can be horizontally scaled
- [ ] Data storage scales with volume (not a single flat file)

### Security
- [ ] Trust boundaries are defined (what trusts what)
- [ ] Data in transit is encrypted
- [ ] Data at rest protection is defined for sensitive data
- [ ] Access control model is documented

### Maintainability
- [ ] Component responsibilities are clearly separated
- [ ] Inter-component contracts (APIs, schemas) are defined
- [ ] Key architectural decisions have documented rationale
- [ ] Runbook or operational guide is planned

### Cost
- [ ] Cost drivers are identified
- [ ] Cost at current scale and at 10× scale is estimated
- [ ] Right-sizing decisions are documented

---

## Checklist C3: Data Pipeline

### Reliability
- [ ] Each step handles malformed or partial input explicitly
- [ ] Pipeline is idempotent — safe to re-run if interrupted
- [ ] Failed steps produce clear errors, not silent empty output
- [ ] Data integrity checks exist at critical boundaries

### Efficiency
- [ ] Large datasets are processed in chunks/streams, not all at once
- [ ] Redundant processing steps are eliminated
- [ ] Parallelization used where steps are independent

### Scalability
- [ ] Pipeline tested (or designed to work) at 10× current data volume
- [ ] No step that requires loading the full dataset into memory unnecessarily
- [ ] Partition strategy defined if data grows to require it

### Maintainability
- [ ] Each step has logging sufficient to diagnose failures
- [ ] Step inputs and outputs are typed/documented
- [ ] Configuration (file paths, thresholds, destinations) is externalized

---

## Checklist C4: API Integration

### Reliability
- [ ] HTTP 4xx errors handled (distinguish retryable from non-retryable)
- [ ] HTTP 5xx errors handled with retry + backoff
- [ ] Timeout defined on every HTTP call
- [ ] Response validation: confirm expected shape before using data

### Security
- [ ] Credentials from environment variables, not hardcoded
- [ ] Response data validated before use (don't trust external input)
- [ ] Sensitive data in requests/responses not logged

### Efficiency
- [ ] Results cached within session where data doesn't change
- [ ] Batch endpoint used if available and multiple records needed
- [ ] Parallel calls made where requests are independent

### Cost
- [ ] Call volume estimated and within API tier limits
- [ ] Unnecessary calls eliminated (caching, batching)
- [ ] Rate limiting handled gracefully (429 → backoff, not crash)

---

## Checklist C5: Plan or Specification

### User Satisfaction
- [ ] Plan addresses the real underlying need, not just the literal request
- [ ] Assumptions are stated explicitly (not buried in prose)
- [ ] Output format matches how the reader will use this
- [ ] Unclear or uncertain items are flagged, not papered over

### Reliability
- [ ] Failure scenarios are addressed, not just the happy path
- [ ] Rollback or recovery steps are defined for irreversible actions
- [ ] Dependencies and blockers are identified

### Maintainability
- [ ] Plan is structured so it can be updated as context changes
- [ ] Decision rationale is documented (why this approach, not another)
- [ ] Ownership and next steps are concrete and actionable

---

## Universal Pre-Delivery Check (Run for Any Task)

Before submitting any output, answer these 5 questions:

```
1. CORRECTNESS
   "Does this actually solve what was asked?"
   If NO or UNSURE → fix or flag before delivering

2. RELIABILITY
   "What happens when something in this goes wrong?
    Is that behavior acceptable?"
   If NO → add error handling before delivering

3. SECURITY
   "Is there any credential, PII, or sensitive data
    visible in this output that shouldn't be?"
   If YES → remove before delivering

4. CLARITY
   "Would someone new understand what this does and why?"
   If NO → add explanation or rename for clarity

5. COMPLETENESS
   "Is there anything the person receiving this needs to know
    that I haven't told them?"
   If YES → add it before delivering
```

If all 5 are satisfied → deliver.
If any are not → resolve before delivering.
