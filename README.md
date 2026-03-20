# Boundary Protocol

Boundary Protocol is an **accountability protocol for autonomous systems**.

It defines how systems record verifiable traces of **intent, actions, observations, evaluations, and disputes** across system boundaries.

The protocol does **not control systems**. Instead, it provides a structured way to make system behavior **traceable, verifiable, and accountable**.

AI systems are the first major domain where such accountability infrastructure becomes necessary, but the protocol is designed to remain **general and system-agnostic**.


---

# What Boundary Protocol Is

Boundary Protocol is a **minimal protocol for recording and verifying system behavior across boundaries**.

It provides a structured model for:

- declaring **intent**
- recording **actions and transitions**
- observing **system state**
- publishing **attestations**
- issuing **verdicts**
- raising **disputes**

Together these artifacts form an **accountability trace** that can be independently verified.


---

# What Boundary Protocol Is Not

Boundary Protocol intentionally **does not define**:

- system control mechanisms
- orchestration frameworks
- workflow engines
- execution platforms

Execution platforms belong to systems built **on top of** the protocol.

The protocol exists only to make system behavior **auditable and verifiable**.


---

# Principles

Boundary Protocol is built around several core principles:

- **Accountability over control**  
  The protocol records what happened rather than attempting to govern system behavior.

- **Event-first design**  
  System history is expressed through chains of events.

- **Minimal state**  
  Persistent state is minimized; meaning is derived primarily from event history.

- **Verifiable artifacts**  
  Critical steps produce artifacts that can be independently verified.

- **System-agnostic architecture**  
  The protocol can apply to AI systems, software systems, or organizational processes.


---

# Core Model

The protocol defines a minimal semantic model:

- **Intent** — a declared purpose or objective  
- **Transition** — an attempted change or action  
- **State** — an observed condition of the system  

These primitives define the meaning of system behavior within the protocol.


---

# Responsibility Model

Boundary Protocol explicitly models responsibility:

- **Identity** — the actor responsible for actions  
- **Authority** — the scope of permitted actions  
- **Watcher** — an observer responsible for verification or monitoring  

This model allows the protocol to represent **who acted and who verified**.


---

# Verification Artifacts

The protocol defines standardized artifacts that record evaluation and judgment:

- **Transition Event** — a recorded transition attempt  
- **Attestation** — a statement about observed behavior  
- **Verdict** — an evaluation outcome  
- **Dispute** — a challenge to a verdict or attestation  

These artifacts enable traceable accountability chains.


---

# Validation Rules

To maintain the integrity of protocol history, several validation rules apply:

- **Causal Ordering** — events must follow valid causal order
- **Chain Integrity** — protocol history must remain tamper-evident
- **Evidence Linking** — artifacts must reference supporting evidence

These rules ensure that traces remain **verifiable and trustworthy**.


---

# Event Model

Runtime activity within the protocol is represented as events such as:

- intent declared
- transition attempted
- state observed
- attestation published
- verdict issued
- dispute raised

These events collectively form an **accountability trace**.


---

# Initial Domain: AI Systems

Autonomous AI systems are the first domain where boundary-level accountability becomes critical.

AI agents increasingly:

- make decisions
- perform actions
- interact with external systems

Boundary Protocol provides a structured mechanism to make those actions **observable and accountable**.


---

# Repository Structure

This repository contains the **specification of the protocol**, not implementations.

```
CONSTITUTION/             protocol constitution and non-goals
CORE/                     semantic primitives of the protocol
RESPONSIBILITY/           responsibility and authority model
CONTRACTS/                artifact contracts
VALIDATION/               integrity and validation rules
EXTENSIONS/GOVERNANCE/    optional generic governance extensions
SCHEMAS/                  machine-readable schema definitions
RATIONALE/                design explanations
EXAMPLES/                 example traces and protocol usage
META/                     versioning and protocol status
```

This repository is not a governance framework.
Only optional generic governance lifecycle extensions may remain here, under `EXTENSIONS/GOVERNANCE/`.
Profile-specific governance, engine meaning, and executor lifecycle enforcement belong in sibling implementation repositories.

Implementation projects (such as SDKs or executors) are maintained in **separate repositories**.
Executor runtime/interface documentation belongs in those implementation repositories, not here.


---

# Status

Boundary Protocol is currently under **active design**.

The goal of this repository is to establish a stable specification for an accountability protocol that can be adopted across multiple systems and domains.


---

# License

Boundary Protocol is released under the **Apache License 2.0**.
