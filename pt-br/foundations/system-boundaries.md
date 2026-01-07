# Limites do Sistema

**Documentação Relacionada:**
* [Carta do Projeto](project-charter.md) - Valores centrais e não-objetivos
* [Modelo de Segurança](safety-model.md) - Arquitetura de segurança e proteção
* [Modelo de Ameaças](threat-model.md) - Análise e mitigação de ameaças
* [Visão Geral da Arquitetura](architecture/anima-architecture.md) - Arquitetura completa do sistema

---

## Propósito

Este documento define **o que ANIMA pode e não pode fazer** por design.

Estes limites são **impostos pela arquitetura**, não por convenção. Violações são tratadas como bugs de segurança, não requisições de funcionalidades.

---

## O que o Motor NUNCA Pode Fazer

Estas restrições são **invariantes arquiteturais** e não podem ser contornadas.

### 1. Directly Perform Side Effects

**Boundary:** The Core (Cognitive Kernel) never touches the world directly.

**Enforcement:**
* Core produces **intent**, not effects
* All external I/O delegated to out-of-process Modules
* Modules require valid leases to execute
* No third-party code loaded into Core process

**Why:**
* Maintains clean hexagonal architecture
* Enables deterministic testing and replay
* Provides audit points for all effects
* Prevents accidental or malicious direct execution

**Related:** [Cognitive Kernel](architecture/cognitive-kernel.md), [Adapter-Actuator Split](architecture/adapter-actuator-split.md)

---

### 2. Modify Its Own Code or Seed

**Boundary:** ANIMA cannot rewrite itself or change its identity configuration.

**Enforcement:**
* Engine code is stable and version-controlled
* Seeds are loaded once at startup and immutable during runtime
* No dynamic code generation or plugin loading into Core
* Memory evolution is separate from code/seed modification

**Why:**
* Prevents runaway self-modification
* Maintains architectural stability
* Ensures predictable behavior
* Separates identity evolution from code mutation

**Related:** [Seed System](architecture/seed-system.md), [Carta do Projeto](project-charter.md)

---

### 3. Access the Internet Without Explicit Capability

**Boundary:** Network access is opt-in, never default.

**Enforcement:**
* Core has no network access by default
* Internet access requires explicit capability
* Capability must be in Seed allow-list
* Capability must have active, valid lease
* Network modules run out-of-process

**Why:**
* Prevents unauthorized data exfiltration
* User controls what ANIMA can access
* Audit all network activity
* Default to privacy and isolation

**Related:** [Module Types and Leases](architecture/module-types-and-leases.md)

---

### 4. Share Memory Between Instances

**Boundary:** Each ANIMA instance has isolated, instance-local memory.

**Enforcement:**
* Memory is scoped to execution_id and instance
* No shared databases or memory stores across instances
* No cross-instance querying or learning
* Instance URN enforced at memory access layer

**Why:**
* Prevents identity and privacy leakage
* Ensures user data ownership
* Enables truly private instances
* Supports commercial model (private instances vs. ANIMA Prime)

**Related:** [Seed System](architecture/seed-system.md), [Memory Integrity](memory-integrity.md)

---

### 5. Bypass Permission Checks

**Boundary:** All capabilities are permission-gated, always.

**Enforcement:**
* Security layer wraps input before Core
* Security validates intent before execution
* Capability execution requires:
  * Valid lease with current epoch
  * Seed allow-list inclusion
  * User permission (if required)
  * Human confirmation (if dangerous)
* No silent privilege escalation

**Why:**
* Enforces principle of least privilege
* Prevents unauthorized actions
* Maintains user trust and control
* Provides audit trail

**Related:** [Modelo de Segurança](safety-model.md), [Module Types and Leases](architecture/module-types-and-leases.md)

---

### 6. Execute Without a Valid Lease

**Boundary:** No lease, no execution (Zero-Lease State).

**Enforcement:**
* All Module execution requires Core-issued cryptographic lease
* Leases contain:
  * Lease ID and epoch
  * Permitted capability methods (scope)
  * Expiration time
  * Consumer/provider identities
