---
name: security-audit
description: >
  Identify security vulnerabilities, threats, and gaps in a project, codebase,
  or system before they are exploited. Covers threat modeling, vulnerability
  assessment, security testing, and remediation prioritization. Use this skill
  when starting a project with security requirements, before releasing anything
  that handles user data or access, when a security concern is raised, or when
  conducting a periodic security review. Trigger phrases: "security review",
  "threat model this", "find security vulnerabilities", "is this secure?",
  "audit this for security issues", "what are the attack vectors?", "penetration
  test scope", "OWASP review", "security before launch", "assess the risk".
  working-standards covers security as a principle. This skill does the work.
---

# Security Audit Skill

Finds security vulnerabilities and threat vectors before attackers do.
Security gaps in production are not hypothetical — they are liabilities
waiting to be discovered by someone with worse intentions than you.

## Core Principle

> Security is not a checklist — it is a mindset applied to adversarial
> thinking. Ask not "what does this do?" but "what could someone make
> this do that we didn't intend?"

---

## Audit Scope Definition

Before auditing, define what is in scope:

```
SCOPE:
  □ What system, component, or codebase is being audited?
  □ What is the trust boundary? (what is trusted vs untrusted input?)
  □ Who are the users? (authenticated only? public? privileged roles?)
  □ What data does it handle? (PII? financial? credentials? public?)
  □ What are the consequences of a breach? (data leak? financial loss? downtime?)
```

---

## Phase 1: Threat Modeling

Identify what could go wrong before looking for how.

### STRIDE Framework

For each component in scope, evaluate all six threat types:

```
S — SPOOFING        Can an attacker pretend to be someone they're not?
                    → Authentication, identity verification, token forgery

T — TAMPERING       Can an attacker modify data or requests in transit or at rest?
                    → Input validation, integrity checks, encryption

R — REPUDIATION     Can an attacker deny having performed an action?
                    → Audit logging, non-repudiation mechanisms

I — INFORMATION     Can an attacker access data they shouldn't?
    DISCLOSURE      → Authorization, data exposure, error messages, logs

D — DENIAL OF       Can an attacker prevent legitimate use?
    SERVICE         → Rate limiting, resource exhaustion, input size limits

E — ELEVATION OF    Can an attacker gain more privileges than they should have?
    PRIVILEGE       → Authorization checks, privilege separation, role enforcement
```

### Attack Surface Map

```
For each entry point into the system, document:
  Entry point:   <API endpoint, form field, file upload, webhook, etc.>
  Input source:  [Public | Authenticated user | Admin | Internal service]
  Data handled:  <what data flows through this entry point>
  STRIDE risks:  <which STRIDE categories apply>
  Current controls: <what protection exists>
  Gap:           <what protection is missing>
```

---

## Phase 2: Vulnerability Assessment

Check for common vulnerability classes. See `references/vulnerability-checklist.md`.

**Critical categories (check all):**
```
AUTHENTICATION & SESSION
□ Passwords hashed with a modern algorithm (bcrypt, argon2 — not MD5/SHA1)
□ Session tokens are unpredictable and invalidated on logout
□ No credentials in URLs, logs, or error messages
□ Brute force protection on login endpoints

AUTHORIZATION
□ Every endpoint checks authorization (not just authentication)
□ Authorization checked server-side, not client-side only
□ Object-level authorization: can user A access user B's data?
□ Function-level authorization: can a regular user access admin functions?

INPUT HANDLING
□ All user input validated for type, length, format, and range
□ SQL queries use parameterized statements (no string concatenation)
□ No eval() or equivalent on user-controlled input
□ File uploads: type verified server-side, stored outside web root
□ HTML output encoded to prevent XSS

DATA PROTECTION
□ Sensitive data encrypted at rest (PII, financial, credentials)
□ Sensitive data encrypted in transit (HTTPS everywhere)
□ Secrets not in code, config files, or environment variables committed to VCS
□ Minimum data collected — no unnecessary PII stored
□ Data retention policy exists and is enforced

DEPENDENCIES
□ Third-party libraries are up to date (no known CVEs)
□ Dependency lockfile committed and reviewed
□ No abandoned or unmaintained dependencies for critical functions
```

---

## Phase 3: Finding Classification

Each finding is classified by severity and exploitability:

```
FINDING FORMAT:
  ID:              SEC-N
  Category:        [Auth | Authz | Input | Data | Config | Dependency | Logic]
  STRIDE:          [S | T | R | I | D | E]
  Severity:        [Critical | High | Medium | Low | Informational]
  Exploitability:  [Trivial | Easy | Moderate | Difficult]
  Description:     <what the vulnerability is>
  Impact:          <what an attacker could achieve>
  Evidence:        <where it was found / how to reproduce>
  Remediation:     <specific fix>
  References:      <OWASP, CVE, or CWE reference if applicable>
```

**Severity + Exploitability → Priority:**
```
              TRIVIAL     EASY        MODERATE    DIFFICULT
CRITICAL  →   P0 NOW      P0 NOW      P1 URGENT   P1 URGENT
HIGH      →   P0 NOW      P1 URGENT   P2 HIGH     P2 HIGH
MEDIUM    →   P1 URGENT   P2 HIGH     P3 MEDIUM   P4 LOW
LOW       →   P2 HIGH     P3 MEDIUM   P4 LOW      P4 LOW
```

---

## Phase 4: Remediation Plan

```
P0 — Fix immediately (block all other work)
  Findings that are both Critical/High severity AND trivially exploitable.
  Do not release anything until these are resolved.

P1 — Fix before next release (urgent)
  Must be in the next deployment.

P2 — Fix in next sprint (high)
  Scheduled work, not emergency. Track to completion.

P3/P4 — Backlog with review date
  Log, assign, set a review date. Don't let these slip indefinitely.
```

---

## Security Audit Report

```
╔══════════════════════════════════════════════════════════╗
║              SECURITY AUDIT REPORT                       ║
╚══════════════════════════════════════════════════════════╝
SCOPE:     <what was audited>
DATE:      <date>
AUDITOR:   <who conducted the audit>

FINDINGS SUMMARY:
  P0 (Fix now):     <N> findings
  P1 (Urgent):      <N> findings
  P2 (High):        <N> findings
  P3/P4 (Backlog):  <N> findings

CRITICAL FINDINGS:
  SEC-1 [Critical / Trivial] <title>
    Impact: <what an attacker can do>
    Fix:    <specific action>

ATTACK SURFACE GAPS:
  <entry points with no or insufficient controls>

RECOMMENDED ACTIONS (priority order):
  □ <action> — by <date>
  □ <action> — by <date>

OVERALL POSTURE: [Critical Risk | High Risk | Moderate | Acceptable]
╚══════════════════════════════════════════════════════════╝
```

---

## Reference Files

- `references/vulnerability-checklist.md` — Full checklist by vulnerability category
