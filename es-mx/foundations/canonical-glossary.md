# Glosario Canónico

**Documentación Relacionada:**
* [Visión General de la Arquitectura](architecture/anima-architecture.md) - Arquitectura completa del sistema
* [Sistema de Seeds](architecture/seed-system.md) - Inicialización de identidad
* [Tipos de Módulos y Leases](architecture/module-types-and-leases.md) - Ciclo de vida de módulos
* [Arquitectura de Eventos](architecture/event-architecture.md) - Observabilidad y entrada
* [Kernel Cognitivo](architecture/cognitive-kernel.md) - Core como supervisor

---

## Propósito

Estas definiciones son **canónicas** y **nunca deben desviarse**.

Toda documentación, código y discusiones de ANIMA deben usar exactamente estas definiciones. La ambigüedad en la terminología conduce a confusión arquitectónica y errores de implementación.

---

## Componentes del Sistema

### Motor

La totalidad del sistema ANIMA.

**Componentes:**
* Core (Kernel Cognitivo)
* Seed
* Memory (instance-local)
* Capabilities (contracts)
* Modules (out-of-process)
* Adapters (pure translation)
* Actuators (effectful execution)
* Cortex (mandatory cognitive model)
* Arcuate (optional NLP model)

**Características:**
* Agnóstico de identidad
* Modular y extensible
* Aislado de proceso de los módulos
* Basado en razonamiento por eventos

**Relacionado:** [Visión General de la Arquitectura](architecture/anima-architecture.md)

---

### Core (Kernel Cognitivo)

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

**Relacionado:** [Kernel Cognitivo](architecture/cognitive-kernel.md)

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

**Características:**
* Data, not code
* Immutable during runtime
* Instance-defining but not instance-unique
* Loaded once at startup

**Purpose:**
* Separate engine from identity
* Enable multiple independent instances
* Support commercialization (engine vs. identity IP)
* Prevent identity and memory leakage

**Relacionado:** [Sistema de Seeds](architecture/seed-system.md)

---

### Memory

Instance-local data describing past and present state.

**Contains:**
* Past interactions (episodic)
* Observations (semantic facts)
* Task states (active and historical)
* Confidence-weighted facts

**Características:**
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

**Relacionado:** [Memory Integrity](memory-integrity.md)

---

### Capability

A **declarative contract** describing what the Core is allowed to intend.

**Examples:**
* `send_message`
* `speak_audio`
* `move_robot`
* `read_file`

**Características:**
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

**Relacionado:** [Visión General de la Arquitectura](architecture/anima-architecture.md)

---

### Module

An **out-of-process implementation** of one or more capabilities.

**Structure (Adapter-Actuator Split):**
* **Adapter** — Pure translation and schema validation (deterministic)
* **Actuator** — Effectful execution (APIs, hardware, side effects)

**Características:**
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

**Relacionado:** [Tipos de Módulos y Leases](architecture/module-types-and-leases.md), [Adapter-Actuator Split](architecture/adapter-actuator-split.md)

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

**Características:**
* Deterministic and pure
* No memory access
* No business logic or policy
* No direct external I/O (except gRPC to Core/Actuator)
* Schema-driven validation

**Purpose:**
* Protect Core from format pollution
* Maintain process boundaries
* Enable clean hexagonal architecture

**Relacionado:** [Adapter-Actuator Split](architecture/adapter-actuator-split.md)

---

### Actuator

The **effectful execution component** within a module, running out-of-process.

**Responsibilities:**
* Execute real-world side effects (APIs, hardware, platforms)
* Enforce lease scope and interruption policies
* Emit observability events (capability.invoked, capability.completed, capability.interrupted)
* Contain zero Core logic or reasoning

**Características:**
* Invoked by Adapter (same module process)
* Operates under valid lease only
* Respects interruption policies
* Subject to Task Recovery Grace Period constraints

**Relacionado:** [Adapter-Actuator Split](architecture/adapter-actuator-split.md)

---

### Intent

A structured description of **what should happen**, not how.

**Produced by:** Core (Kernel Cognitivo)
**Consumed by:** Adapters and Actuators (via modules)

**Contains:**
* What (capability URN + payload)
* When (timing constraints)
* Where (target/scope)
* How much (resource limits)
* Why (reasoning context)
* Contingencies (what if failure)
* Confidence scores

**Características:**
* Auditable
* Loggable
* Replayable
* Structured and schema-validated
* Never executed directly by Core

**Relacionado:** [Visión General de la Arquitectura](architecture/anima-architecture.md)

---

### Task

A long-lived unit of work the engine undertakes, solved with series of Intents.

**Características:**
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

**Relacionado:** [Kernel Cognitivo](architecture/cognitive-kernel.md)

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

**Relacionado:** [Tipos de Módulos y Leases](architecture/module-types-and-leases.md)

---

## Componentes de Enrutamiento y Ejecución del Core

### Gateway

Un **límite de enrutamiento del Core** que posee una implementación de puerto/cliente y expone un único punto de entrada.

**Punto de Entrada:**
* `send(operation) -> result`

**Responsabilidades:**
* Aceptar objetos de operación
* Despachar por tipo de operación
* Aplicar restricciones de límite (tipado, operaciones permitidas, reglas "solo traductor", etc.)
* Reenviar a una implementación de puerto/cliente inyectada

**No-responsabilidades:**
* Sin política de dominio (sin "¿deberíamos hacer X?")
* Sin I/O de infraestructura

**Características:**
* Límite de enrutamiento sin lógica de negocio
* Respaldado por implementaciones de puerto/cliente (ej: CortexPort, MemoryPort, ArcuateClient)
* Preserva límites de arquitectura hexagonal

