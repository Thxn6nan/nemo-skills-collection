---
name: system-design
description: >
  Design systems and architectures before building them — defining components,
  interfaces, data flows, trade-offs, and decisions. Use this skill when starting
  a significant new system, when an existing system needs architectural changes,
  or when making decisions about how components should connect and communicate.
  Trigger phrases: "design this system", "architecture for this", "how should
  we structure this?", "design this before we build it", "system architecture",
  "what's the right design for this?", "technical design document", "design
  review", "how do components connect?", "ADR", "architecture decision".
  Sits between project-planner (what to build) and working-standards (how to build).
---

# System Design Skill

Produces clear, reasoned architectural designs before implementation begins.
A well-designed system is easier to build, test, maintain, and scale.
A poorly-designed system becomes more expensive with every line of code added.

## Core Principle

> Design is the time when changes are cheapest.
> An hour of design prevents a week of refactoring.
> Every design decision is a trade-off — make trade-offs consciously,
> document them explicitly, and know what you're giving up.

---

## Design Workflow

### Phase 1: Understand the Requirements

```
FUNCTIONAL REQUIREMENTS (what the system must do):
  □ Core use cases: what are the top 5 things users/callers will do?
  □ Data: what data enters the system? What data leaves?
  □ Actors: who or what uses this system? (users, other services, scheduled jobs)
  □ Integrations: what external systems does this connect to?

NON-FUNCTIONAL REQUIREMENTS (how the system must perform):
  □ Scale: how many users/requests/records at launch? In 1 year? In 3 years?
  □ Latency: what response time is acceptable? (P50? P99?)
  □ Availability: what uptime is required? (99%? 99.9%? 99.99%?)
  □ Consistency: can the system show slightly stale data? Or must it be exact?
  □ Security: what data is sensitive? Who can access what?
  □ Cost: are there budget constraints that affect design choices?

CONSTRAINTS (what limits the design):
  □ Technology constraints (must use X because Y team uses it)
  □ Timeline constraints (must ship in Z weeks — limits complexity)
  □ Team skill constraints (team knows A but not B)
  □ Existing system constraints (must integrate with legacy system X)
```

### Phase 2: Define Components

```
IDENTIFY COMPONENTS:
  List the major logical components in the system.
  Each component should:
    □ Have a single clear responsibility
    □ Have defined inputs and outputs
    □ Be independently deployable or testable (ideally)
    □ Have a clear owner or ownership model

COMPONENT TEMPLATE:
  Name:          <component name>
  Responsibility: <what this component does — one sentence>
  Inputs:        <what data/requests it receives and from where>
  Outputs:       <what data/responses it produces and where they go>
  Dependencies:  <what it calls or depends on>
  State:         <what data it owns or persists>
```

### Phase 3: Design Interfaces

**Interfaces are the most expensive things to change later. Design them carefully.**

```
FOR EACH INTERFACE BETWEEN COMPONENTS:
  □ Protocol: REST? GraphQL? gRPC? message queue? event?
  □ Data format: JSON schema? Protobuf? Event schema?
  □ Authentication: how does the caller authenticate?
  □ Error handling: what errors can occur? What do callers do with them?
  □ Versioning: how will this interface evolve without breaking callers?

API DESIGN PRINCIPLES:
  □ Design from the consumer's perspective (what does the caller need?)
  □ Use consistent naming, formats, and error structures
  □ Make the common case easy; make the complex case possible
  □ Include only what is necessary — adding is easy, removing is breaking

INTERFACE DOCUMENTATION FORMAT:
  Endpoint/Topic: <name>
  Purpose:        <what it does>
  Request:        <shape with types>
  Response:       <shape with types>
  Errors:         <error codes and meanings>
  Example:        <request/response pair>
```

### Phase 4: Data Design

```
DATA OWNERSHIP:
  □ Which component owns which data?
    (only one component should be the source of truth for any given data)
  □ How does data flow between components?
  □ What data is duplicated, and why?

DATA SCHEMA (for each major entity):
  □ What fields does it have?
  □ What are the relationships?
  □ What are the constraints (unique, required, ranges)?
  □ How does it change over time? (versioning, audit trail)

STORAGE SELECTION:
  Relational DB (PostgreSQL, MySQL):
    Use when: structured data, complex relationships, ACID required
  Document DB (MongoDB, DynamoDB):
    Use when: flexible schema, hierarchical data, high write throughput
  Key-Value (Redis):
    Use when: caching, sessions, simple lookups, high speed required
  Message Queue (SQS, Kafka):
    Use when: async processing, event streaming, decoupling producers/consumers
  Object Storage (S3):
    Use when: files, blobs, large unstructured data
```

### Phase 5: Trade-off Analysis

**Every design choice is a trade-off. Document them before choosing.**

```
TRADE-OFF FRAMEWORK:
  For each significant decision, document:

  DECISION: <what is being decided>
  OPTIONS:
    A: <option description>
       Pros: <benefits>
       Cons: <costs/risks>
    B: <option description>
       Pros: <benefits>
       Cons: <costs/risks>
  CHOSEN: <A or B>
  BECAUSE: <reasoning — what factors made this the right choice here>
  TRADE-OFF ACCEPTED: <what we're giving up>
  REVISIT IF: <conditions that would make us reconsider>
```

**Common trade-offs to address explicitly:**
```
□ Consistency vs Availability (CAP theorem for distributed systems)
□ Simplicity vs Flexibility (simple now vs extensible for future)
□ Build vs Buy (custom vs third-party service)
□ Sync vs Async (immediate response vs eventual consistency)
□ Monolith vs Services (simple deployment vs independent scaling)
□ SQL vs NoSQL (structure vs flexibility)
```

### Phase 6: Design Review

Before finalizing, check:

```
CORRECTNESS:
  □ Do the components together fulfill all functional requirements?
  □ Can all use cases be traced through the design?
  □ Are all data flows accounted for?

RISK:
  □ What is the single point of failure? Is that acceptable?
  □ What happens when the most likely thing breaks?
  □ Run tenth-man: what has been assumed that might be wrong?

SIMPLICITY:
  □ Is there a simpler design that meets the same requirements?
  □ Has complexity been added before it was needed?
  □ Can a new team member understand this design in 30 minutes?

EVOLUTION:
  □ How does this design change when scale doubles?
  □ How does this design change when requirements change?
  □ What is easiest/hardest to change later?
```

---

## Design Document Format

```
SYSTEM DESIGN DOCUMENT
══════════════════════════════════════════════════
System:       <name>
Author:       <name>
Date:         <date>
Status:       [Draft | In Review | Approved]

1. CONTEXT
   <Why does this system need to exist? What problem does it solve?>

2. REQUIREMENTS
   Functional: <use cases and behaviors>
   Non-functional: <performance, availability, scale, security>
   Constraints: <technology, time, team>
   Out of scope: <explicitly excluded>

3. DESIGN OVERVIEW
   <High-level diagram description — components and how they connect>

4. COMPONENTS
   <One section per component using the template above>

5. DATA DESIGN
   <Key entities, storage choices, data flows>

6. INTERFACES
   <Key APIs and contracts between components>

7. KEY DECISIONS (Architecture Decision Records)
   <One ADR per significant trade-off using the trade-off format above>

8. RISKS AND OPEN QUESTIONS
   <What is uncertain? What could go wrong? What needs further investigation?>

9. ALTERNATIVES CONSIDERED
   <What other approaches were evaluated and why they were not chosen>
══════════════════════════════════════════════════
```
