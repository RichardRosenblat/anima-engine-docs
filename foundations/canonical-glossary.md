# Canonical Glossary

**Related Documentation:**
* [Architecture Overview](architecture/anima-architecture.md) - Complete system architecture
* [Seed System](architecture/seed-system.md) - Identity initialization
* [Module Types and Leases](architecture/module-types-and-leases.md) - Module lifecycle
* [Event Architecture](architecture/event-architecture.md) - Observability and input
* [Cognitive Kernel](architecture/cognitive-kernel.md) - Core as supervisor

---

## Purpose

These definitions are **canonical** and must **never drift**.

All ANIMA documentation, code, and discussions must use these exact definitions. Ambiguity in terminology leads to architectural confusion and implementation errors.

---

## System Components

### Engine

The entirety of the ANIMA system.

**Components:**
* Core (Cognitive Kernel)
* Seed
* MTL (Medial Temporal Lobe; memory domain)
* Capabilities (contracts)
* Modules (out-of-process)
* Adapters (pure translation)
* Actuators (effectful execution)
* Cortex (mandatory cognitive model)
* Arcuate (optional NLP model)

**Characteristics:**
* Identity-agnostic
* Modular and extensible
* Process-isolated from modules
* Event-reasoning based

**Related:** [Architecture Overview](architecture/anima-architecture.md)

---

### Core (Cognitive Kernel)

The reasoning and supervisory system inside the engine.

**Not** a simple loop — the Core operates as a **cognitive kernel** that:

* Ingests structured events (never raw input)
* Queries memory with controlled slices
* Applies Seed constraints and behavioral policies
* Reasons via Cortex (mandatory cognitive model)
* Optionally processes language via Arcuate (optional NLP model)
* Manages concurrent tasks with explicit lifecycle
* Routes and classifies interruptions
* Produces **intent graphs**, not direct effects
* Supervises execution through lease-gated modules

**Kernel Responsibilities:**
* Task lifecycle management (spawn, run, pause, cancel, complete)
* Interrupt routing and arbitration
* Event dispatch and prioritization
* Capability-aware execution control
* Observability and audit logging

**Core Rules:**
* Never touches the world directly
* Never blocks on task execution
* Never loads third-party code
* Produces intent, not effects
* Supervises work, doesn't execute it

**Related:** [Cognitive Kernel](architecture/cognitive-kernel.md)

---

### Seed

