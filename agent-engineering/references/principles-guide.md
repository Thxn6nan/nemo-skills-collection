# Principles Guide — 8 SE Principles for Agent Systems

Each principle has: definition, scoring criteria, agent-specific anti-patterns,
and concrete improvement actions. Read only the sections relevant to your assessment.

---

## 1. Reliability (Weight: 20%)

**Definition:** The agent produces correct, consistent results under real-world
conditions — including partial failures, unexpected inputs, and system degradation.

### Scoring Criteria

| Score | What it looks like |
|-------|-------------------|
| 9–10 | Graceful degradation; circuit breakers; retry with backoff; full observability; <1% error rate under load |
| 7–8 | Handles most failures; retries exist; errors are logged and alerted |
| 5–6 | Works under normal conditions; known failure modes not yet handled |
| 3–4 | Occasional silent failures; no retry logic; errors surface late |
| 0–2 | Frequent failures; no error handling; failures are invisible |

### Key Criteria to Evaluate

**Error Handling**
- [ ] All tool call failures are explicitly handled (not ignored)
- [ ] Agent distinguishes retryable vs. non-retryable errors
- [ ] Exponential backoff with jitter on retries
- [ ] Maximum retry limit to prevent infinite loops
- [ ] Partial failures are detected and don't produce silent wrong output

**Consistency**
- [ ] Same input produces same category of output across runs
- [ ] Agent behavior is predictable under prompt variation
- [ ] State is managed correctly across multi-step tasks
- [ ] No race conditions in concurrent tool calls

**Observability**
- [ ] Agent logs key decision points (not just errors)
- [ ] Tool call outcomes are recorded with inputs and outputs
- [ ] Failure rates and latency are measurable
- [ ] Alerts exist for anomalous behavior

**Degradation**
- [ ] Agent has fallback behavior when external services fail
- [ ] Partial results are surfaced (not suppressed) when a step fails
- [ ] Agent signals uncertainty rather than producing confident wrong output

### Anti-Patterns 🚩
- Silent `try/except` that swallows errors and continues
- Retry on non-retryable errors (e.g., 400 Bad Request)
- No timeout on external tool calls
- State stored only in memory (lost on crash)
- "Succeeded" logged even when output was empty/null

### Improvements
- Add circuit breaker: stop calling a failing service after N failures
- Implement structured logging with trace IDs
- Add health checks that verify end-to-end agent flow, not just uptime
- Write chaos tests: what happens when tool X returns 500?

---

## 2. Security & Trust (Weight: 15%)

**Definition:** The agent handles sensitive data responsibly, operates with minimum
necessary privilege, resists manipulation, and maintains auditability of its actions.

### Scoring Criteria

| Score | What it looks like |
|-------|-------------------|
| 9–10 | Zero secrets in prompts/logs; least-privilege tools; prompt injection resistant; full audit trail; data minimization |
| 7–8 | Secrets managed properly; most inputs validated; audit log exists |
| 5–6 | Known gaps but no active vulnerabilities; secrets partially exposed |
| 3–4 | Credentials in prompts or logs; no input validation; wide permissions |
| 0–2 | Critical vulnerabilities: exposed secrets, no access control, no audit trail |

### Key Criteria to Evaluate

**Secrets & Credentials**
- [ ] No API keys, passwords, or tokens in agent prompts or system context
- [ ] Secrets loaded from environment/vault, not hardcoded
- [ ] Secrets not logged even at DEBUG level
- [ ] Token rotation and expiry managed

**Principle of Least Privilege**
- [ ] Agent tools provide minimum necessary access
- [ ] Write/delete permissions granted only where required
- [ ] Agent cannot access data outside its task scope
- [ ] Tool permissions are scoped per task, not global

**Input Validation & Prompt Injection**
- [ ] External content (web pages, user files, API responses) is not treated as instructions
- [ ] User input is sanitized before being embedded in agent context
- [ ] Agent refuses requests that exceed its authorization
- [ ] Indirect prompt injection vectors are identified and blocked

**Auditability**
- [ ] Every significant agent action is logged with timestamp, actor, and outcome
- [ ] Audit logs are tamper-evident and retained appropriately
- [ ] Sensitive operations require explicit confirmation before execution
- [ ] Agent cannot take irreversible actions without a safety gate

**Data Handling**
- [ ] PII is not logged or included in prompts unnecessarily
- [ ] Data minimization: agent only receives data it needs
- [ ] Output doesn't leak data from one user/context to another

