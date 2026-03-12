# Attestation Contract

## 1. Purpose

The Attestation Contract defines how observations and evidence are formally declared within the Boundary Protocol.

While transition events record that an execution attempt occurred, **attestations provide verifiable statements about observed system conditions**.

Attestations allow watchers or observers to publish structured claims about system state that can later be evaluated by verification engines.

---

## 2. Core Definition

An **Attestation** is a verifiable statement produced by an identity asserting that a particular observation about a system state is true.

Conceptually:

```
Attestation = verifiable claim about an observed state
```

Attestations connect **state observations** with **accountable observers**.

---

## 3. Contract Goals

The Attestation Contract ensures that observation claims are:

- attributable
- verifiable
- structured
- portable across implementations

The contract defines the structure of attestations but does not enforce how evidence is produced.

---

## 4. Minimal Attestation Structure

A minimal attestation contains the following fields:

```
attestation
├─ attestation_id
├─ subject_state
├─ observer
├─ observed_at
├─ claim
└─ evidence
```

---

## 5. Field Definitions

### attestation_id

Deterministic identifier derived from the attestation document.

```
attestation_id = hash(attestation_document)
```

This allows attestations to be independently verified and referenced.

---

### subject_state

Reference to the state object that the attestation describes.

```
subject_state: <state_id>
```

---

### observer

Identity responsible for producing the observation.

```
observer: <identity_id>
```

Observers typically correspond to watchers defined in the Watcher Model.

---

### observed_at

Timestamp indicating when the observation occurred.

Example:

```
observed_at: 2026-03-07T11:05:00Z
```

---

### claim

Statement describing the observation being asserted.

Example:

```
claim:
  "serviceA is running version v2"
```

Claims may be expressed as structured attributes or textual statements depending on implementation.

---

### evidence

Optional supporting material that justifies the claim.

Examples may include:

- system responses
- logs
- metrics
- monitoring snapshots

Evidence allows other systems to independently evaluate the attestation.

---

## 6. Attestation Example

Example attestation record:

```
attestation
  attestation_id: a19f0c...
  subject_state: state_b3
  observer: watcher_A
  observed_at: 2026-03-07T11:05:00Z
  claim: "serviceA running version v2"
  evidence: metrics_snapshot
```

---

## 7. Multiple Attestations

Multiple observers may attest to the same state.

Example:

```
watcher_A → attestation_1
watcher_B → attestation_2
watcher_C → attestation_3
```

Verification engines may evaluate these attestations individually or collectively.

This supports distributed verification models.

---

## 8. Attestation Immutability

Attestations are immutable artifacts.

Once published, they must not be modified.

Corrections or disputes must be expressed through additional attestations rather than edits.

This preserves a transparent record of observations.

---

## 9. Relationship to Verification

Attestations provide the evidence used by verification engines to evaluate system outcomes.

Conceptually:

```
Intent
   ↓
Transition Event
   ↓
State Observation
   ↓
Attestation
   ↓
Verification
```

Verification systems interpret attestations to determine whether intents were satisfied.

---

## 10. Summary

The Attestation Contract defines how observers publish verifiable claims about system state.

Key properties:

- observer-attributed
- evidence-backed
- immutable
- independently verifiable

Attestations provide the evidence layer necessary for reliable verification within the Boundary Protocol.

