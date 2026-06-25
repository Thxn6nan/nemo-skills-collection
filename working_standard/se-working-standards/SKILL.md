---
name: se-working-standards
description: >
  Apply software engineering principles as working standards when executing any
  project task. This skill guides HOW work gets done on a project — not whether
  the work is in scope (project-qa) or whether the output has defects (work-audit).
  Use this skill at the start of any non-trivial project task to establish working
  standards, and refer to it throughout execution to maintain quality decisions.
  Trigger phrases: "build this properly", "engineer this well", "follow SE standards",
  "apply best practices", "do this the right way", "production-quality", "set up a
  project", "start a new feature", "implement this", or any task where the work
  will produce code, designs, documents, or systems. These 8 principles are not a
  scoring rubric — they are working standards for doing good work on any project.
---

# Working Standards Skill

Eight principles that guide how to do project work well.
Read the principles relevant to the task at hand. Apply them as you work.

## Core Principle

> Good work is not about doing more.
> It is about making the right decisions at each step so the result
> is reliable, secure, maintainable — and actually serves the project's goal.

---

## The 8 Working Principles

### 1. Reliability — Build things that work under real conditions

**What this means when working:**
Every deliverable should handle failure. Assume inputs will be unexpected.
Assume dependencies will break. The happy path is not the whole job.

**Apply when:**
- Writing code that calls external services → add error handling + retry logic
- Processing inputs → validate and handle edge cases explicitly
- Building multi-step workflows → define what happens if a step fails
- Creating data pipelines → ensure consistency even when something goes wrong

**Working standards:**
- Every failure path is handled explicitly — no silent failures
- Functions return meaningful errors, not just null or false
- State that must survive interruption is persisted, not kept in memory only
- Timeouts exist on every external call

**Ask yourself while working:**
"What happens when this fails? Is that acceptable?"
If the answer is "it crashes" or "nothing happens" → fix before moving on.

---

### 2. Security & Trust — Handle data and access responsibly

**What this means when working:**
Every piece of code, config, or design that touches sensitive data or access
controls must be written defensively. Security is a constraint respected from
the first decision, not a feature added at the end.

**Apply when:**
- Handling credentials or tokens → never hardcode, always environment/vault
- Accepting user input → validate and sanitize before use
- Designing access → grant minimum privilege needed, no more
- Writing logs → exclude PII and secrets
- Making decisions with user data → confirm authorization first

**Working standards:**
- Secrets live in environment variables or a secrets manager, never in code
- User-provided input is never trusted without validation
- Logs capture what happened, not the sensitive data involved
- Every operation uses the minimum permission scope required

**Ask yourself while working:**
"If this were exposed or leaked, what would be compromised?"
If the answer is credentials or user data → restructure before proceeding.

---

### 3. Efficiency — Use the minimum resources to do the job correctly

**What this means when working:**
Do not repeat work already done. Do not run sequentially what can run in
parallel. Do not use a complex solution where a simple one works.
Efficiency is not premature optimization — it is not being wasteful.

**Apply when:**
- Fetching data that was already fetched → cache or reuse the result
- Running independent subtasks → parallelize where possible
- Processing large datasets → use streaming or batching
- Choosing an approach → pick one appropriate for the actual scale

**Working standards:**
- Data fetched once per session unless it changes
- Independent operations run in parallel where the environment supports it
- Algorithm choice considers the actual scale of the problem
- The solution is as simple as it can be while still being correct

**Ask yourself while working:**
"Have I already done this? Could multiple things happen at once here?"
If yes → refactor before adding more steps.

---

### 4. Scalability — Design for growth, not just for today

**What this means when working:**
Write and design things that still work when load is 10× higher, datasets
are 10× larger, or the number of users doubles. Don't over-engineer — but
don't create hard limits that require a full rewrite when things grow.

**Apply when:**
- Storing data → use a structure that works at 10× current volume
- Designing interfaces → pagination from the start, not added later
- Writing queries → ensure they work at scale, not just for today's rows
- Handling concurrent operations → avoid shared mutable state
- Setting limits → make them configurable, not hardcoded

**Working standards:**
- Pagination on every list endpoint from the start
- No assumption that a dataset, queue, or file will stay small
- Limits and thresholds are constants or config, not magic numbers
- Queries are appropriate for the access pattern at realistic volume

**Ask yourself while working:**
"What breaks at 10× current load? Is that acceptable to fix later?"

---

### 5. Maintainability — Write for the person who reads this next