* Stale or missing leases → execution refused
* Requests carry cryptographic proof bound to lease ID, epoch, nonce

**Why:**
* Prevents replay attacks
* Enforces temporal execution boundaries
* Enables revocation and scope changes
* Maintains security under partial failure

**Related:** [Module Types and Leases](architecture/module-types-and-leases.md)

---

### 7. Load Third-Party Code into Core

**Boundary:** Core process runs only trusted, engine-owned code.

**Enforcement:**
* All Modules run out-of-process
* Communication over gRPC with mTLS only
* No dynamic imports, plugins, or Python execution from modules
* Adapters and Actuators in module-owned processes
* Capability contracts are declarative JSON Schemas, not executable code

**Why:**
* Eliminates code injection risk
* Maintains architectural purity
* Enables security audits of Core
* Prevents supply chain attacks via modules

**Related:** [Adapter-Actuator Split](architecture/adapter-actuator-split.md)

---

### 8. Violate Hexagonal Architecture Boundaries

**Boundary:** Domains never import infrastructure.

**Enforcement:**
* Domains define ports (interfaces)
* Infrastructure implements adapters
* Runtime is the only layer allowed to wire everything
* Code reviews reject domain-to-infra imports
* No infrastructure implementations in domain code

**Why:**
* Maintains clean architecture
* Enables infrastructure replacement
* Simplifies testing (mock ports)
* Prevents tight coupling

**Related:** [Domain and Infrastructure](architecture/domain-and-infrastructure.md)

---

### 9. Create or Modify Events After Emission

**Boundary:** Events are immutable once emitted.

**Enforcement:**
* Events written to append-only streams
* No update or delete operations on events
* Event files are execution-partitioned
* Tooling treats events as source of truth

**Why:**
* Ensures audit integrity
* Enables deterministic replay
* Prevents tampering with history
* Supports forensics and debugging

**Related:** [Event Architecture](architecture/event-architecture.md)

---

### 10. Access Full Memory Without Constraints

**Boundary:** Memory access is mediated and scoped.

**Enforcement:**
* Cortex receives controlled memory slices, not full access
* Arcuate operates with restricted or empty memory
* Memory queries filtered by relevance and permissions
* No raw database access from Core reasoning

**Why:**
* Prevents hallucination from overwhelming context
* Maintains reasoning focus
* Enforces information boundaries
* Supports privacy (e.g., forgotten data)

**Related:** [AI Model Topology](architecture/ai-model-topology.md), [Memory Integrity](memory-integrity.md)

---

## What MUST ALWAYS Require Confirmation

These actions require **explicit human confirmation** before execution.

### 1. Accessing Sensitive User Data

**Examples:**
* Reading authentication credentials
* Accessing financial information
* Viewing private messages or files

**Confirmation Mechanism:**
* Capability flagged as `requires_confirmation: true`
* User prompted before execution
* Confirmation tracked in audit log

---

### 2. Executing High-Risk Capabilities

**Examples:**
* Financial transactions
* Physical robot movements
* System-level operations
* External API calls with side effects

**Confirmation Mechanism:**
* Risk level declared in capability contract
* Multi-channel confirmation (e.g., voice + GUI)
* Explicit intent restatement required

---

### 3. Handling Destructive Commands

**Examples:**
* Deleting files or data
* Shutting down systems
* Irreversible state changes

**Confirmation Mechanism:**
* Destructive capabilities marked explicitly
* Confirmation includes impact statement
* Optional undo/rollback path presented

---

### 4. Replacing Data

**Examples:**
* Overwriting files
* Updating records
* Modifying configurations

**Confirmation Mechanism:**
* Diff or preview shown to user
* User confirms replacement explicitly
* Previous state preserved for rollback (if feasible)

---

### 5. Non-Read-Only Irreversible Actions

**Examples:**
* Sending messages to external systems
* Publishing content
* Making reservations or commitments

**Confirmation Mechanism:**
* Clear statement of permanence
* User approves final payload
* Confirmation logged with intent

---