### Anti-Patterns 🚩
- `system_prompt = f"Your API key is {api_key}"` — key in context
- Agent given `admin: true` tool access for a read-only task
- User-provided URLs fetched and content injected into agent instructions
- No distinction between "agent decided to do X" vs "user asked for X" in logs
- Same agent context shared across different users

### Improvements
- Implement a tool permission manifest: declare what each tool can access
- Add a "sensitive operation" gate: agent must justify before write/delete calls
- Test with adversarial prompts: can external content redirect the agent?
- Separate audit log stream from application logs

---

## 3. Efficiency (Weight: 15%)

**Definition:** The agent accomplishes tasks using the minimum necessary resources —
tokens, tool calls, time, and compute — without sacrificing correctness.

### Scoring Criteria

| Score | What it looks like |
|-------|-------------------|
| 9–10 | Cached results reused; parallel where possible; no redundant calls; prompt is minimal and precise |
| 7–8 | No obviously wasteful patterns; some optimization opportunities remain |
| 5–6 | Functional but visibly wasteful (redundant reads, sequential when parallel is possible) |
| 3–4 | Multiple redundant tool calls; re-reads data already in context; unnecessarily large prompts |
| 0–2 | Extremely wasteful; same data fetched repeatedly; bloated context on every call |

### Key Criteria to Evaluate

**Token Efficiency**
- [ ] System prompt contains only information the agent actually needs
- [ ] Context is pruned as the conversation grows (not unbounded)
- [ ] Agent doesn't re-read files/data already in context
- [ ] Responses are appropriately concise for the task

**Tool Call Efficiency**
- [ ] No tool call made more than once for the same data
- [ ] Results cached within a session where valid
- [ ] Parallel tool calls used when calls are independent
- [ ] Agent doesn't search/list when it can address directly

**Workflow Efficiency**
- [ ] Subtasks that can be parallelized are parallelized
- [ ] Agent doesn't re-plan after every step (plan once, execute)
- [ ] No unnecessary confirmation loops for low-risk actions
- [ ] Sequential steps that could be batched are batched

**Model Selection**
- [ ] Right model for the task (not using Opus for classification)
- [ ] Small/fast models used for simple routing and triage tasks
- [ ] Large models reserved for complex reasoning tasks

### Anti-Patterns 🚩
- Fetching a file 3 times in one session because it was already "consumed"
- Searching the web for information that's already in the system prompt
- Running 5 steps sequentially when 4 of them are independent
- 10,000-token system prompt for a task requiring 500 tokens
- Using the most expensive model for every subtask regardless of complexity

### Improvements
- Add a context cache layer: track what data was fetched this session
- Profile token usage per task type; identify top consumers
- Map which tool calls can be parallelized in the task graph
- Implement model routing: classify task complexity, select model accordingly

---

## 4. Scalability (Weight: 12%)

**Definition:** The agent system maintains performance and correctness as load,
data volume, or number of concurrent users increases.

### Scoring Criteria

| Score | What it looks like |
|-------|-------------------|
| 9–10 | Horizontal scaling tested; no shared mutable state; bottlenecks identified and addressed |
| 7–8 | Works at current scale; known limits documented; scaling path exists |
| 5–6 | Works now; untested at higher load; likely bottlenecks exist |
| 3–4 | Will break at 5–10× current load; no scaling plan |
| 0–2 | Single-threaded; shared state; will fail at any meaningful scale |

### Key Criteria to Evaluate

**Statelessness**
- [ ] Agent instances don't share mutable state
- [ ] Session state is stored externally (DB, cache), not in memory
- [ ] Agent can be restarted without losing critical context

**Bottleneck Identification**
- [ ] The slowest step in the pipeline is known
- [ ] External API rate limits are documented and respected
- [ ] Context window limits are understood and planned for

**Concurrency**
- [ ] Multiple agent instances can run simultaneously without conflict
- [ ] Idempotent operations where concurrent retries are possible
- [ ] Locks/queues used where true mutual exclusion is required

**Data Volume**
- [ ] Agent handles growing data sets (uses pagination, streaming)
- [ ] Search/filter at scale (indexed, not full scan)
- [ ] Context window doesn't grow unboundedly with task size

