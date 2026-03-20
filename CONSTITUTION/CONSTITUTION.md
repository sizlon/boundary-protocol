# Boundary Protocol Constitution

## 1. Purpose of Boundary Protocol

Boundary Protocol is an accountability protocol for autonomous systems. Its purpose is to define the minimal structure required to record and verify intent, transitions, observable state, attestations, verdicts, and disputes across system boundaries.

Boundary Protocol solves the accountability problem. It provides a verifiable accountability trace that allows independent parties to reconstruct what was attempted, what was observed, who acted, who verified, and whether the result was disputed.

Boundary Protocol is not a control system. It does not define execution policies, permission enforcement mechanisms, runtime orchestration, execution platforms, workflow engines, or application logic.

Boundary Protocol is not a policy engine. It records evaluation artifacts and results of evaluation, but it does not evaluate policy rules directly.

Boundary Protocol is not a system monitoring platform. Monitoring systems may provide inputs, but metrics, telemetry, and performance monitoring are outside protocol scope.

Boundary Protocol is not a security enforcement layer. Authentication, authorization, sandboxing, and other security controls are external.

Boundary Protocol is not a single implementation. SDKs, trace collectors, verification engines, dispute systems, audit tooling, executors, and similar systems are separate from the protocol.

Boundary Protocol is not a replacement for human judgment. Automated artifacts may be produced within the protocol, but interpretation and oversight remain human responsibilities.

Boundary Protocol is not a governance framework. Governance frameworks may sit above the protocol and may consume protocol artifacts. This repository may contain optional generic governance lifecycle extensions, but those extensions are not protocol core.

## 2. Authority Model

Protocol meaning is defined by the specification repository content, not by implementations.

The constitutional sources of protocol meaning are:

- `CONSTITUTION/` for foundational positioning, principles, and non-goals
- `CORE/` for semantic primitives and event structure
- `RESPONSIBILITY/` for identity, authority, and watcher roles
- `CONTRACTS/` for artifact contracts
- `SCHEMAS/` for machine-readable structural constraints
- `VALIDATION/` for integrity, causal, evidence-linking, and binding validation rules
- `META/` for protocol state and version compatibility expectations
- `EXTENSIONS/GOVERNANCE/` only for optional generic lifecycle extensions, not for protocol-core meaning

Authority within protocol traces is explicit. The protocol distinguishes identity from authority. Identity answers who acted or observed. Authority expresses permission granted to an identity within a scope. Watchers are distinct observers responsible for verification or monitoring.

Authority is never implicit. Authority-bearing acts, adoption acts, and activation eligibility are distinct and must be represented explicitly where applicable extension documents apply.

The following are not authority:

- proposals
- procedural validation alone
- runtime inference
- mutable working copies
- external network lookups used to create meaning at runtime
- implementation convenience behavior
- undocumented local conventions

Where the repository provides optional lifecycle extensions in `EXTENSIONS/GOVERNANCE/`, those files are upstream-owned extension texts. They are not protocol core, they are not executor-local authority, and they do not erase the rule that governance frameworks sit above the protocol.

## 3. Ownership Boundary

`boundary-protocol` owns protocol meaning. This includes constitutional principles, core primitives, responsibility roles, artifact contracts, schemas, integrity and validation rules, version compatibility expectations, and optional generic governance lifecycle extensions maintained in this repository.

Executors and other implementations own execution. They may consume protocol meaning, validate protocol artifacts, emit protocol artifacts, and provide runtime interfaces.

Executors and other implementations must not redefine protocol meaning. They must not:

- change the semantics of intent, transition, state, attestation, verdict, or dispute
- create alternate authority sources
- reinterpret artifacts under mutable local rules
- turn procedural validation into adoption
- turn execution into constitutional authority
- infer authority that is not explicitly represented

Implementations may add runtime systems, storage systems, programming languages, SDKs, verifiers, or execution environments. Those additions do not define the protocol.

## 4. Structural Model of Meaning

Boundary Protocol separates meaning, structure, and enforcement.

### CORE

`CORE/` defines the semantic primitives of the protocol:

- Intent declares the desired outcome.
- Transition records an attempt to realize an intent.
- State records an observable condition of a subject.
- Events provide the append-only historical structure through which artifacts are created and linked.

