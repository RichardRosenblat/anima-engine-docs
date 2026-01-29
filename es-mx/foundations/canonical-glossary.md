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
* MTL (Lóbulo Temporal Medial; subsistema de memoria)
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

### ANIMA Prime Identity

Una identidad especial y protegida con Seed único no-exportable y Memoria privada en evolución.

**Definición:** ANIMA Prime Identity es la identidad canónica orientada al público usada exclusivamente para streaming, encarnación VTuber e implementación de referencia.

**Propiedades:**
* Seed protegido único (nunca distribuido o compartido)
* Memoria privada en evolución (nunca exportada o compartida)
* No-clonable (no puede ser instanciada por terceros)
* No-exportable (Seed y Memoria permanecen como propiedad intelectual protegida)

**Lo que NO es:**
* No es clonable o bifurcable
* No es distribuible a los usuarios
* No es instanciable fuera de contextos autorizados
* No está disponible para uso privado

**Restricciones Críticas:**
* Prime Seed NUNCA se comparte públicamente
* Prime Memory NUNCA se exporta o distribuye
* Ninguna Instancia privada puede cargar Prime Identity
* Prime Identity no puede ser clonada o bifurcada

**Propósito:**
* Streaming público y encarnación VTuber
* Implementación de referencia de las capacidades de ANIMA
* Demostración de la filosofía de diseño del motor
* Identidad comercial protegida

**Relacionado:** [Sistema de Seeds](../architecture/seed-system.md), [Modelo de Licenciamiento](../governance/licensing-model.md)

---

### MTL (Lóbulo Temporal Medial)

El **subsistema de memoria** responsable del almacenamiento, recuperación, decaimiento y promoción de memoria instance-local.

**Responsabilidades:**
* Mecánicas de almacenamiento de memoria (capas episódica, semántica, narrativa)
* Recuperación y consulta con rebanadas controladas
* Aplicación de políticas de decaimiento y promoción
* Rastreo de confianza y procedencia
* Integridad y límites de memoria

**Características:**
* Instance-local (nunca compartida entre instancias)
* Media todo acceso a la memoria (previene acceso completo directo)
* Aplica políticas de memoria definidas por el Seed
* Proporciona rebanadas de memoria controladas al Cortex
* Opera como un límite de subsistema dentro del Core

**Relacionado:** [Memory Integrity](../safety/memory-integrity.md), [AI Model Topology](../architecture/ai-model-topology.md)

---

### Memory

Instance-local data describing past and present state, managed by the MTL domain.

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
* Queried through MTL with controlled slices
* Tracked as observed, remembered, inferred, or unknown
* No cross-instance sharing ever

**Relacionado:** [Memory Integrity](../safety/memory-integrity.md)

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

Un **límite de enrutamiento** dentro del Core que proporciona separación limpia entre lógica de dominio e infraestructura.

**Papel Arquitectónico:**
* Aceptar solicitudes de operación tipadas
* Despachar a implementaciones de puerto/cliente apropiadas
* Aplicar restricciones de límite sin incorporar política
* Mantener separación de arquitectura hexagonal

**Relación con el Core:**
* Se sitúa en el borde de la lógica de dominio del Core
* Conecta operaciones de dominio a puertos de infraestructura
* Previene contaminación del dominio con preocupaciones de infraestructura
* Permite implementaciones intercambiables detrás de interfaces estables

**Reglas de Límite:**
* Sin política de dominio ("¿deberíamos hacer X?")
* Sin I/O externo directo
* Sin lógica de negocio
* Enrutamiento puro y aplicación de restricciones

**Propósito:**
* Preservar límites arquitectónicos
* Permitir testabilidad a través de inyección de puerto
* Soportar múltiples gateways de subsistema (cognición, MTL, lenguaje)
* Mantener lógica de enrutamiento explícita y auditable

**Relacionado:** ADR-006, ADR-007

---

### Registry

Una **autoridad de descubrimiento y rastreo** que mantiene conocimiento en tiempo de ejecución de capacidades disponibles sin controlar autorización.

**Papel Arquitectónico:**
* Descubrir e indexar capacidades disponibles
* Rastrear salud y vivacidad de proveedores de capacidad
* Proporcionar resolución de endpoint para solicitudes de capacidad
* Mantener atestación y estado de handshake

**Relación con el Core:**
* Consultado durante ejecución de tarea para resolver capacidades
* Separa "qué existe" de "qué está permitido"
* Permite disponibilidad dinámica de capacidad
* Sin autoridad sobre decisiones de lease o permisos

**Límites de Autoridad:**
* Rastrea disponibilidad de capacidad (no permiso)
* Proporciona información de conexión (no autorización)
* Monitorea salud (no política de seguridad)

**Propósito:**
* Desacoplar descubrimiento de capacidad de autorización
* Permitir registro de capacidad en tiempo de ejecución
* Soportar ciclo de vida de módulo sin acumulación de política
* Hacer topología de capacidad observable

