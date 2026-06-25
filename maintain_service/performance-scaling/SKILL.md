---
name: performance-scaling
description: >
  Diagnose performance problems, optimize bottlenecks, and plan for scale.
  Covers profiling, load testing, capacity planning, and scaling decisions
  (vertical vs horizontal, caching, CDN, database read replicas, etc.).
  Use this skill when something is too slow, when preparing for growth, when
  load testing before a launch, or when deciding how to scale a system.
  Trigger phrases: "this is too slow", "optimize performance", "load test this",
  "prepare for 10x traffic", "capacity planning", "the system is struggling
  under load", "should we scale horizontally or vertically?", "where is the
  bottleneck?", "performance profiling", "latency is too high".
  DISAMBIGUATION — use a different skill instead when: the slowness is a
  sudden production incident affecting users now (use incident-response);
  the cause is a specific bug rather than capacity or load (use
  debugging-rootcause); the slowness is specifically database queries or
  schema (use database-maintenance). Choose performance-scaling for
  profiling, optimization, load testing, and capacity planning.
---

# Performance & Scaling Skill

Finds what is slow, fixes it, and plans for growth.
Optimization without measurement is guessing. Scale without planning is gambling.

## Core Principle

> Measure before you optimize. The bottleneck is never where you think it is.
> Fix the constraint that limits the whole system, not the part that is
> most familiar or most interesting to work on.

---

## The Performance Workflow

```
MEASURE → IDENTIFY BOTTLENECK → OPTIMIZE → VERIFY → REPEAT
```

Never skip MEASURE and jump to OPTIMIZE.
Never skip VERIFY and assume the optimization worked.

---

## Phase 1: Measure — Establish Baselines

Before any optimization work, measure:

```
LATENCY:
  □ P50 (median): typical user experience
  □ P95: experience for 95% of users
  □ P99: experience for worst 1% — often where problems hide
  □ Max: the worst case — useful for diagnosing outliers

THROUGHPUT:
  □ Requests per second at current load
  □ Maximum requests per second before degradation
  □ At what throughput does latency start to increase?

RESOURCE UTILIZATION:
  □ CPU: average and peak during load
  □ Memory: average, peak, growth rate over time
  □ Disk I/O: read/write throughput and latency
  □ Network: bandwidth utilization and connection count
  □ Database: query time, connection pool usage, slow queries

BASELINE FIRST:
  Measure under normal conditions before load testing.
  You cannot know if you've improved unless you know where you started.
```

---

## Phase 2: Identify the Bottleneck

Apply Theory of Constraints: one constraint limits the system more than all others.
Optimizing anything that is not the bottleneck does not improve the system.

### Finding the Bottleneck

```
STEP 1 — Profiling (find where time is spent):
  Backend:  profiler shows time spent per function/method
  Database: slow query log shows which queries consume the most time
  Frontend: browser DevTools Network/Performance tab
  Distributed: distributed tracing (Jaeger, Zipkin, Datadog APM)

STEP 2 — The 80/20 check:
  In most systems, 80% of latency comes from 20% of operations.
  Sort by: total time consumed = (avg time × call frequency)
  The top item is almost always the bottleneck.

STEP 3 — Resource saturation check:
  Which resource is at or near 100%?
    CPU at 100% → compute-bound bottleneck
    Memory at max → memory-bound (swapping to disk = severe)
    Disk I/O at max → I/O-bound (often database)
    Network at max → bandwidth-bound
    Database connections at max → connection pool bottleneck
```

### Common Bottleneck Patterns

```
DATABASE (most common bottleneck):
  Signals: slow queries, high database CPU, connection pool exhaustion
  Fixes: query optimization, indexes, caching, read replicas, connection pooling

N+1 QUERIES:
  Signals: many small queries per request in profiler
  Fix: batch queries, JOINs, eager loading

SYNCHRONOUS BLOCKING:
  Signals: threads/workers waiting on I/O (CPU utilization low but latency high)
  Fix: async I/O, message queues for non-time-critical work

CACHE MISS:
  Signals: high hit rate → fast; low hit rate → every request hits DB
  Fix: increase cache size, improve cache key design, pre-warm cache

SINGLE THREAD / PROCESS:
  Signals: one CPU core at 100%, others idle
  Fix: parallelize, use worker pools, async processing
```