**Relacionado:** ADR-012

---

### CortexGateway / MemoryGateway / ArcuateGateway

**Implementaciones específicas de gateway** para diferentes subsistemas del Core.

**CortexGateway:**
* Enruta operaciones cognitivas al puerto/cliente Cortex obligatorio
* Maneja solicitudes de razonamiento cognitivo

**MemoryGateway:**
* Enruta operaciones de memoria a puertos de memoria (repositorios, servicios de memoria)
* Sin política de memoria incorporada

**ArcuateGateway:**
* Enruta operaciones de traducción NLP a Arcuate (si está presente)
* DEBE permanecer solo traductor (sin planificación ni toma de decisiones)

**Relacionado:** ADR-012, ADR-009

---

### Operation Objects

**Objetos de operación tipados** despachados por Gateways que representan "qué comportamiento" sin codificar lógica de enrutamiento.

**Categorías de Ejemplo:**
* `ProcessEvent`
* `EvaluateIntent`
* `Reflect`
* `TranslateInputNlToSemantic`
* `RealizeIntentToNl`

**Características:**
* Type-safe
* Descriptores de comportamiento
* Agnóstico de enrutamiento
* Específico de dominio

**Relacionado:** ADR-012

---

### ModuleRegistry

La **autoridad de descubrimiento de capacidades y conexión**.

**Autoridad Para:**
* Descubrimiento de módulos disponibles
* Rastreo de atestación y handshake
* Rastreo de vivacidad
* Indexación de capacidades
* Devolver handles de endpoint activos para capacidades

**No Autoridad Para:**
* Permisos (eso es territorio de lease + política de seguridad)
* Concesión de lease
* Promoción/degradación de alcance

**Características:**
* Solo descubrimiento de capacidades
* Sin decisiones de autorización
* Rastrea salud y conectividad de módulos
* Proporciona mapeo capacidad-a-endpoint

**Relacionado:** ADR-012

---

### LeaseManager

Un **componente dedicado propiedad del Core** responsable de toda autoridad de lease.

**Responsabilidades:**
* Emisión de leases
* Renovación de leases
* Revocación de leases
* Gestión de epochs
* Aplicación de la semántica "sin lease, sin ejecución"

**Reglas de Autoridad de Lease:**
* Único componente permitido para mutar estado de lease
* Propiedad y control del Core
* Explícito y auditable
* Refuerza aislamiento de proceso

**Características:**
* Fuente única de verdad para estado de lease
* Consciente de epoch
* Componente crítico de seguridad
* Nunca delegado a módulos o puertos

**Relacionado:** ADR-012, ADR-003

---

### ModulePort / ModuleSession

Una **conexión de transporte opaca** vinculada a un módulo específico.

**Vincula:**
* `LeaseMeta` (lease_id, epoch, proofs)
* `ExecMeta` (execution_id, trace_id, span_id, thread_id)

**Superficie de Invocación:**
* `invoke(capability_urn, payload, meta) -> result`

**Responsabilidades:**
* Serialización/deserialización
* Adjuntar metadatos requeridos
* Reintentos para falla de transporte (si está configurado)
* Surfacing de errores de forma determinística

**No-responsabilidades:**
* Sin semántica de negocio
* Sin decisiones de autorización
* Sin lógica de política (ej: "cuántos reintentos porque es importante")

**Características:**
* Sesión vinculada a lease
* Transporte enriquecido con metadatos
* Opaco a la lógica de negocio
* Aplicación de límite de proceso

**Relacionado:** ADR-012, ADR-003

---

### TaskOrchestrator

La **columna vertebral de ejecución supervisada del Kernel**.

**Responsabilidades:**
* Gestionar ciclo de vida de tarea/span (spawn/run/pause/cancel/complete)
* Cooperar con enrutamiento y políticas de interrupción
* Resolver capacidades vía ModuleRegistry
* Obtener/validar contexto de lease vía LeaseManager antes de la invocación
* Despachar tareas a ModulePorts
* Agregar resultados y emitir eventos

**No-responsabilidades:**
* Sin codificación de política de dominio (ej: específica de intent "reintentar porque tiene significado de negocio")
* Sin lógica de planificación (eso pertenece a la cognición/Cortex)
* Sin bypass de autoridad de lease

**Características:**
* Supervisa ejecución, no ejecuta
* Coordinación libre de política
* Invocación controlada por lease
* Gestión de ciclo de vida orientada a eventos

**Relacionado:** ADR-012, ADR-008, ADR-003, ADR-004

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

**Características:**
* Mandatory (Core cannot function without it)
* Interchangeable via ICognitive interface
* Replaceable without changing engine
* Access to controlled memory slices

**Relacionado:** [AI Model Topology](architecture/ai-model-topology.md)

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

**Relacionado:** [AI Model Topology](architecture/ai-model-topology.md)

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

**Características:**
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

**Relacionado:** [Arquitectura de Eventos](architecture/event-architecture.md)

---

### Execution

A bounded Core runtime representing a single causal universe.

**Características:**
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

**Relacionado:** [Arquitectura de Eventos](architecture/event-architecture.md)

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

**Características:**
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

**Características:**
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

**Relacionado:** [Tipos de Módulos y Leases](architecture/module-types-and-leases.md)

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

**Relacionado:** [Tipos de Módulos y Leases](architecture/module-types-and-leases.md)

---

## Summary

This glossary defines the **canonical vocabulary** of ANIMA.

> **Precision in language prevents confusion in architecture.**
>
> **These definitions are stable, documented, and enforced.**
>
> **Drift in terminology leads to drift in implementation.**