**Relacionado:** ADR-003

---

### Lease Authority

Un **sistema de autorización dedicado** dentro del Core que controla todos los permisos de ejecución de módulo.

**Papel Arquitectónico:**
* Emitir, renovar, revocar y validar leases
* Gestionar epochs y alcance de lease
* Aplicar invariante "sin lease, sin ejecución"
* Mantener requisitos de prueba criptográfica

**Relación con el Core:**
* Propiedad y control exclusivo del Core
* Nunca delegado a módulos o componentes externos
* Consultado antes de cada invocación de módulo
* Fuente única de verdad para autorización de ejecución

**Límites de Autoridad:**
* Único componente que muta estado de lease
* No puede ser ignorado por ningún otro componente
* Aplica garantías de aislamiento de proceso
* Mantiene protección anti-replay basada en epoch

**Propósito:**
* Centralizar decisiones de autorización
* Prevenir acumulación de permiso distribuido
* Permitir rastreo de ejecución de nivel de auditoría
* Reforzar invariantes de seguridad

**Relacionado:** ADR-003

---

### Synapse

Una **abstracción de transporte** que vincula contexto de lease y ejecución a la comunicación de módulo.

**Papel Arquitectónico:**
* Representar conexión a módulo fuera de proceso
* Adjuntar pruebas de lease y metadatos de ejecución a todas las invocaciones
* Manejar serialización y preocupaciones de nivel de transporte
* Proporcionar superficie de invocación opaca para capas superiores

**Relación con el Core:**
* Usado por capa de orquestación para invocar módulos
* Aplica requisitos de metadatos automáticamente
* Abstrae mecanismo de transporte de lógica de negocio
* Hace límites de proceso explícitos

**Reglas de Límite:**
* Sin semántica o política de negocio
* Sin lógica de autorización (delega a autoridad de lease)
* Solo transporte y serialización
* Enriquecido con metadatos pero agnóstico de dominio

**Propósito:**
* Aplicar separación de límite de proceso
* Garantizar vinculación de lease para todas las invocaciones
* Permitir evolución de transporte sin cambios de dominio
* Hacer rastreabilidad de ejecución automática

**Relacionado:** ADR-003

---

### Orchestrator

La **capa de coordinación de ejecución** que supervisa ciclo de vida de tarea sin incorporar política de dominio.

**Papel Arquitectónico:**
* Gestionar estados de ciclo de vida de tarea y span
* Coordinar resolución de capacidad vía registry
* Validar contexto de lease vía autoridad de lease
* Enrutar tareas a puertos de módulo apropiados
* Agregar resultados de ejecución en eventos

**Relación con el Core:**
* Se sitúa entre cognición (Cortex) y ejecución (módulos)
* Columna vertebral de ejecución supervisada, no motor de política
* Consume intención, produce eventos estructurados
* Coopera con sistema de interrupción

**Reglas de Límite:**
* Sin política de dominio (reglas de reintento, importancia, significado de negocio)
* Sin lógica de planificación (pertenece a capa de cognición)
* Sin bypass de autoridad de lease
* Coordinación pura sin toma de decisiones

**Propósito:**
* Separar supervisión de toma de decisiones
* Permitir patrones de ejecución consistentes
* Soportar observabilidad a través de emisión de eventos
* Prevenir acumulación de política en infraestructura

**Relacionado:** ADR-008, ADR-003, ADR-004

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

### ANIMA Instance

Una única ejecución en curso del motor ANIMA.

**Definición:** Una ANIMA Instance es un proceso en ejecución que ejecuta la lógica del motor ANIMA.

**Propiedades:**
* Efímera (existe solo durante la ejecución, destruida al apagarse)
* Posee ciclo de vida de ejecución y recursos
* Aloja una ANIMA Identity activa a la vez
* Identificada por un URN de Instancia único generado al inicio

**Lo que NO es:**
* No es una identidad (las identidades son persistentes, las instancias son efímeras)
* No es persistente entre reinicios
* No es lo que proporciona continuidad (la continuidad viene de Identity: Seed + Memory)

**Oración Canónica:** "Una ANIMA Instance ejecuta una ANIMA Identity derivada de un ANIMA Seed y Memorias."

**Relacionado:** [Sistema de Seeds](../architecture/seed-system.md), [Límites del Sistema](system-boundaries.md)

---

### ANIMA Identity

Una identidad materializada compuesta por un ANIMA Seed más su Memoria asociada.

**Definición:** Una ANIMA Identity es la personalidad persistente y en evolución y el estado de conocimiento que puede ser cargado en una ANIMA Instance.

**Composición:**
* **ANIMA Seed** (priors de identidad y configuración)
* **Memory** (capas episódica, semántica y narrativa que evolucionan con el tiempo)

