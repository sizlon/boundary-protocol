# NON GOALS

This document defines what Boundary Protocol is **not intended to do**.

Establishing explicit non-goals is essential to prevent scope creep and to preserve the clarity of the protocol's purpose.

Boundary Protocol is an **accountability protocol**, not a universal system control framework.

---

# 1. Not a Control System

Boundary Protocol does not attempt to control or enforce system behavior.

It does not define:

- execution policies
- permission enforcement mechanisms
- runtime orchestration

Systems remain responsible for their own behavior. The protocol only records what happened and how it was evaluated.

---

# 2. Not a Policy Engine

Boundary Protocol does not evaluate policy rules directly.

Policy evaluation may be performed by external systems, but the protocol itself only records:

- the action
- the observation
- the evaluation artifact

The protocol records **results of evaluation**, not policy logic.

---

# 3. Not a System Monitoring Platform

Boundary Protocol is not intended to replace monitoring or observability systems.

It is not designed to collect:

- metrics
- telemetry
- system performance data

Monitoring tools may provide inputs to the protocol, but they are not part of the protocol itself.

---

# 4. Not an AI Governance Framework

Boundary Protocol does not attempt to define ethical rules or governance frameworks for artificial intelligence.

The protocol does not decide:

- what AI systems should be allowed to do
- which decisions are ethically acceptable

Instead, it provides a structure for recording **who made decisions and how they were evaluated**.

---

# 5. Not a Security Enforcement Layer

Boundary Protocol does not enforce security boundaries.

Security controls such as:

- authentication
- authorization
- sandboxing

must be implemented by external systems.

The protocol may record events related to those systems, but it does not implement them.

---

# 6. Not a Single Implementation

Boundary Protocol is not a specific software product.

It is a specification that multiple independent implementations may adopt.

Possible implementations may include:

- SDKs
- trace collectors
- verification engines
- auditing systems

But none of these implementations define the protocol itself.

---

# 7. Not a Replacement for Human Judgment

The protocol does not eliminate the need for human interpretation.

Even when automated verification systems produce verdicts, interpretation of those results remains a human responsibility.

Boundary Protocol records accountability structures, but it does not replace human oversight.

---

# Summary

Boundary Protocol deliberately limits its scope to the problem of **accountability tracing**.

It does not attempt to control systems, enforce policy, or replace governance structures.

By remaining narrowly focused on traceability and verifiable history, the protocol can remain stable, composable, and broadly applicable across different domains.
