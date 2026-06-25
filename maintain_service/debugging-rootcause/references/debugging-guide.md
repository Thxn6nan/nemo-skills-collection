# Root Cause Methods & Debugging by Symptom

---

## Part 1: Root Cause Methods

### Method 1: 5 Whys
Best for: causal chains, process failures, logic errors
```
Start with the symptom. Ask "why did this happen?" 5 times.
Stop when you reach something that could have been prevented by design.

Rules:
  - Each "why" must be answered with evidence, not assumption
  - If you reach "human error" → ask one more why (why was error possible?)
  - Multiple branches are valid — draw a tree, not just a chain
  - Stop at the level where a fix is actually possible
```

### Method 2: Bisection (Binary Search)
Best for: regressions — something worked before and broke at some point
```
1. Identify a known-good state (commit, time, version)
2. Identify the known-bad state
3. Test the midpoint
4. Narrow the range by half based on result
5. Repeat until the exact change is identified

Tools: git bisect (for code), log timestamps (for operational issues)
```

### Method 3: Component Isolation
Best for: distributed systems, integration bugs, "works in isolation" bugs
```
1. Map all components involved in the failing path
2. Replace each with a known-good stub or mock, one at a time
3. When the bug disappears — the last component replaced is the culprit
4. When the bug persists with all components stubbed — it's in the test harness

Variation: "Divide and conquer"
  Split the system in half. Test each half independently.
  Repeat on the failing half.
```

### Method 4: Fault Tree Analysis
Best for: complex failures with multiple contributing factors
```
Start with the top-level failure (root of tree).
Branch downward: "what conditions would cause this?"
Each branch: AND gate (all must be true) or OR gate (any can be true)
Work down until you reach observable, testable leaf conditions.
Test each leaf. The ones that are TRUE are contributing causes.
```

### Method 5: Change Analysis
Best for: sudden regressions with no obvious cause
```
List everything that changed between "last known good" and "now":
  □ Code changes (commits, deployments)
  □ Configuration changes
  □ Data changes (schema, volume, content)
  □ Infrastructure changes (server, network, dependencies)
  □ External dependency changes (API updates, library versions)
  □ Time-based changes (certificates expiring, cron jobs, quotas resetting)

For each change: could this cause the observed symptom?
  YES → investigate first
  NO  → deprioritize but don't eliminate
```

---

## Part 2: Debugging by Symptom

### "Works locally, fails in production"

```
Common causes (check in order):
  1. Environment differences
     □ Different env variables or config values
     □ Different dependency versions (check lock files)
     □ Different OS or runtime version

  2. Data differences
     □ Production data has edge cases local data doesn't
     □ Production data volume is different (timeout, memory)
     □ Production data encoding or format differs

  3. Infrastructure differences
     □ Network latency (local: 0ms, prod: 50-200ms)
     □ File system permissions differ
     □ Connection pool limits hit in production

  4. Concurrency (local is single-user, production is multi-user)
     □ Race condition only visible under concurrent load
     □ Shared state that works alone, breaks concurrently
```

### Intermittent / Non-Reproducible Failures

```
Intermittent bugs are almost always one of:
  1. Race condition — two operations interleaving differently each time
  2. External dependency flakiness — third-party service failing occasionally
  3. Resource exhaustion — memory/connection/file handles running out over time
  4. Timing-dependent — something works if fast enough, fails if slow
  5. Data-dependent — only certain data combinations trigger it

Investigation approach:
  □ Increase logging around the suspected area
  □ Run under load to increase frequency
  □ Look for patterns: time of day? specific users? after uptime threshold?
  □ Add a counter: how often does X happen before the failure?
```

### Wrong Output (No Error)

```
The hardest class of bugs — the system thinks it succeeded.

Investigation approach:
  □ Trace the output backward: where is the output value set?
  □ Add assertions at each step: is the intermediate value correct?
  □ Check all code paths: is there a branch being taken unexpectedly?
  □ Check data transformations: is something being cast, rounded, or truncated?
  □ Check default values: is a default being used when it shouldn't be?
  □ Check caching: is stale data being returned?
```

### Performance Degradation (Getting Slower Over Time)

```
Common causes:
  1. Data growth → query that was fast at 1,000 rows is slow at 1,000,000
  2. Memory leak → each request leaves something behind; GC pressure grows
  3. Connection pool exhaustion → connections pile up, new ones wait
  4. Log growth → writing to a very large log file becomes slow
  5. Index fragmentation → database index needs rebuilding
  6. Cache pollution → cache fills with useless entries, hit rate drops

Investigation:
  □ Is degradation linear with time, or with data volume?
  □ Does a restart restore performance? (yes = memory/connection leak)
  □ Is a specific query or operation the bottleneck? (profile first)
```

### Timeout or Hanging

```
Investigation:
  □ Is it hanging forever or timing out after a specific duration?
     Specific duration → configured timeout is being hit
     Forever → likely deadlock or infinite loop

  □ Deadlock check: are two processes each waiting for what the other holds?
  □ Infinite loop: is there a while/retry loop with a missing exit condition?
  □ Blocked on external: is it waiting for a response that never comes?
     Add timeout to every external call. Always.

  □ Thread dump / stack trace of the hanging process: what is it waiting for?
```

### Data Corruption or Inconsistency

```
Investigation:
  □ Find the earliest point where the data is wrong
     (work backward from the observed inconsistency)
  □ Is the inconsistency in storage or in presentation?
     (check the raw data vs how it's displayed)
  □ Is there a race condition in writes?
     (two writes to the same record without locking)
  □ Is there a migration or transformation that ran on only some records?
  □ Is there a timezone or encoding issue in the data?

Prevention: add data integrity checks (constraints, checksums) at write time,
not just at read time.
```