**Propiedades:**
* Persistente entre ejecuciones de Instance
* Evoluciona con el tiempo a través de la experiencia
* Puede ser migrada entre Instances (con Seed + Memory)
* Independiente de Instance (el "quién" que trasciende runtimes)

**Lo que NO es:**
* No es un proceso en ejecución (eso es una Instance)
* No es efímera (persiste entre runtimes)
* No es clonable sin migración completa de Seed y Memorias

**Modelo de Relación:** Seed → Identity (Seed + Memory) → Instance (ejecuta Identity)

**Relacionado:** [Sistema de Seeds](../architecture/seed-system.md), [Integridad de la Memoria](../safety/memory-integrity.md)

---

## Identidad e Identificación

### URN de Instancia ANIMA

Un URN ANIMA que identifica de forma opaca y única un runtime específico del proceso ANIMA.

**También conocido como:** URN de Instancia del Núcleo (Core Instance URN)

**Propósito:**
* Rastrear y gestionar el ciclo de vida de una instancia específica
* Aplicar límites de aislamiento de instancia
* Habilitar memoria y ejecución con alcance de instancia
* Vincular leases a runtimes específicos del Núcleo

**Formato:** `urn:anima:core:instance@<version>:<instance-id>`

**Características:**
* Generado por el Núcleo en el inicio
* Único para cada invocación de runtime
* Identificador opaco (no derivado del contenido)
* Permanece estable durante la vida del runtime
* Usado en sobres de eventos para identificar la fuente del evento
* Validado en la capa de acceso a memoria

**Dónde se Usa:**
* Sobre de evento (campo `instance_urn`)
* Identidad del consumidor del lease
* Aplicación de aislamiento de memoria
* Codificación de certificado mTLS

**Relacionado:** [Especificación URN ANIMA](../specs/anima-urn.md), [Límites del Sistema](system-boundaries.md)

---

### URN del Núcleo

Un URN ANIMA que identifica exclusivamente el Núcleo ANIMA dentro del ecosistema ANIMA.

**Propósito:**
* Servir como punto de referencia estable para servicios del Núcleo
* Identificar la instalación del Núcleo a través de instancias de runtime
* Habilitar identificación Núcleo-a-Núcleo en escenarios distribuidos futuros
* Vincular módulos a instalaciones específicas del Núcleo

**Formato:** `urn:anima:core:core.identifier@<version>:<instance-id>`

**Características:**
* Establecido durante la configuración inicial del sistema Núcleo
* Permanece constante a través de diferentes instancias de runtime
* Identificador opaco (no derivado del contenido)
* Estable a través de reinicios del Núcleo
* Usado en sobres de eventos para identificar la fuente del Núcleo
* Codificado en certificados mTLS

**Dónde se Usa:**
* Sobre de evento (campo `core_urn`)
* Vinculación de módulos y establecimiento de confianza
* Identidad del Núcleo en escenarios distribuidos
* Autorización de capacidad a largo plazo

**Distinción del URN de Instancia:**
* URN del Núcleo identifica la **instalación/configuración**
* URN de Instancia identifica el **runtime específico**
* URN del Núcleo es estable a través de reinicios
* URN de Instancia cambia con cada ejecución

**Relacionado:** [Especificación URN ANIMA](../specs/anima-urn.md), [Tipos de Módulos y Leases](../architecture/module-types-and-leases.md)

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

## Seguridad y Riesgo

### Nivel de Riesgo del Sistema

El impacto potencial máximo a la integridad, seguridad, disponibilidad o límites de confianza del sistema si la operación se comporta incorrectamente o es abusada.

**Características:**
* Expresa severidad del impacto, no probabilidad
* Independiente del Nivel de Riesgo del Usuario
* Evaluado para cada capacidad
* Estable a través de cambios de política

**Propósito:**
* Informar mecanismos de consentimiento
* Guiar decisiones de sandboxing
* Apoyar la aplicación de políticas
* Habilitar clasificaciones de auditoría

**Relacionado:** [Modelo de Seguridad](../safety/safety-model.md), [Límites del Sistema](system-boundaries.md)

---

### Nivel de Riesgo del Usuario

El daño potencial máximo a la privacidad, finanzas, reputación, autonomía, salud (mental o física) o resultados irreversibles del usuario si la operación se ejecuta incorrectamente o sin comprensión completa.

**Características:**
* Expresa severidad del impacto, no probabilidad
* Independiente del Nivel de Riesgo del Sistema
* Evaluado para cada capacidad
* Estable a través de cambios de política

**Propósito:**
* Informar mecanismos de consentimiento
* Guiar requisitos de confirmación
* Apoyar la aplicación de políticas
* Habilitar clasificaciones de auditoría

**Relacionado:** [Modelo de Seguridad](../safety/safety-model.md), [Límites del Sistema](system-boundaries.md)

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
