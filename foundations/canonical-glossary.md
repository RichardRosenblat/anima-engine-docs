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
* Memory (instance-local)
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

A **static, declarative configuration artifact** loaded at initialization.

**Contains:**
* Personality parameters (tone, verbosity, emotional expressiveness)
* Behavioral policies (risk tolerance, uncertainty handling)
* Capability allow/deny lists
* Default interaction style
* Initial self-concept and narrative framing
* Memory decay and promotion policies

**Must NOT contain:**
* Learned memories
* User data
* Conversation logs
* Private behavior evolution
* Execution logic
* Cross-instance state

**Characteristics:**
* Data, not code
* Immutable during runtime
* Instance-defining but not instance-unique
* Loaded once at startup

**Purpose:**
* Separate engine from identity
* Enable multiple independent instances
* Support commercialization (engine vs. identity IP)
* Prevent identity and memory leakage

**Related:** [Seed System](architecture/seed-system.md)

---

### Memory

Instance-local data describing past and present state.

**Contains:**
* Past interactions (episodic)
* Observations (semantic facts)
* Task states (active and historical)
* Confidence-weighted facts

**Characteristics:**
* Instance-local (never shared between instances)
* Fallible and queryable (never blindly trusted)
* Confidence-weighted
* Subject to decay and promotion policies

**Memory Layers:**
* Episodic — specific events and interactions
* Semantic — extracted facts and knowledge
* Narrative — high-level story and identity continuity

**Memory Rules:**
* Informs reasoning, never overrides policy
* Queried with controlled slices (prevents full access)
* Tracked as observed, remembered, inferred, or unknown
* No cross-instance sharing ever

**Related:** [Memory Integrity](memory-integrity.md)

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

A **Core routing boundary** that owns a port/client implementation and exposes a single entry point.

**Entry Point:**
* `send(operation) -> result`

**Responsibilities:**
* Accept operation objects
* Dispatch by operation type
* Enforce boundary constraints (typing, allowed ops, "translator-only" rules, etc.)
* Forward to an injected port/client implementation

**Non-responsibilities:**
* No domain policy (no "should we do X?")
* No infrastructure I/O

**Characteristics:**
* Routing boundary with no business logic
* Backed by port/client implementations (e.g., CortexPort, MemoryPort, ArcuateClient)
* Preserves hexagonal architecture boundaries

**Related:** ADR-012

---

### CortexGateway / MemoryGateway / ArcuateGateway

**Specific gateway implementations** for different Core subsystems.

**CortexGateway:**
* Routes cognitive operations to the mandatory Cortex port/client
* Handles cognitive reasoning requests

**MemoryGateway:**
* Routes memory operations to memory ports (repositories, memory services)
* No embedded memory policy

**ArcuateGateway:**
* Routes NLP translation operations to Arcuate (if present)
* MUST remain translator-only (no planning or decision-making)

**Related:** ADR-012, ADR-009

---

### Operation Objects

**Typed operation objects** dispatched by Gateways representing "which behavior" without encoding routing logic.

**Example Categories:**
* `ProcessEvent`
* `EvaluateIntent`
* `Reflect`
* `TranslateInputNlToSemantic`
* `RealizeIntentToNl`

**Characteristics:**
* Type-safe
* Behavior descriptors
* Routing-agnostic
* Domain-specific

**Related:** ADR-012

---

### ModuleRegistry

The **capability discovery and connection authority**.

**Authority For:**
* Discovery of available modules
* Attestation and handshake tracking
* Liveness tracking
* Capability indexing
* Returning live endpoint handles for capabilities

**Not Authority For:**
* Permissioning (that is lease + security policy territory)
* Lease granting
* Scope promotion/demotion

**Characteristics:**
* Capability discovery only
* No authorization decisions
* Tracks module health and connectivity
* Provides capability-to-endpoint mapping

**Related:** ADR-012

---

### LeaseManager

A **dedicated Core-owned component** responsible for all lease authority.

**Responsibilities:**
* Issuing leases
* Renewing leases
* Revoking leases
* Managing epochs
* Enforcing "no lease, no execution" semantics

**Lease Authority Rules:**
* Only component allowed to mutate lease state
* Core-owned and Core-controlled
* Explicit and auditable
* Reinforces process isolation

**Characteristics:**
* Single source of truth for lease state
* Epoch-aware
* Security-critical component
* Never delegated to modules or ports

**Related:** ADR-012, ADR-003

---

### ModulePort / ModuleSession

An **opaque transport connection** bound to a specific module.

**Binds:**
* `LeaseMeta` (lease_id, epoch, proofs)
* `ExecMeta` (execution_id, trace_id, span_id, thread_id)

**Invocation Surface:**
* `invoke(capability_urn, payload, meta) -> result`

**Responsibilities:**
* Serialization/deserialization
* Attaching required metadata
* Retries for transport failure (if configured)
* Surfacing errors deterministically

**Non-responsibilities:**
* No business semantics
* No authorization decisions
* No policy logic (e.g., "how many retries because it's important")

**Characteristics:**
* Lease-bound session
* Metadata-enriched transport
* Opaque to business logic
* Process boundary enforcement

**Related:** ADR-012, ADR-003

---

### TaskOrchestrator

The **Kernel's supervised execution spine**.

**Responsibilities:**
* Manage task/span lifecycle (spawn/run/pause/cancel/complete)
* Cooperate with interruption routing and policies
* Resolve capabilities via ModuleRegistry
* Obtain/validate lease context via LeaseManager before invocation
* Dispatch tasks to ModulePorts
* Aggregate outcomes and emit events

**Non-responsibilities:**
* No domain policy encoding (e.g., intent-specific "retry because of business meaning")
* No planning logic (that belongs in cognition/Cortex)
* No bypassing lease authority

**Characteristics:**
* Supervises execution, doesn't execute
* Policy-free coordination
* Lease-gated invocation
* Event-driven lifecycle management

**Related:** ADR-012, ADR-008, ADR-003, ADR-004

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