---

## Phase 3: Optimization Strategies

Apply the right strategy for the bottleneck found:

### Caching
```
WHAT TO CACHE:
  □ Expensive computations with stable results
  □ Frequently accessed data that changes infrequently
  □ External API responses (with appropriate TTL)
  □ Rendered HTML or API responses (full-page or fragment caching)

CACHE DESIGN:
  □ Cache key: must uniquely identify the content
  □ TTL: how long before stale? (shorter = fresher, more cache misses)
  □ Invalidation: how is cache cleared when underlying data changes?
  □ Cache stampede prevention: if cache expires, only one request rebuilds it

CACHE HIERARCHY:
  L1: In-process memory (fastest, limited size, not shared)
  L2: Shared cache (Redis, Memcached) — shared across instances
  L3: CDN (for static assets and cacheable API responses)
```

### Async and Queue-Based Processing
```
USE WHEN:
  The response does not require the operation to be complete.
  Examples: sending email, generating reports, processing uploads,
            webhooks, notifications, data exports.

PATTERN:
  Request → acknowledge immediately → queue the work → process async
  User gets instant response; work happens in background.

TOOLS: Redis queues, RabbitMQ, SQS, Celery, Sidekiq, BullMQ
```

### Database Read Scaling
```
READ REPLICAS:
  Route read queries to replicas; write queries to primary.
  Replication lag: reads may return slightly stale data — acceptable?

CONNECTION POOLING:
  Database connections are expensive to create.
  Pool: create N connections once, reuse them.
  PgBouncer (Postgres), ProxySQL (MySQL), connection pool in ORMs.

QUERY RESULT CACHING:
  Cache the result of expensive queries in Redis.
  Invalidate when underlying data changes.
```

---

## Phase 4: Load Testing

Load testing before production reveals what monitoring will discover in crisis.

```
LOAD TEST TYPES:
  Load test:    Normal expected load — does it handle it?
  Stress test:  2-3× expected load — where does it break?
  Soak test:    Sustained load over time — memory leaks? connection exhaustion?
  Spike test:   Sudden surge then drop — recovery behavior?

WHAT TO MEASURE DURING LOAD TEST:
  □ At what throughput does latency start to degrade?
  □ What resource (CPU/memory/DB/network) saturates first?
  □ Does the system recover after load drops? (no lingering degradation)
  □ Are error rates acceptable at target load?

TOOLS: k6, Locust, Apache JMeter, Artillery, Gatling
```

---

## Phase 5: Scaling Decisions

```
VERTICAL SCALING (scale up):
  Bigger machine: more CPU, more RAM, faster disk.
  Pros: simple, no code changes, immediate
  Cons: expensive, has a ceiling, single point of failure
  Use when: cost is not the primary concern, complexity is

HORIZONTAL SCALING (scale out):
  More machines: distribute load across multiple instances.
  Pros: theoretically unlimited, better resilience
  Cons: requires stateless design, more complex, load balancing needed
  Use when: vertical ceiling reached, or resilience is required

DECISION FRAMEWORK:
  Is the service stateless? (can requests go to any instance?)
    NO → must make it stateless first before horizontal scaling
    YES → horizontal scaling is viable

  Is the bottleneck in the application or the database?
    Application → horizontal scaling helps
    Database    → horizontal scaling does NOT help; fix database first

  Is cost a concern?
    Vertical: cost grows linearly
    Horizontal: cost can be lower at scale, but more complex to manage
```

---

## Phase 6: Capacity Planning

```
INPUTS NEEDED:
  □ Current traffic volume and growth rate
  □ Current resource utilization at that volume
  □ Maximum safe utilization threshold (typically 70% for headroom)
  □ Lead time to add capacity

CALCULATION:
  Current headroom = (max safe util - current util) / current util × 100%
  Time until headroom is exhausted = headroom / monthly growth rate

PLANNING RULE:
  If time to exhaustion < lead time to add capacity → act now.
  Always maintain ≥ 30% headroom for traffic spikes.

TRIGGERS FOR CAPACITY REVIEW:
  □ Any resource crosses 60% sustained utilization
  □ P95 latency exceeds SLA threshold
  □ Load test shows limit within 2× of current traffic
```

---

## Reference Files

- `references/profiling-guide.md` — How to profile different stack types