See **[ANIMA Seed](#anima-seed)** in the Identity & Identification section for the complete canonical definition.

**Quick Reference:**
* A static, declarative configuration artifact defining identity priors
* Memoryless (contains no learned memories or experiences)
* Combined with Memory to form an ANIMA Identity
* Portable and shareable (except ANIMA Prime Seed)

**Related:** [Seed System](architecture/seed-system.md), [ANIMA Identity](#anima-identity)

---

### MTL (Medial Temporal Lobe)

The **memory domain** responsible for storage, retrieval, decay, and promotion of identity-local memory.

**Responsibilities:**
* Memory storage mechanics (episodic, semantic, narrative layers)
* Retrieval and querying with controlled slices
* Decay and promotion policy enforcement
* Confidence tracking and provenance
* Memory integrity and boundaries

**Characteristics:**
* Identity-local (never shared between Identities)
* Mediates all memory access (prevents direct full access)
* Enforces memory policies defined by Seed
* Provides controlled memory slices to Cortex
* Operates as a domain boundary within the Core

**Related:** [Memory Integrity](../safety/memory-integrity.md), [AI Model Topology](../architecture/ai-model-topology.md)

---

### Memory

Identity-local data describing past and present state, managed by the MTL domain.

**Contains:**
* Past interactions (episodic)
* Observations (semantic facts)
* Task states (active and historical)
* Confidence-weighted facts

**Characteristics:**
* Identity-local (never shared between Identities)
* Fallible and queryable (never blindly trusted)
* Confidence-weighted
* Subject to decay and promotion policies
* Part of an ANIMA Identity (Seed + Memory)

**Memory Layers:**
* Episodic — specific events and interactions
* Semantic — extracted facts and knowledge
* Narrative — high-level story and identity continuity

**Memory Rules:**
* Informs reasoning, never overrides policy
* Queried through MTL with controlled slices
* Tracked as observed, remembered, inferred, or unknown
* No cross-Identity sharing ever

**Related:** [Memory Integrity](../safety/memory-integrity.md), [ANIMA Identity](#anima-identity)

---

### Capability

A **declarative contract** describing what the Core is allowed to intend.

**Examples:**
* `send_message`
* `speak_audio`
* `move_robot`
* `read_file`

**Characteristics:**
* Symbolic (URN-based)
* Contains no logic or I/O
* Permission-gated by security layer
* Seed-restricted (allow/deny lists)
* Versioned and immutable
* Declared via JSON Schemas

**Capability Contract Defines:**
* Method signatures and payloads
* Side-effect class (pure / reversible / irreversible)
* Interruption policy (soft-stop / hard-stop / checkpoint / non-interruptible)
* Maximum lease duration
* Abortability guarantees

**Related:** [Architecture Overview](architecture/anima-architecture.md)

---

### Module

An **out-of-process implementation** of one or more capabilities.

**Structure (Adapter-Actuator Split):**
* **Adapter** — Pure translation and schema validation (deterministic)
* **Actuator** — Effectful execution (APIs, hardware, side effects)

**Characteristics:**
* Runs in separate process from Core
* Communicates over gRPC with mTLS
* Requires valid lease to execute
* Never contains Core logic or reasoning
* Declares module type (I, II, or III)

**Module Types:**
* **Type I** — Ephemeral Private (lease-bound, terminates on Zero-Lease)
* **Type II** — Resident Private (standby on Zero-Lease, single Core)
* **Type III** — Resident Shared (multi-tenant, infrastructure-governed)

**Module Rules:**
* Capture inputs or execute outputs
* Never think or decide
* Operate only under valid lease
* Emit structured observability events

**Related:** [Module Types and Leases](architecture/module-types-and-leases.md), [Adapter-Actuator Split](architecture/adapter-actuator-split.md)

---

### Adapter

A **pure translation layer** between representations, running out-of-process.

**Input Adapters:**
* Transform external signals → structured events
* Map platform events to `input.nl`, `input.system`, or `input.semantic`
* No side effects, no I/O beyond gRPC

**Output Adapters:**
* Transform Core intent → module command schema
* Validate payloads against capability JSON Schemas
* Enforce lease scope
* Invoke Actuators

**Characteristics:**
* Deterministic and pure
* No memory access
* No business logic or policy
* No direct external I/O (except gRPC to Core/Actuator)
* Schema-driven validation

**Purpose:**
* Protect Core from format pollution
* Maintain process boundaries
* Enable clean hexagonal architecture

**Related:** [Adapter-Actuator Split](architecture/adapter-actuator-split.md)

---

### Actuator

The **effectful execution component** within a module, running out-of-process.

**Responsibilities:**
* Execute real-world side effects (APIs, hardware, platforms)
* Enforce lease scope and interruption policies
* Emit observability events (capability.invoked, capability.completed, capability.interrupted)
* Contain zero Core logic or reasoning

**Characteristics:**
* Invoked by Adapter (same module process)
* Operates under valid lease only
* Respects interruption policies
* Subject to Task Recovery Grace Period constraints

**Related:** [Adapter-Actuator Split](architecture/adapter-actuator-split.md)

---

### Intent

A structured description of **what should happen**, not how.

**Produced by:** Core (Cognitive Kernel)
**Consumed by:** Adapters and Actuators (via modules)

**Contains:**
* What (capability URN + payload)
* When (timing constraints)
* Where (target/scope)
* How much (resource limits)
* Why (reasoning context)
* Contingencies (what if failure)
* Confidence scores

**Characteristics:**
* Auditable
* Loggable
* Replayable
* Structured and schema-validated
* Never executed directly by Core

**Related:** [Architecture Overview](architecture/anima-architecture.md)

---

### Task

A long-lived unit of work the engine undertakes, solved with series of Intents.

**Characteristics:**
* Persist across time
* Explicitly created and identifiable
* Observable in event timelines
* Interruptible by design
* Cooperative in execution

**Task Properties:**
* Unique span_id and execution_id
* Declared interruptible flag
* Interruption policy
* Lifecycle state (spawned, running, paused, cancelled, completed)

**Task Rules:**
* MUST expose lifecycle state
* MUST periodically check for control signals
* MUST terminate cleanly when interrupted
* MUST NOT assume uninterrupted execution

**Related:** [Cognitive Kernel](architecture/cognitive-kernel.md)

---

### Lease

A **cryptographic warrant** of a connection between a Core and a Module, with allowed actions.

**Contains:**
* Lease ID (connection identifier)
* Consumer identity (Core Instance URN)
* Provider identity (Module URN)
* Permitted capability methods (scope)
* Lease epoch (monotonic, anti-desynchronization)
* Expiration time

**Lease Authority:**
* **Core is sole authority** to issue, renew, modify, or revoke leases
* Modules only acknowledge, enforce, or refuse execution
* No symmetric authorization

**Lease Rules:**
* All execution requires valid lease
* No lease, no execution (Zero-Lease State)
* Stale epochs are rejected immediately
* Scope changes require new epoch
* Cryptographic proof required for every request

**Related:** [Module Types and Leases](architecture/module-types-and-leases.md)

---

## Core Routing & Execution Components

### Gateway

A **routing boundary** within the Core that provides clean separation between domain logic and infrastructure.

**Architectural Role:**
* Accept typed operation requests
* Dispatch to appropriate port/client implementations
* Enforce boundary constraints without embedding policy
* Maintain hexagonal architecture separation

**Relationship to Core:**
* Sits at the edge of Core domain logic
* Connects domain operations to infrastructure ports
* Prevents domain contamination with infrastructure concerns
* Enables swappable implementations behind stable interfaces

**Boundary Rules:**
* No domain policy ("should we do X?")
* No direct external I/O
* No business logic
* Pure routing and constraint enforcement

**Purpose:**
* Preserve architectural boundaries
* Enable testability through port injection
* Support multiple domain gateways (cognition, MTL, language)
* Keep routing logic explicit and auditable

**Related:** ADR-006, ADR-007

---

### Registry

A **discovery and tracking authority** that maintains runtime knowledge of available capabilities without controlling authorization.

**Architectural Role:**
* Discover and index available capabilities
* Track capability provider health and liveness
* Provide endpoint resolution for capability requests
* Maintain attestation and handshake state

**Relationship to Core:**
* Consulted during task execution to resolve capabilities
* Separates "what exists" from "what is permitted"
* Enables dynamic capability availability
* No authority over lease decisions or permissions

**Authority Boundaries:**
* Tracks capability availability (not permission)
* Provides connection information (not authorization)
* Monitors health (not security policy)

**Purpose:**
* Decouple capability discovery from authorization
* Enable runtime capability registration
* Support module lifecycle without policy creep
* Make capability topology observable

**Related:** ADR-003

---

### Lease Authority

A **dedicated authorization system** within the Core that controls all module execution permissions.

**Architectural Role:**
* Issue, renew, revoke, and validate leases
* Manage lease epochs and scope
* Enforce "no lease, no execution" invariant
* Maintain cryptographic proof requirements

**Relationship to Core:**
* Core-owned and Core-controlled exclusively
* Never delegated to modules or external components
* Consulted before every module invocation
* Single source of truth for execution authorization

**Authority Boundaries:**
* Only component that mutates lease state
* Cannot be bypassed by any other component
* Enforces process isolation guarantees
* Maintains epoch-based anti-replay protection

**Purpose:**
* Centralize authorization decisions
* Prevent distributed permission creep
* Enable audit-grade execution tracking
* Reinforce security invariants

**Related:** ADR-003

---

### Synapse

A **transport abstraction** that binds lease and execution context to module communication.

**Architectural Role:**
* Represent connection to out-of-process module
* Attach lease proofs and execution metadata to all invocations
* Handle serialization and transport-level concerns
* Provide opaque invocation surface to higher layers

**Relationship to Core:**
* Used by orchestration layer to invoke modules
* Enforces metadata requirements automatically
* Abstracts transport mechanism from business logic
* Makes process boundaries explicit

**Boundary Rules:**
* No business semantics or policy
* No authorization logic (delegates to lease authority)
* Transport and serialization only
* Metadata-enriched but domain-agnostic

**Purpose:**
* Enforce process boundary separation
* Ensure lease binding for all invocations
* Enable transport evolution without domain changes
* Make execution traceability automatic

**Related:** ADR-003

---

### Orchestrator

The **execution coordination layer** that supervises task lifecycle without embedding domain policy.

**Architectural Role:**
* Manage task and span lifecycle states
* Coordinate capability resolution via registry
* Validate lease context via lease authority
* Route tasks to appropriate module ports
* Aggregate execution outcomes into events

**Relationship to Core:**
* Sits between cognition (Cortex) and execution (modules)
* Supervised execution spine, not policy engine
* Consumes intent, produces structured events
* Cooperates with interruption system

**Boundary Rules:**
* No domain policy (retry rules, importance, business meaning)
* No planning logic (belongs in cognition layer)
* No lease authority bypass
* Pure coordination without decision-making

**Purpose:**
* Separate supervision from decision-making
* Enable consistent execution patterns
* Support observability through event emission
* Prevent policy accumulation in infrastructure

**Related:** ADR-008, ADR-003, ADR-004

---

## AI Models

### Cortex

The **mandatory cognitive model** wrapper connected to the engine for reasoning.

**Responsibilities:**
* Plan generation and refinement
* Task expansion and contingency modeling
* Semantic spine reasoning
* Capability-aware decision making
* Enforcing seed-defined behavioral constraints
* Reasoning over controlled memory slices

**Characteristics:**
* Mandatory (Core cannot function without it)
* Interchangeable via ICognitive interface
* Replaceable without changing engine
* Access to controlled memory slices

**Related:** [AI Model Topology](architecture/ai-model-topology.md)

---

### Arcuate

The **optional natural language processing** model wrapper.

**Responsibilities:**
* Natural language parsing (`input.nl` → `input.semantic`)
* Semantic frame extraction
* Natural language generation (intent → linguistic surface)
* Linguistic normalization and realization

**Constraints:**
* Optional (NLP can be delegated to modules)
* MUST NOT perform autonomous planning
* MUST NOT generate or mutate tasks
* MUST NOT access episodic or narrative memory
* MUST NOT override Cortex decisions
* Operates with restricted or empty memory

**Related:** [AI Model Topology](architecture/ai-model-topology.md)

---

## Observability & Events

### Event

A structured, immutable fact that occurred in the system.

**Event Types:**
* `input.nl` — Natural language (pre-semantic)
* `input.system` — System/mechanical facts
* `input.semantic` — Post-interpretation asserted meaning
* `capability.invoked` — Capability execution started
* `capability.completed` — Capability execution finished
* `capability.interrupted` — Capability execution interrupted

**Required Fields:**
* execution_id (partition boundary)
* trace_id (end-to-end causal chain)
* span_id / parent_span_id (hierarchical structure)
* thread_id (concurrency visibility)
* timestamp (RFC3339)
* source (core | module:<name>)
* type (stable event type identifier)
* payload (structured data only, no free text)

**Characteristics:**
* Immutable
* Append-only
* Execution-scoped
* Causally linked
* Concurrency-safe

**Purpose:**
* ANIMA does not log text; it records facts
* Events are the source of truth
* Enable deterministic replayability
* Support audit-grade observability

**Related:** [Event Architecture](architecture/event-architecture.md)

---

### Execution

A bounded Core runtime representing a single causal universe.

**Characteristics:**
* Globally unique execution_id
* Highest-level isolation boundary for observability
* All events partitioned by execution
* Metadata tracked (started_at, ended_at, core_version, host)

**File Layout:**
```
events/
  executions/
    exec-<uuid>/
      metadata.json
      events.ndjson
```

**Related:** [Event Architecture](architecture/event-architecture.md)

---

## Identity & Identification

### ANIMA Instance

A single running execution of the ANIMA engine.

**Definition:** An ANIMA Instance is a running process that executes the ANIMA engine logic.

**Properties:**
* Ephemeral (exists only during runtime, destroyed on shutdown)
* Owns execution lifecycle and resources
* Hosts one active ANIMA Identity at a time
* Identified by a unique Instance URN generated at startup

**What it is NOT:**
* Not an identity (identities are persistent, instances are ephemeral)
* Not persistent across restarts
* Not what provides continuity (continuity comes from Identity: Seed + Memory)

**Canonical Sentence:** "An ANIMA Instance executes an ANIMA Identity derived from an ANIMA Seed and Memories."

**Related:** [Seed System](../architecture/seed-system.md), [System Boundaries](system-boundaries.md)

---

### ANIMA Identity

A materialized identity composed of an ANIMA Seed plus its associated Memory.

**Definition:** An ANIMA Identity is the persistent, evolving personality and knowledge state that can be loaded into an ANIMA Instance.

**Composition:**
* **ANIMA Seed** (identity priors and configuration)
* **Memory** (episodic, semantic, and narrative layers that evolve over time)

**Properties:**
* Persistent across Instance executions
* Evolves over time through experience
* Can be migrated between Instances (with Seed + Memory)
* Instance-independent (the "who" that transcends runtimes)

**What it is NOT:**
* Not a running process (that's an Instance)
* Not ephemeral (persists across runtimes)
* Not cloneable without complete migration of Seed and Memories

**Relationship Model:** Seed → Identity (Seed + Memory) → Instance (executes Identity)

**Related:** [Seed System](../architecture/seed-system.md), [Memory Integrity](../safety/memory-integrity.md)

---

### ANIMA Seed

A cryptographic, unmaterialized identity definition that contains identity priors but no memory.

**Definition:** An ANIMA Seed is a static, declarative configuration artifact that defines the initial parameters and policies for an ANIMA Identity.

**Contains:**
* Personality parameters (tone, verbosity, emotional expressiveness)
* Behavioral policies (risk tolerance, uncertainty handling)
* Capability allow/deny lists
* Default interaction style
* Initial self-concept and narrative framing
* Memory decay and promotion policies

**Properties:**
* Memoryless (contains no learned experiences or conversation history)
* Portable (can be distributed and shared)
* Defines who ANIMA can become (not what she remembers)
* Used to initialize an Identity

**What it is NOT:**
* Not an identity by itself (Identity = Seed + Memory)
* Not executable code (declarative configuration only)
* Not stateful (does not evolve or change)

**Must NOT contain:**
* Learned memories
* User data
* Conversation logs
* Private behavior evolution
* Execution logic
* Cross-instance state

**Characteristics:**
* Declarative configuration data, not executable code
* Defines parameters and policies interpreted by the engine
* Immutable during runtime
* Instance-defining but not instance-unique
* Loaded once at startup

**Purpose:**
* Separate engine from identity
* Enable multiple independent identities
* Support commercialization (engine vs. identity IP)
* Prevent identity and memory leakage

**Related:** [Seed System](../architecture/seed-system.md)

---

### ANIMA Prime Identity

A special, protected identity with unique non-exportable Seed and private evolving Memory.

**Definition:** ANIMA Prime Identity is the canonical public-facing identity used exclusively for streaming, VTuber embodiment, and reference implementation.

**Properties:**
* Unique protected Seed (never distributed or shared)
* Private evolving Memory (never exported or shared)
* Non-cloneable (cannot be instantiated by third parties)
* Non-exportable (Seed and Memory remain protected IP)

**What it is NOT:**
* Not cloneable or forkable
* Not distributable to users
* Not instantiable outside authorized contexts
* Not available for private use

**Critical Constraints:**
* Prime Seed is NEVER shared publicly
* Prime Memory is NEVER exported or distributed
* No private Instance may load Prime Identity
* Prime Identity cannot be cloned or forked

**Purpose:**
* Public-facing streaming and VTuber incarnation
* Reference implementation of ANIMA capabilities
* Demonstration of engine design philosophy
* Protected commercial identity

**Related:** [Seed System](../architecture/seed-system.md), [Licensing Model](../governance/licensing-model.md)

---

### Anima Instance URN

An ANIMA URN that opaquely and uniquely identifies a specific runtime of the ANIMA process.

**Also known as:** Core Instance URN

**Purpose:**
* Track and manage the lifecycle of a specific ANIMA Instance
* Enforce instance isolation boundaries
* Enable identity-scoped memory and execution
* Bind leases to specific Core runtimes

**Format:** `urn:anima:core:instance@<version>:<instance-id>`

**Characteristics:**
* Generated by the Core at startup
* Unique to each runtime invocation
* Opaque identifier (not derived from content)
* Remains stable for the lifetime of the runtime
* Used in event envelopes to identify event source
* Validated at memory access layer

**Where Used:**
* Event envelope (`instance_urn` field)
* Lease consumer identity
* Memory isolation enforcement
* mTLS certificate encoding

**Related:** [ANIMA URN Specification](../specs/anima-urn.md), [System Boundaries](system-boundaries.md)

---

### Core URN

An ANIMA URN that uniquely identifies the ANIMA Core within the ANIMA ecosystem.

**Purpose:**
* Serve as a stable reference point for Core services
* Identify the Core installation across runtime instances
* Enable Core-to-Core identification in future distributed scenarios
* Bind modules to specific Core installations

**Format:** `urn:anima:core:core.identifier@<version>:<instance-id>`

**Characteristics:**
* Established during initial setup of the Core system
* Remains constant across different runtime instances
* Opaque identifier (not derived from content)
* Stable across Core restarts
* Used in event envelopes to identify Core source
* Encoded in mTLS certificates

**Where Used:**
* Event envelope (`core_urn` field)
* Module binding and trust establishment
* Core identity in distributed scenarios
* Long-term capability authorization

**Distinction from Instance URN:**
* Core URN identifies the **installation/setup**
* Instance URN identifies the **specific runtime**
* Core URN is stable across restarts
* Instance URN changes with each execution

**Related:** [ANIMA URN Specification](../specs/anima-urn.md), [Module Types and Leases](../architecture/module-types-and-leases.md)

---

## Architecture Patterns

### Package

A distributable group of modules, adapters, and capability definitions.

**Contains:**
* Related capabilities (bundled)
* Adapters for those capabilities
* Actuators for effectful execution
* Capability contracts (JSON Schemas + manifest)
* Transport layer (gRPC server/client, mTLS)

**Characteristics:**
* Versioned
* Can be shared
* Installed into ANIMA instance to extend functionality

---

### Semantic Spine

An explicit data structure for semantic representation of messages.

**Purpose:**
* Ensure consistent and meaningful communication
* Standardized way to represent meaning and context
* Language-agnostic semantic representation

**Characteristics:**
* Encapsulate intent and context
* Facilitate accurate interpretation
* Language-agnostic
* Support complex interactions
* Enable better memory encoding

---

## Safety & Risk

### System Risk Level

The maximum potential impact to the system's integrity, security, availability, or trust boundaries if the operation behaves incorrectly or is abused.

**Characteristics:**
* Expresses impact severity, not likelihood
* Independent of User Risk Level
* Evaluated for every capability
* Stable across policy changes

**Purpose:**
* Inform consent mechanisms
* Guide sandboxing decisions
* Support policy enforcement
* Enable audit classifications

**Related:** [Safety Model](../safety/safety-model.md), [System Boundaries](system-boundaries.md)

---

### User Risk Level

The maximum potential harm to the user's privacy, finances, reputation, autonomy, health (mental or physical), or irreversible outcomes if the operation is executed incorrectly or without full understanding.

**Characteristics:**
* Expresses impact severity, not likelihood
* Independent of System Risk Level
* Evaluated for every capability
* Stable across policy changes

**Purpose:**
* Inform consent mechanisms
* Guide confirmation requirements
* Support policy enforcement
* Enable audit classifications

**Related:** [Safety Model](../safety/safety-model.md), [System Boundaries](system-boundaries.md)

---

## Process & State

### Zero-Lease State

The state of a Module when it has no currently valid lease.

**Occurs on:**
* Lease expiration
* Lease revocation
* Connection loss
* Failure to establish lease within bounded window

**Invariant:** **No Lease, No Execution**

**Module Behavior by Type:**
* **Type I** — Graceful termination after Task Recovery Grace Period
* **Type II** — Enter Standby Mode (inert, not terminated)
* **Type III** — Per-tenant isolation (affects only owning Core)

**Related:** [Module Types and Leases](architecture/module-types-and-leases.md)

---

### Task Recovery Grace Period

A bounded interval during which in-flight task state may be retained.

**Purpose:**
* Support rollback, retry, or error reporting for a single lease

**Constraints:**
* Non-renewable and non-extendable
* Non-accessible for new leases or tasks
* Scoped to a single lease
* State destroyed after period expires

**Related:** [Module Types and Leases](architecture/module-types-and-leases.md)

---

## Summary

This glossary defines the **canonical vocabulary** of ANIMA.

> **Precision in language prevents confusion in architecture.**
>
> **These definitions are stable, documented, and enforced.**
>
> **Drift in terminology leads to drift in implementation.**
