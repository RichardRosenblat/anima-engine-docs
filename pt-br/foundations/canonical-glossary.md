# Glossário Canônico

**Documentação Relacionada:**
* [Visão Geral da Arquitetura](architecture/anima-architecture.md) - Arquitetura completa do sistema
* [Sistema de Seeds](architecture/seed-system.md) - Inicialização de identidade
* [Tipos de Módulos e Leases](architecture/module-types-and-leases.md) - Ciclo de vida de módulos
* [Arquitetura de Eventos](architecture/event-architecture.md) - Observabilidade e entrada
* [Kernel Cognitivo](architecture/cognitive-kernel.md) - Core como supervisor

---

## Propósito

Estas definições são **canônicas** e **nunca devem desviar**.

Toda documentação, código e discussões da ANIMA devem usar exatamente estas definições. Ambiguidade na terminologia leva a confusão arquitetural e erros de implementação.

---

## Componentes do Sistema

### Motor

A totalidade do sistema ANIMA.

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
* Agnóstico de identidade
* Modular e extensível
* Isolado de processo dos módulos
* Baseado em raciocínio por eventos

**Relacionado:** [Visão Geral da Arquitetura](architecture/anima-architecture.md)

---

### Core (Kernel Cognitivo)

O sistema de raciocínio e supervisão dentro do motor.

**Não** é um loop simples — o Core opera como um **kernel cognitivo** que:

* Ingere eventos estruturados (nunca entrada bruta)
* Consulta memória com fatias controladas
* Aplica restrições de Seed e políticas comportamentais
* Raciocina via Cortex (modelo cognitivo obrigatório)
* Opcionalmente processa linguagem via Arcuate (modelo NLP opcional)
* Gerencia tarefas concorrentes com ciclo de vida explícito
* Roteia e classifica interrupções
* Produz **grafos de intenção**, não efeitos diretos
* Supervisiona execução através de módulos controlados por lease

**Responsabilidades do Kernel:**
* Gerenciamento de ciclo de vida de tarefas (spawn, run, pause, cancel, complete)
* Roteamento e arbitragem de interrupções
* Despacho e priorização de eventos
* Controle de execução ciente de capacidades
* Observabilidade e registro de auditoria

**Regras do Core:**
* Nunca toca o mundo diretamente
* Nunca bloqueia em execução de tarefas
* Nunca carrega código de terceiros
* Produz intenção, não efeitos
* Supervisiona trabalho, não o executa

**Relacionado:** [Kernel Cognitivo](architecture/cognitive-kernel.md)

---

### Seed

Um **artefato de configuração estático e declarativo** carregado na inicialização.

**Contém:**
* Parâmetros de personalidade (tom, verbosidade, expressividade emocional)
* Políticas comportamentais (tolerância a risco, tratamento de incerteza)
* Listas de allow/deny de capacidades
* Estilo de interação padrão
* Auto-conceito inicial e enquadramento narrativo
* Políticas de decaimento e promoção de memória

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

**Contém:**
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

**Relacionado:** [Visão Geral da Arquitetura](architecture/anima-architecture.md)

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

**Relacionado:** [Tipos de Módulos e Leases](architecture/module-types-and-leases.md), [Adapter-Actuator Split](architecture/adapter-actuator-split.md)

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

**Contém:**
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

**Relacionado:** [Visão Geral da Arquitetura](architecture/anima-architecture.md)

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

**Contém:**
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

**Relacionado:** [Tipos de Módulos e Leases](architecture/module-types-and-leases.md)

---

## Componentes de Roteamento e Execução do Core

### Gateway

Um **limite de roteamento do Core** que possui uma implementação de porta/cliente e expõe um único ponto de entrada.

**Ponto de Entrada:**
* `send(operation) -> result`

**Responsabilidades:**
* Aceitar objetos de operação
* Despachar por tipo de operação
* Aplicar restrições de limite (tipagem, operações permitidas, regras "somente tradutor", etc.)
* Encaminhar para uma implementação de porta/cliente injetada

**Não-responsabilidades:**
* Sem política de domínio (sem "devemos fazer X?")
* Sem I/O de infraestrutura

**Características:**
* Limite de roteamento sem lógica de negócio
* Apoiado por implementações de porta/cliente (ex: CortexPort, MemoryPort, ArcuateClient)
* Preserva limites de arquitetura hexagonal

**Relacionado:** ADR-012

---

### CortexGateway / MemoryGateway / ArcuateGateway

**Implementações específicas de gateway** para diferentes subsistemas do Core.

**CortexGateway:**
* Roteia operações cognitivas para a porta/cliente Cortex obrigatória
* Gerencia solicitações de raciocínio cognitivo

