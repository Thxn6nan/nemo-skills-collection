# Defect Checklists by Output Type

Use the relevant checklist for the type of output being audited.
Check each item: ✅ Pass | ❌ Fail (log as defect) | ⏭️ Not applicable

---

## Checklist 1: Code Output

### Correctness
- [ ] Output matches stated requirements
- [ ] Logic handles the happy path correctly
- [ ] Return values / types match expected
- [ ] No off-by-one errors in loops or index operations
- [ ] Math operations are correct (no integer overflow, division by zero risk)
- [ ] Boolean logic is correct (no inverted conditions)

### Edge Cases
- [ ] Handles empty input (empty string, [], {}, None/null)
- [ ] Handles single-element input
- [ ] Handles maximum/minimum values
- [ ] Handles unexpected input types (graceful failure, not crash)
- [ ] Handles concurrent access if applicable

### Error Handling
- [ ] All error paths have explicit handling (no silent failures)
- [ ] Exceptions are caught at the right level
- [ ] Error messages are informative (not just "error occurred")
- [ ] Resources are released on failure (files closed, connections released)
- [ ] No swallowed exceptions (`except: pass` patterns)

### Security
- [ ] No hardcoded credentials, tokens, or secrets
- [ ] User input is validated / sanitized before use
- [ ] No SQL injection vectors (parameterized queries used)
- [ ] No path traversal vulnerabilities
- [ ] Sensitive data not logged in plaintext

### Code Quality
- [ ] No leftover debug code (`print`, `console.log`, `debugger`, `TODO: remove`)
- [ ] No commented-out code blocks left in
- [ ] No duplicate logic that should be extracted
- [ ] No unused imports or variables
- [ ] No magic numbers (should be named constants)

### Tests (if agent was asked to write them)
- [ ] Tests actually test the code (not just calling it without assertions)
- [ ] Tests cover failure cases, not just happy path
- [ ] Test names describe what they verify
- [ ] No tests that always pass regardless of code behavior

---

## Checklist 2: Plan / Document Output

### Completeness
- [ ] All required sections are present
- [ ] No section is left as placeholder or "TBD" without justification
- [ ] All action items have owners (or "unassigned" is explicit)
- [ ] All action items have timelines (or noted as TBD with reason)

### Accuracy
- [ ] Factual claims are correct (verify any that seem surprising)
- [ ] Numbers, dates, and metrics are consistent throughout
- [ ] Referenced sources/links actually support the claims made
- [ ] No claims that contradict information provided in the task

### Internal Consistency
- [ ] Step 3 doesn't contradict Step 1
- [ ] Risk section covers risks mentioned in the plan body
- [ ] Assumptions in one section are consistent with those in another
- [ ] Timelines add up correctly

### Actionability
- [ ] Recommendations are concrete ("update the config" not "consider updating")
- [ ] Next steps are specific and executable
- [ ] Blockers are identified, not just implied

### Assumption Transparency
- [ ] Assumptions are stated explicitly, not buried in prose
- [ ] Key assumptions are validated or noted as unvalidated

---

## Checklist 3: Tool Call Sequences (Agentic Actions)

### Ordering
- [ ] Read operations happen before write operations that depend on them
- [ ] Dependencies between steps are respected (B runs after A if B needs A's output)
- [ ] Destructive operations (delete, overwrite) are not run before backups/checks

### Idempotency
- [ ] Running the sequence twice doesn't cause double-execution of side effects
- [ ] Create operations check for existence before creating
- [ ] Retry logic won't cause duplicates (payment, record creation, emails)

### Error Recovery
- [ ] Sequence handles partial failure (what happens if step 3 of 5 fails?)
- [ ] Rollback path exists for irreversible operations
- [ ] Failure in one branch doesn't silently skip remaining required steps

### Redundancy
- [ ] No API call made more than once unnecessarily
- [ ] No file read twice when result could be cached
- [ ] No search performed when result is already in context

### Side Effects
- [ ] All side effects (writes, sends, posts) were authorized by the task
- [ ] No side effects on systems outside the task scope
- [ ] External service calls are rate-limit-aware

---

## Checklist 4: Config / Data Files

### Schema Validity
- [ ] File parses without errors (JSON valid, YAML valid, etc.)
- [ ] All required fields are present
- [ ] No extra fields that would be rejected by the consumer
- [ ] Field names match the expected schema exactly (case-sensitive)

### Value Correctness
- [ ] No values outside allowed ranges (ports: 1-65535, percentages: 0-100, etc.)
- [ ] Enum values match exactly what the system accepts
- [ ] URLs are well-formed and use correct protocol
- [ ] Timestamps are in expected format and timezone

### Security
- [ ] No credentials, passwords, or API keys in the file
- [ ] Permissions/access levels are not overly broad
- [ ] No internal IP addresses or hostnames in files intended for external use

### Encoding / Format
- [ ] File encoding is UTF-8 (or as required)
- [ ] Line endings are consistent (LF vs CRLF as required)
- [ ] No BOM (Byte Order Mark) if not expected
- [ ] Numbers are not accidentally strings (and vice versa)