Meaning begins here. Core primitives define what kinds of things exist in a Boundary Protocol trace and what questions they answer.

### CONTRACTS

`CONTRACTS/` defines the artifact contracts for transition events, attestations, verdicts, and disputes. Contracts state what each artifact is for and what minimum information it must carry.

Contracts define artifact meaning and role. They are semantic, not merely syntactic.

### SCHEMAS

`SCHEMAS/` provides machine-readable schema definitions for protocol artifacts. Schemas constrain admissible structure, field presence, and allowed values.

Schemas do not create protocol meaning on their own. They encode structural conformance for meanings defined elsewhere.

### VALIDATION

`VALIDATION/` defines the rules by which traces and governed validation artifacts are checked. This includes:

- causal ordering
- chain integrity
- evidence linking
- policy bundle canonicalization
- signature validation
- signer-chain validation
- temporal validity
- trust-anchor binding
- semantic version binding

Validation enforces conformance and integrity. It does not create new meaning at runtime. Validation is deterministic and fail-closed where the validation documents state such requirements.

### EXTENSIONS/GOVERNANCE

`EXTENSIONS/GOVERNANCE/` contains optional generic lifecycle extensions such as proposal, adoption, sealing, and activation sequencing for governed objects.

These files are upstream-owned extension texts. They are not required to interpret the minimal accountability protocol core.

### Interaction Rules

The structural interaction is:

1. `CORE/` defines primitives and event structure.
2. `RESPONSIBILITY/` defines who acts, observes, and holds scoped authority.
3. `CONTRACTS/` defines the role of each artifact.
4. `SCHEMAS/` encode admissible artifact structure.
5. `VALIDATION/` defines how conformance and integrity are checked.
6. `EXTENSIONS/GOVERNANCE/`, where applicable, governs optional proposal, adoption, sealing, activation eligibility, and similar lifecycle constraints for governed object classes.

Meaning lives in constitutional, core, responsibility, contract, validation, and applicable extension texts. Structure is encoded by schemas. Enforcement is performed by validators and implementations. Implementations consume meaning; they do not originate it.

## 5. Invariants (Non-Negotiable)

The following invariants are established by the existing repository and are non-negotiable.

1. Accountability over control is mandatory. The protocol records and verifies behavior; it does not control system behavior.
2. Event-first design is mandatory. Protocol history is built from events and their causal relationships.
3. Minimal persistent state is mandatory. Meaning is derived primarily from event sequences, verifiable artifacts, and linked evidence rather than mutable global state.
4. Protocol artifacts are immutable once published. Histories are append-only. Corrections require new artifacts or events, not mutation.
5. Historical trace integrity is mandatory. Implementations must preserve the integrity of the accountability trace.
6. Responsibility is explicit. Meaningful actions must be attributable to an identity operating under a defined authority.
7. Identity and authority are distinct. Identity does not imply authority.
8. Watchers are distinct from actors. Observation and verification roles are separate from execution roles.
9. No implicit authority is permitted. Proposal is not authority. Validation is not adoption. Adoption is not activation.
10. Execution never decides constitutional meaning. Runtime execution may enforce admitted rules, but it must not infer, redefine, or generalize protocol meaning.
11. The protocol is implementation-neutral. Runtime systems, storage systems, and programming languages are not prescribed by the protocol.
12. Distributed compatibility is mandatory. The protocol does not require a single centralized executor or a global ordering authority.
13. Independent verification is mandatory in principle. Artifacts, evidence, and histories must support independent examination.
14. Validation rules that declare fail-closed behavior are mandatory as written. No fallback, bypass, warning-only, partial-acceptance, or reinterpretation path is permitted where prohibited by `VALIDATION/`.
15. Runtime trust mutation is forbidden where trust-anchor rules apply. Implementations must not create new authority at runtime.
16. Content-addressed identity and deterministic interpretation are mandatory where specified. Identical inputs must yield identical identifiers or validation outcomes where the repository states that requirement.
17. Once the protocol reaches `v1.0`, a valid trace produced under `v1.x` must remain interpretable under future versions.

## 6. Executor Relationship

Executors consume the protocol. They may:

- accept protocol-defined inputs
- validate artifacts and bindings
- execute within their own runtime boundaries
- emit protocol artifacts
- materialize evidence needed for verification

