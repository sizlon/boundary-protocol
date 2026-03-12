# EXAMPLES

This directory contains illustrative examples of **Boundary Protocol traces**.

The purpose of these examples is to help readers and implementers understand how the protocol primitives work together in practice.

Each example demonstrates a different kind of accountability trace.

---

# Example Categories

## 1. Simple Trace

A minimal trace demonstrating the basic lifecycle of a protocol interaction.

Typical flow:

```
intent declared
   ↓
transition attempted
   ↓
state observed
   ↓
attestation published
   ↓
verdict issued
```

This example focuses on the **core protocol primitives** and shows the smallest meaningful trace.

---

## 2. AI Agent Trace

An example showing how an AI agent interaction may be represented using the protocol.

Possible flow:

```
intent: "retrieve financial report"
   ↓
agent invokes tool
   ↓
external API interaction
   ↓
state observation
   ↓
attestation
   ↓
verdict
```

This scenario demonstrates how Boundary Protocol can capture accountability in AI-driven workflows.

---

## 3. Dispute Trace

An example where different observers disagree about an evaluation.

Possible flow:

```
verdict issued
   ↓
alternate evaluation
   ↓
dispute raised
   ↓
dispute resolution
```

This example highlights how the protocol supports **structured disagreement and review**.

---

# Trace Representation

Each example directory may include:

- `events/` — event log entries
- `artifacts/` — protocol artifacts (intent, state, attestations, etc.)
- `trace.md` — a narrative explanation of the trace

Example layout:

```
EXAMPLES/
  simple_trace/
    events/
    artifacts/
    trace.md
```

---

# Purpose of Examples

These examples serve several roles:

- help readers understand protocol semantics
- provide reference traces for implementers
- support experimentation with SDKs and verification engines

As the protocol evolves, additional examples may be added to illustrate more complex scenarios.