**MemoryGateway:**
* Roteia operações de memória para portas de memória (repositórios, serviços de memória)
* Sem política de memória incorporada

**ArcuateGateway:**
* Roteia operações de tradução NLP para Arcuate (se presente)
* DEVE permanecer somente tradutor (sem planejamento ou tomada de decisão)

**Relacionado:** ADR-012, ADR-009

---

### Operation Objects

**Objetos de operação tipados** despachados por Gateways representando "qual comportamento" sem codificar lógica de roteamento.

**Categorias de Exemplo:**
* `ProcessEvent`
* `EvaluateIntent`
* `Reflect`
* `TranslateInputNlToSemantic`
* `RealizeIntentToNl`

**Características:**
* Type-safe
* Descritores de comportamento
* Agnóstico de roteamento
* Específico de domínio

**Relacionado:** ADR-012

---

### ModuleRegistry

A **autoridade de descoberta de capacidades e conexão**.

**Autoridade Para:**
* Descoberta de módulos disponíveis
* Rastreamento de atestado e handshake
* Rastreamento de vivacidade
* Indexação de capacidades
* Retornar handles de endpoint ativos para capacidades

**Não Autoridade Para:**
* Permissões (isso é território de lease + política de segurança)
* Concessão de lease
* Promoção/rebaixamento de escopo

**Características:**
* Apenas descoberta de capacidades
* Sem decisões de autorização
* Rastreia saúde e conectividade de módulos
* Fornece mapeamento capacidade-para-endpoint

**Relacionado:** ADR-012

---

### LeaseManager

Um **componente dedicado de propriedade do Core** responsável por toda autoridade de lease.

**Responsabilidades:**
* Emissão de leases
* Renovação de leases
* Revogação de leases
* Gerenciamento de epochs
* Aplicação da semântica "sem lease, sem execução"

**Regras de Autoridade de Lease:**
* Único componente permitido a mutar estado de lease
* Propriedade e controle do Core
* Explícito e auditável
* Reforça isolamento de processo

**Características:**
* Fonte única de verdade para estado de lease
* Consciente de epoch
* Componente crítico de segurança
* Nunca delegado a módulos ou portas

**Relacionado:** ADR-012, ADR-003

---

### ModulePort / ModuleSession

Uma **conexão de transporte opaca** vinculada a um módulo específico.

**Vincula:**
* `LeaseMeta` (lease_id, epoch, proofs)
* `ExecMeta` (execution_id, trace_id, span_id, thread_id)

**Superfície de Invocação:**
* `invoke(capability_urn, payload, meta) -> result`

**Responsabilidades:**
* Serialização/desserialização
* Anexação de metadados necessários
* Tentativas de retry para falha de transporte (se configurado)
* Surfacing de erros de forma determinística

**Não-responsabilidades:**
* Sem semântica de negócio
* Sem decisões de autorização
* Sem lógica de política (ex: "quantas tentativas porque é importante")

**Características:**
* Sessão vinculada a lease
* Transporte enriquecido com metadados
* Opaco à lógica de negócio
* Aplicação de limite de processo

**Relacionado:** ADR-012, ADR-003

---

### TaskOrchestrator

A **espinha de execução supervisionada do Kernel**.

**Responsabilidades:**
* Gerenciar ciclo de vida de tarefa/span (spawn/run/pause/cancel/complete)
* Cooperar com roteamento e políticas de interrupção
* Resolver capacidades via ModuleRegistry
* Obter/validar contexto de lease via LeaseManager antes da invocação
* Despachar tarefas para ModulePorts
* Agregar resultados e emitir eventos

**Não-responsabilidades:**
* Sem codificação de política de domínio (ex: específica de intent "retry porque tem significado de negócio")
* Sem lógica de planejamento (isso pertence à cognição/Cortex)
* Sem bypass de autoridade de lease

**Características:**
* Supervisiona execução, não executa
* Coordenação livre de política
* Invocação controlada por lease
* Gerenciamento de ciclo de vida orientado a eventos

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

**Relacionado:** [Arquitetura de Eventos](architecture/event-architecture.md)

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

**Relacionado:** [Arquitetura de Eventos](architecture/event-architecture.md)

---

## Architecture Patterns

### Package

A distributable group of modules, adapters, and capability definitions.

**Contém:**
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

**Relacionado:** [Tipos de Módulos e Leases](architecture/module-types-and-leases.md)

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

**Relacionado:** [Tipos de Módulos e Leases](architecture/module-types-and-leases.md)

---

## Summary

This glossary defines the **canonical vocabulary** of ANIMA.

> **Precision in language prevents confusion in architecture.**
>
> **These definitions are stable, documented, and enforced.**
>
> **Drift in terminology leads to drift in implementation.**
