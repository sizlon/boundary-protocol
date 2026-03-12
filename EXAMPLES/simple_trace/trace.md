# Simple Trace Example

This example illustrates the minimal lifecycle of a Boundary Protocol trace.

The purpose of this example is to demonstrate how accountability emerges from a sequence of events and artifacts.

The lifecycle shown here is:

Intent → Transition → State → Attestation → Verdict

The trace is defined through **event causal relationships** and **artifact references**.

Artifact identifiers in this example use a human-readable sample namespace for clarity.

---

# Trace Overview

The trace represents a simple configuration change scenario.

An operator declares an intent to enable a feature flag in a configuration service.  
The system attempts the transition, the resulting state is observed, the observation is attested, and finally a verdict is evaluated.

Artifacts produced during the lifecycle:

- intent:sample:001
- transition:sample:001
- state:sample:001
- attestation:sample:001
- verdict:sample:001

---

# Event Chain

The event chain captures the causal ordering of the trace.

event-001 — Intent declared  
↓  
event-002 — Transition attempted  
↓  
event-003 — State observed  
↓  
event-004 — Attestation recorded  
↓  
event-005 — Verdict evaluated  

Each event references the artifact it introduces and may reference previous events to establish causal ordering.

---

# Artifact Flow

The artifacts form the accountability chain.

intent:sample:001  
↓  
transition:sample:001  
↓  
state:sample:001  
↓  
attestation:sample:001  
↓  
verdict:sample:001  

Each artifact represents a protocol object defined by the Boundary Protocol models.

---

# Events

The `events/` directory contains the chronological event log of the trace.

Each event contains:

- event identifier  
- event type  
- artifact reference  
- causal references  

Example event structure:

event_id: event-002  
event_type: transition  
artifact_ref: transition:sample:001  
causal_refs:
- event-001

Causal references define how events connect to form the trace graph.

---

# Artifacts

The `artifacts/` directory contains the durable protocol artifacts referenced by events.

Artifacts represent protocol objects such as:

- Intent declarations  
- Transition attempts  
- State observations  
- Attestations  
- Verdicts  

Artifact identifiers in this example use a **sample namespace**:

intent:sample:001  
state:sample:001  
verdict:sample:001  

In production implementations, artifact identifiers may be deterministic or content-addressed.

---

# Minimal Accountability Chain

This example demonstrates the minimal chain required to produce a verifiable verdict.

Intent declared  
↓  
Transition attempted  
↓  
State observed  
↓  
Attestation recorded  
↓  
Verdict evaluated  

The chain allows independent verification of the final verdict by tracing back through the underlying events and artifacts.

---

# Purpose of the Example

This example exists to illustrate:

- the minimal Boundary Protocol lifecycle  
- the relationship between events and artifacts  
- the structure of a simple trace  

More complex traces may include branching, merging, or dispute resolution.

See other examples for advanced scenarios.