Executors must remain downstream of protocol meaning.

Executors must not:

- redefine protocol primitives
- redefine extension semantics
- invent alternate authority models
- create local constitutional meaning that contradicts this repository
- treat mutable runtime state as protocol authority
- use undocumented fallback authority
- treat proposal artifacts as adopted authority
- treat execution success as proof of semantic validity

Where governance and validation texts define binding, sealing, trust-anchor, signer-chain, temporal, or semantic-dependency rules, executors may enforce them. They do not own those rules.

If an executor contradicts this Constitution or any authoritative upstream protocol text, the executor is wrong.

## 7. Evolution Rules

The protocol may evolve, but evolution is constrained.

Core principles are intended to remain stable even as implementations evolve.

During `v0.x`, breaking changes are still possible. At this stage, the conceptual model is being stabilized, schemas may change, artifact field naming may change, event envelope details may change, and validation and trace reconstruction strategy may change.

A change is breaking if it:

- modifies the meaning of protocol primitives
- alters artifact semantics
- invalidates previously valid traces

Breaking changes require a new major version once the protocol reaches stable versioning expectations.

Governance lifecycle extensions apply only where governed object classes explicitly adopt them. In those areas:

- proposal, procedural validation, adoption, sealing, and activation eligibility are distinct lifecycle stages
- adoption is authority-bearing
- sealing binds admitted objects to immutable evidence
- activation eligibility is not implied by proposal, validation, or adoption unless explicitly defined

A valid extension must preserve constitutional boundaries already stated in the repository. In particular:

- implementation additions must not redefine core meaning
- delegated authority must not widen parent authority where signer-chain rules apply
- semantic dependencies must be explicit where semantic version binding applies
- older artifacts must not be reinterpreted under changed semantics when version binding or compatibility rules forbid it

Governance lifecycle semantics are optional extensions rather than protocol core. Profile-specific governance, engine meaning, and executor lifecycle behavior belong in other repositories.

## 8. Meta-State

`META/STATUS.md` defines current protocol maturity. Boundary Protocol is currently:

- Status: Experimental
- Version: `v0.1`
- Stability: not production-stable

At `v0.1`, the most stable areas are:

- accountability positioning
- the core primitives of intent, transition, and state
- the responsibility model of identity, authority, and watcher
- the event-first design direction

Areas still evolving include:

- schema details
- artifact field naming conventions
- event envelope details
- validation and trace reconstruction strategy
- implementation guidance

`META/VERSION.md` defines the version model `MAJOR.MINOR` and the staged path:

- `v0.x` experimental design
- `v0.5` structural stabilization
- `v0.9` freeze candidate
- `v1.0` stable protocol

In protocol context, a release marks a change in maturity and compatibility expectations of the specification. A stable release means core primitives are fixed, artifact contracts are stable, schema compatibility is guaranteed, and implementations may safely depend on the specification. Documentation clarification, examples, and rationale expansion do not change protocol version if they do not change protocol meaning.

### Constitutional Gaps Detected

- `VALIDATION/` contains strong authority, signer, trust-anchor, and semantic-binding rules. The repository does not yet fully distinguish which of these rules are minimal protocol-core invariants and which are domain-specific validation layers built above the accountability core.
- `CORE/TRANSITION_MODEL.md` defines the primitive as Transition, while `CONTRACTS/TRANSITION_EVENT_CONTRACT.md` and `SCHEMAS/transition_event.schema.json` define the concrete artifact as Transition Event. The repository does not fully normalize whether Transition is an abstract primitive, a contract name, or an artifact class layered through events.
- `CORE/EVENT_MODEL.md` says each event corresponds to the creation of a protocol artifact, but it also states that events create or reference objects. The exact rule for when an event creates versus references an artifact is not fully specified.
- `RESPONSIBILITY/AUTHORITY_MODEL.md` says additional capability types may be defined by implementations. The repository does not define the constitutional boundary for when implementation-defined capability types remain protocol-compatible rather than meaning-changing.
- `META/STATUS.md` identifies validation and trace reconstruction strategy as still evolving, while `VALIDATION/` now contains strong normative fail-closed binding texts. The repository does not yet fully distinguish stable validation invariants from profile-specific or still-evolving validation layers.