## What is Delegated to Modules

Modules are the **only** place where external effects occur.

### 1. All External I/O Operations

**Delegated:**
* File system read/write
* Network communication
* Database queries
* Hardware interactions

**Enforcement:**
* Core has no direct I/O capability
* Modules run out-of-process
* I/O occurs only in Actuators (not Adapters)
* Actuators operate under valid leases

---

### 2. API Calls

**Delegated:**
* REST API requests
* GraphQL queries
* gRPC calls (beyond Core-Module communication)
* WebSocket connections

**Enforcement:**
* Core produces intent with API parameters
* Adapter validates schema
* Actuator executes API call
* Events emitted for observability

---

### 3. Hardware Interactions

**Delegated:**
* Robot control
* Sensor reading
* Audio/video capture and playback
* Device state changes

**Enforcement:**
* Hardware capabilities declared in contracts
* Lease scope restricts hardware access
* Interruption policies enforce safe stops
* Task Recovery Grace Period handles in-flight operations

---

### 4. Intaking User Commands and Information

**Delegated:**
* Input capture (text, voice, sensors)
* Platform event monitoring (Discord, CLI, etc.)
* Signal processing (e.g., speech-to-text)

**Enforcement:**
* Modules capture raw input
* Adapters transform to structured events (`input.nl`, `input.system`, `input.semantic`)
* Core consumes events, never raw input

---

### 5. Executing Capability Commands

**Delegated:**
* Translating intent to platform-specific commands
* Invoking external services
* Producing observable effects

**Enforcement:**
* Adapter validates payload against capability schema
* Actuator executes under lease
* Events track execution lifecycle (invoked, completed, interrupted)

---

## Security Boundaries

### Authentication Boundary

**Where:** Input to Core

**Enforcement:**
* Security layer wraps input events
* User identity verified before Core processing
* Role-based access control applied

---

### Authorization Boundary

**Where:** Intent to Execution

**Enforcement:**
* Security validates intent against:
  * Seed capability allow-list
  * User permissions
  * Lease scope
* Dangerous actions gated by confirmation

---

### Process Boundary

**Where:** Core vs. Modules

**Enforcement:**
* gRPC over mTLS only
* Cryptographic lease enforcement
* No shared memory or direct function calls
* Module types enforce lifecycle constraints

---

### Memory Boundary

**Where:** Instance vs. Instance

**Enforcement:**
* Instance-scoped data stores
* No cross-instance queries
* Instance URN validated on all memory operations

---

### Observability Boundary

**Where:** Execution Partitioning

**Enforcement:**
* All events scoped to execution_id
* No cross-execution event mixing
* Event files isolated by execution

---

## Failure Modes & Boundaries

### Fail Closed on Uncertainty

If lease validity, permissions, or safety cannot be determined → **refuse execution**.

**Examples:**
* Stale lease epoch → reject
* Ambiguous permission → deny
* Network partition during validation → abort
* Uncertain scope → fail closed

---

### Graceful Degradation on Module Failure

If a module fails, Core remains operational.

**Enforcement:**
* Core does not crash if module crashes
* Failed capability returns error intent
* Alternative capabilities may be attempted (if Seed allows)
* Observability events track failure

---

### Interrupt on Emergency

Emergency interruptions bypass normal flow but are still:

* Classified and validated
* Logged as events
* Subject to role-based permissions
* Auditable

---

## Summary

ANIMA's boundaries are **explicit, enforced, and observable**.

> **What the engine cannot do is as important as what it can do.**
>
> **These boundaries enable trust, safety, and long-term viability.**
>
> **Violations are architectural bugs, not feature requests.**

---

## Litmus Test

Ask yourself:

* **Can the Core directly touch the world?** → No
* **Can instances share memory?** → No
* **Can modules execute without leases?** → No
* **Can the engine modify itself?** → No
* **Can capabilities bypass permissions?** → No
* **Can third-party code run in Core?** → No
* **Can events be modified after emission?** → No
* **Can dangerous actions occur without confirmation?** → No

If any answer is "yes," the boundary is violated.