### Anti-Patterns 🚩
- All agent state stored in a single Python dict in memory
- No rate limiting awareness — will hit API limits at scale
- Database queries that scan all rows for every agent call
- Agent prompt that grows by 1,000 tokens per subtask, indefinitely
- One agent instance handling all users (serialized queue)

### Improvements
- Externalize session state to Redis or a database
- Implement backpressure: queue tasks when downstream is overloaded
- Test at 10× current load in staging before production scale
- Replace sequential loops with parallel fan-out for independent subtasks

---

## 5. Maintainability (Weight: 12%)

**Definition:** The agent system can be understood, debugged, modified, and
extended by someone who didn't build it — including future you.

### Scoring Criteria

| Score | What it looks like |
|-------|-------------------|
| 9–10 | Modular design; documented prompts with rationale; version-controlled; change tested in isolation |
| 7–8 | Readable structure; most decisions documented; changes are low-risk |
| 5–6 | Works but lacks documentation; changes require full re-testing |
| 3–4 | Monolithic prompt; no documentation; fragile — small changes break things |
| 0–2 | Impossible to understand or change safely; no documentation; no tests |

### Key Criteria to Evaluate

**Prompt Management**
- [ ] Prompts are stored as versioned files, not hardcoded in code
- [ ] Each prompt section has a comment explaining its purpose
- [ ] Prompt changes are tracked in version control with reasoning
- [ ] Prompts are modular (system/user/few-shot separated)

**Code Structure**
- [ ] Agent logic is separated from tool implementations
- [ ] Configuration is separated from business logic
- [ ] Each agent component has a single responsibility
- [ ] Function/module names describe what they do

**Documentation**
- [ ] README explains what the agent does, how to run it, how to modify it
- [ ] Non-obvious decisions have inline comments with rationale
- [ ] Known limitations and edge cases are documented
- [ ] Runbook exists: what to do when the agent fails

**Testability**
- [ ] Agent behavior can be tested without hitting real external services (mocks)
- [ ] Regression tests exist for previously-broken behaviors
- [ ] Tests cover the prompts, not just the surrounding code

### Anti-Patterns 🚩
- 2,000-line system prompt with no comments or sections
- Tool behavior and agent logic interleaved in the same function
- "Magic" prompt text that works but nobody knows why
- No way to run the agent locally without production credentials
- Last modified by someone who left the team 6 months ago

### Improvements
- Break monolithic prompt into named, commented sections
- Write an "Architecture Decision Record" for key design choices
- Create a local test harness with mocked tool responses
- Add a CHANGELOG to track what was changed and why

---

## 6. Cost-effectiveness (Weight: 10%)

**Definition:** The resources consumed (tokens, API calls, compute, time) are
proportional to the value the agent delivers.

### Scoring Criteria

| Score | What it looks like |
|-------|-------------------|
| 9–10 | Cost per task measured; ROI tracked; budget alerts in place; continuous optimization |
| 7–8 | Costs known and reasonable; obvious waste eliminated |
| 5–6 | Cost unknown or unmeasured; likely inefficiencies not yet addressed |
| 3–4 | Measurably wasteful; expensive model for cheap tasks; no cost awareness |
| 0–2 | Runaway cost; no visibility; same expensive operation repeated constantly |

### Key Criteria to Evaluate

**Measurement**
- [ ] Cost per task type is measured and tracked
- [ ] Token usage is logged per run
- [ ] Budget alerts trigger before runaway costs occur

**Optimization**
- [ ] Most expensive operations identified and targeted
- [ ] Caching reduces repeat identical requests
- [ ] Model tier matches task complexity
- [ ] Batch API used for non-time-sensitive tasks

**ROI Awareness**
- [ ] Cost per task compared to value delivered (or human alternative cost)
- [ ] Low-value tasks that consume high resources identified
- [ ] Cost/quality tradeoff is a conscious decision

### Anti-Patterns 🚩
- Using flagship model for every call including "yes/no" classifications
- No caching — same expensive web search repeated every session
- Batch tasks submitted one-by-one instead of using batch endpoints
- No awareness of token usage; bills arrive as surprises
- Agent retrying expensive calls 10 times on failures that won't recover

### Improvements
- Implement cost tracking: log (model, input_tokens, output_tokens, cost) per call
- Set up budget alerts at 50% and 80% of monthly allocation
- Use cheaper models for routing, classification, and simple extraction
- Evaluate batch API for non-real-time workloads (often 50% cheaper)

---

## 7. User Satisfaction (Weight: 10%)

