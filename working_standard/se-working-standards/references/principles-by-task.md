# Principles by Task Type

Quick reference: which principles matter most for each type of work.
Focus on the top 3 for each task; apply the rest where relevant.

---

## Writing Code (features, scripts, functions)

| Priority | Principle | Why it matters here |
|----------|-----------|-------------------|
| 🥇 | **Reliability** | Code must handle failures; unhappy paths are real |
| 🥈 | **Security** | Code is the attack surface; mistakes here are persistent |
| 🥉 | **Maintainability** | Code will be read and changed many more times than written |
| + | Efficiency | Avoid O(n²) where O(n) exists; don't repeat expensive operations |
| + | Scalability | Don't hardcode limits that will require rewrites later |

**Key questions before submitting code:**
- Does every function that can fail handle failure explicitly?
- Is any secret or credential visible in the code or logs?
- Would someone new to the project understand this code in 5 minutes?
- Are there any operations that will become slow as data grows?

---

## Designing a System or Architecture

| Priority | Principle | Why it matters here |
|----------|-----------|-------------------|
| 🥇 | **Scalability** | Architecture decisions are expensive to change later |
| 🥈 | **Reliability** | System design determines failure modes |
| 🥉 | **Maintainability** | Complex architectures become unmaintainable quickly |
| + | Security | Security gaps in architecture are the hardest to fix retroactively |
| + | Cost-effectiveness | Architecture determines the long-term cost envelope |
| + | Innovation | Modern patterns may eliminate components entirely |

**Key questions before finalizing a design:**
- What breaks at 10× current load, and is that acceptable?
- Where are the single points of failure, and what's the mitigation?
- How does a new engineer understand and extend this system?
- What does this cost to run, and how does cost scale with usage?

---

## Building or Modifying a Data Pipeline

| Priority | Principle | Why it matters here |
|----------|-----------|-------------------|
| 🥇 | **Reliability** | Silent data corruption is worse than a visible crash |
| 🥈 | **Efficiency** | Pipelines process large volumes; waste compounds |
| 🥉 | **Scalability** | Today's pipeline will process tomorrow's larger dataset |
| + | Security | Data pipelines often touch sensitive data |
| + | Maintainability | Pipelines run unattended; operators must be able to debug them |

**Key questions before completing a pipeline:**
- What happens if a step produces malformed or partial output?
- Is idempotency guaranteed? Can this be re-run safely if it fails mid-way?
- Are there unbounded operations that will time out at larger data volumes?
- Is there sufficient logging to diagnose failures without re-running?

---

## Calling External APIs or Services

| Priority | Principle | Why it matters here |
|----------|-----------|-------------------|
| 🥇 | **Reliability** | External services fail; your code must handle it |
| 🥈 | **Security** | API calls transmit credentials and may expose data |
| 🥉 | **Efficiency** | External calls are expensive — avoid repeating them |
| + | Cost-effectiveness | Many APIs charge per call; batch where possible |

**Key questions before writing API integration code:**
- What happens when the API returns 429, 500, or times out?
- Is retry logic present with backoff — and a maximum retry limit?
- Are credentials loaded from environment, not hardcoded?
- Can multiple calls be batched into one where the API supports it?
- Is the response cached so the same call isn't repeated unnecessarily?

---

## Writing a Technical Plan or Specification

| Priority | Principle | Why it matters here |
|----------|-----------|-------------------|
| 🥇 | **User Satisfaction** | A plan that doesn't address the real need is wasted effort |
| 🥈 | **Reliability** | Plans must account for what happens when things go wrong |
| 🥉 | **Maintainability** | Plans will be read, updated, and referenced by others |
| + | Scalability | Plans should consider growth, not just current state |
| + | Innovation | Plans shouldn't default to the familiar when better approaches exist |

**Key questions before delivering a plan:**
- Does this plan solve what the person actually needs, not just what they asked?
- Does it address failure scenarios, not just the happy path?
- Is it clear enough that someone else could execute it?
- Are assumptions stated explicitly?

---

## Setting Up Infrastructure or Configuration

| Priority | Principle | Why it matters here |
|----------|-----------|-------------------|
| 🥇 | **Security** | Misconfigured infrastructure is a common attack vector |
| 🥈 | **Reliability** | Infrastructure must stay up; configuration errors can take down systems |
| 🥉 | **Maintainability** | Config must be understandable and safe to modify |
| + | Cost-effectiveness | Infrastructure is billed continuously; right-sizing matters |
| + | Scalability | Infrastructure should scale without manual intervention |

**Key questions before completing infrastructure setup:**
- Are all secrets in a secrets manager, not in config files?
- Does the configuration specify what each setting does and what's safe to change?
- Is there a rollback path if this configuration causes problems?
- Is the infrastructure right-sized for current needs, with a growth path?

---

## Debugging or Investigating a Problem

| Priority | Principle | Why it matters here |
|----------|-----------|-------------------|
| 🥇 | **Reliability** | Bugs often reveal reliability gaps — fix the pattern, not just the instance |
| 🥈 | **Maintainability** | The fix should be understandable; the diagnosis should be documented |
| 🥉 | **Efficiency** | Don't reproduce a bug expensively when a cheaper reproduction path exists |

**Key questions while debugging:**
- Is this a one-off bug, or a symptom of a systemic gap (missing error handling, etc.)?
- Once the fix is found, is it the right fix — or does it paper over a deeper issue?
- Should this bug trigger a new test to prevent regression?
- Is the root cause documented so others don't debug it again?

---

## Reviewing or Refactoring Existing Code

| Priority | Principle | Why it matters here |
|----------|-----------|-------------------|
| 🥇 | **Maintainability** | Refactoring exists to improve understandability and changeability |
| 🥈 | **Reliability** | Refactoring is a common source of regressions |
| 🥉 | **Efficiency** | Refactoring is an opportunity to fix inefficient patterns |

**Key questions during refactoring:**
- Do existing tests cover the behavior being refactored?
- Is the refactored code demonstrably clearer or more reliable?
- Are any changes introducing new dependencies or assumptions?
- Is the scope of the refactor limited to what was asked?