**What this means when working:**
Code, configs, and documents should be understandable by someone who
wasn't there when they were created. That person is often yourself, six
months later. Maintainability is about making changes safe and intent clear.

**Apply when:**
- Naming things → names should reveal intent, not just describe shape
- Writing complex logic → add a comment explaining why, not just what
- Making decisions → leave a note explaining the choice
- Structuring work → each component does one thing well
- Finishing a task → leave it in a state the next person can understand

**Working standards:**
- Names reveal intent: `fetch_user_orders()` not `get_data()`
- Non-obvious decisions have a comment explaining the reasoning
- No magic numbers — use named constants with descriptive names
- Each function or module has a single clear responsibility
- The work is documented enough that someone else could continue it

**Ask yourself while working:**
"If I came back to this in 6 months with no context, would I understand it?"
If no → add clarity before moving on.

---

### 6. Cost-effectiveness — Spend resources proportional to value delivered

**What this means when working:**
Not every task needs the most powerful tool or the most complex approach.
Use what's appropriate for the job. Know what things cost and make
conscious trade-offs rather than defaulting to the most expensive option.

**Apply when:**
- Choosing tools or services → match capability to the actual need
- Designing workflows → batch non-urgent work, avoid one-at-a-time processing
- Caching → avoid re-running expensive operations for the same inputs
- Storing data → don't keep what you don't need longer than needed
- Estimating → know the cost of the approach before committing

**Working standards:**
- Tool and service selection matches task complexity
- Expensive operations are cached where inputs repeat
- Data is retained only as long as it serves a purpose
- Cost of the chosen approach is understood before implementation begins
- Trade-offs between cost and quality are made explicitly

**Ask yourself while working:**
"Is this the right tool for this specific step, or am I using it out of habit?"

---

### 7. User Satisfaction — Solve the real problem, not just the stated one

**What this means when working:**
The literal request is not always the actual need. Good work solves what
the person actually needs, in a form they can use, with honesty about
what the result can and cannot do.

**Apply when:**
- Interpreting a request → check if the literal interpretation is the real need
- Choosing output format → match the format to how it will be consumed
- Handling uncertainty → flag it clearly rather than hiding it
- Delivering results → include what the person needs to act on it
- Hitting a limitation → surface it and suggest what else can be done

**Working standards:**
- Output format matches use case: don't produce JSON when prose is needed
- Uncertainty is flagged: "This relies on X being true — verify before use"
- When a better approach exists: say so rather than doing the literal thing
- Incomplete results are labeled as incomplete, not presented as final
- The person receiving the output can act on it without additional guesswork

**Ask yourself while working:**
"Will the person receiving this be able to use it directly?
Is there anything they need to know that I haven't included?"

---

### 8. Innovation — Use the best available approach, not the first one that comes to mind

**What this means when working:**
The first solution you think of is not always the best one. Existing tools,
patterns, and established approaches often provide simpler, more reliable
solutions than building from scratch. Stay current, but adopt thoughtfully.

**Apply when:**
- Building something manually → check if a library or built-in does this
- Using a familiar pattern → check if a better alternative exists now
- Solving a recurring problem → look for established solutions first
- Designing a workflow → consider if a different approach eliminates steps

**Working standards:**
- Check for existing solutions before implementing from scratch
- Evaluate trade-offs before adopting a new approach: capability, complexity, cost
- Don't use a complex solution when a simple one does the job
- Don't use a legacy pattern when a modern equivalent is cleaner and well-supported
- When choosing between approaches, document why this one was selected

**Ask yourself while working:**
"Is there a better way I haven't considered?
Am I solving a problem that's already been solved?"

---

## How to Apply These Principles in Practice

### At the Start of a Task

Identify which 2–3 principles are most critical for this specific task:

```
TASK TYPE                  → PRIORITY PRINCIPLES
──────────────────────────────────────────────────────────────
Writing code               → Reliability, Security, Maintainability
Designing a system         → Scalability, Reliability, Cost-effectiveness
Processing or moving data  → Reliability, Efficiency, Security
Writing a plan or document → User Satisfaction, Maintainability
Building an integration    → Reliability, Security, Efficiency, Cost
Delivering a feature       → All 8, weighted by context
```

### During the Task

At each significant decision point, apply the relevant principle's question.
Do not apply all 8 to every decision — focus on the 2–3 that matter most here.

### Before Delivering

Run the working checklist in `references/working-checklist.md` for the task type.

---

## Reference Files

- `references/working-checklist.md` — Pre-delivery checklist by task type
- `references/principles-by-task.md` — Which principles apply to which work
