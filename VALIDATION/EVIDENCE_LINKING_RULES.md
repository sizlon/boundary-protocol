# Evidence Linking Rules

## 1. Purpose

The Evidence Linking Rules define how evaluation artifacts (such as verdicts) must reference the observations and attestations used during verification.

These rules ensure that every evaluation produced within the Boundary Protocol can be traced back to the evidence that supported it.

This enables **verification transparency**, **auditability**, and **independent re-evaluation**.

---

## 2. Evidence Traceability Principle

Every evaluation artifact must provide traceability to the evidence used in the evaluation process.

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
Verdict
```

Each step must reference the artifacts that provide supporting evidence.

---

## 3. Evidence Reference Requirement

Evaluation artifacts must explicitly reference the artifacts used as evidence.

For example:

```
verdict.evidence_refs → attestation_ids
```

These references allow independent systems to inspect the evidence chain.

---

## 4. Attestation–Observation Link

Attestations must reference the state observation they assert.

Example:

```
attestation.state_ref → state_id
```

This ensures that attestations remain tied to specific observations.

---

## 5. Observation–Subject Link

State observations must reference the subject being observed.

Example:

```
state.subject_ref → resource_identifier
```

The subject may represent:

- system resources
- documents
- processes
- software systems

---

## 6. Evaluation Reconstruction

By following the evidence references, systems must be able to reconstruct the evaluation path.

Example:

```
verdict
  → attestations
      → states
          → transition_event
              → intent
```

This allows auditors or automated systems to review how a conclusion was reached.

---

## 7. Evidence Independence

Evidence artifacts may originate from independent observers.

The protocol does not require that all evidence be produced by a single system.

Multiple attestations may exist for the same observation.

Example:

```
attestation_A
attestation_B
attestation_C
```

A verifier may choose which attestations to include when producing a verdict.

---

## 8. Evidence Completeness

A verdict should include sufficient evidence references for an independent verifier to reproduce the evaluation.

This rule supports **reproducible verification**.

However, the protocol does not mandate a specific evidence quantity or verification method.

---

## 9. Evidence Transparency

Evidence references should remain visible to systems evaluating protocol artifacts.

If evidence artifacts are restricted or unavailable, the verification result may not be independently reproducible.

Systems consuming protocol artifacts may treat missing evidence as a verification limitation.

---

## 10. Summary

The Evidence Linking Rules ensure that evaluations produced within the Boundary Protocol remain traceable to their supporting evidence.

Key properties:

- explicit evidence references
- reproducible evaluation paths
- support for independent observation
- distributed verification compatibility

These rules allow independent systems to analyze, audit, and reproduce verification outcomes across system boundaries.