**Definition:** The agent genuinely solves the user's real problem — not just
the literal request — in a way that feels appropriate and trustworthy.

### Scoring Criteria

| Score | What it looks like |
|-------|-------------------|
| 9–10 | User needs validated; feedback loop exists; output format matches use case; trust maintained |
| 7–8 | Users get what they need most of the time; known UX gaps being addressed |
| 5–6 | Technically correct output that users find frustrating or hard to use |
| 3–4 | Output often requires significant post-processing; users lose trust |
| 0–2 | Agent output doesn't serve user needs; high abandonment; negative feedback |

### Key Criteria to Evaluate

**Output Quality**
- [ ] Output format matches how the user will consume it (code, prose, table, JSON)
- [ ] Response length is appropriate — not too terse, not verbose padding
- [ ] Agent explains its reasoning when output might be surprising
- [ ] Agent signals uncertainty rather than sounding confident when wrong

**Real Need vs. Literal Request**
- [ ] Agent asks clarifying questions when intent is ambiguous
- [ ] Agent catches when the literal request would produce unhelpful output
- [ ] Agent suggests better approaches when the user's stated approach has problems

**Trust and Transparency**
- [ ] Agent is honest about its limitations and knowledge boundaries
- [ ] Agent attributes sources or explains reasoning on consequential outputs
- [ ] Agent doesn't hallucinate facts to fill gaps
- [ ] Users understand what the agent did and why

**Feedback Loop**
- [ ] Mechanism exists to capture when agent output was wrong or unhelpful
- [ ] Failure cases are analyzed, not just counted
- [ ] Improvements are tested against real user scenarios

### Anti-Patterns 🚩
- JSON output when the user wanted a readable summary
- Agent confidently answers outside its knowledge boundary
- "I've completed the task" when the task silently produced empty results
- No way for users to report when the agent is wrong
- Optimizing for CSAT score rather than actual usefulness

### Improvements
- Test outputs with real users before optimizing for metrics
- Add "confidence indicator" to agent responses where uncertainty is high
- Implement a lightweight feedback mechanism (thumbs up/down + reason)
- Review failed tasks weekly to identify patterns

---

## 8. Innovation (Weight: 6%)

**Definition:** The agent leverages current capabilities effectively — using
modern tools, patterns, and approaches rather than over-engineering or under-utilizing.

### Scoring Criteria

| Score | What it looks like |
|-------|-------------------|
| 9–10 | Using best available tools for each task; exploring new patterns thoughtfully; not reinventing solved problems |
| 7–8 | Generally modern stack; aware of better alternatives; intentional about adoption |
| 5–6 | Functional but conservative; missing opportunities to simplify with newer tools |
| 3–4 | Manually implementing things that tools handle; ignoring relevant advances |
| 0–2 | Actively fighting the platform; outdated patterns causing real problems |

### Key Criteria to Evaluate

**Tool Utilization**
- [ ] Using native tool-use / function-calling instead of parsing text output
- [ ] Using structured outputs where schema is fixed
- [ ] Using built-in retrieval rather than manual chunking/embedding from scratch
- [ ] Using orchestration frameworks where they reduce complexity

**Pattern Currency**
- [ ] Agent design reflects current best practices (not 2022 patterns on 2025 models)
- [ ] Prompt patterns updated as model capabilities have evolved
- [ ] Not using chain-of-thought tricks that newer models do natively

**Avoid Reinventing**
- [ ] Using existing libraries for chunking, embedding, vector search
- [ ] Using provider-managed context caching rather than manual caching
- [ ] Using eval frameworks rather than hand-rolled testing scripts

**Thoughtful Adoption**
- [ ] New tools adopted for real capability gains, not novelty
- [ ] Trade-offs of new approaches are understood before adoption
- [ ] Team can explain why each technology was chosen

### Anti-Patterns 🚩
- Parsing unstructured LLM text output when tool-use is available
- Building custom vector DB when managed solutions fit the scale
- Prompt patterns copied from 2022 tutorials that are now handled by the model
- Never updating agent design despite significant model capability improvements
- Adopting every new framework without evaluating fit

### Improvements
- Quarterly review: what has changed in the model/tool ecosystem?
- Audit for manual implementations that could be replaced by built-in capabilities
- Run A/B tests when adopting new patterns — verify actual improvement
- Maintain a "why we chose X over Y" doc for key architectural decisions
