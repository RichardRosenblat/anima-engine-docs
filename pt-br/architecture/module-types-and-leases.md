# Tipos de Módulos e Autorização Baseada em Lease

**ADR Relacionado:** [ADR-003: Protocolo de Comunicação Core ↔ Módulo, Tipos de Módulos e Ciclo de Vida de Lease](../adr/ADR-003.md)

---

## Visão Geral

ANIMA's Core communicates with Modules through a strict, secure protocol based on **gRPC over mTLS** and **cryptographic leases**. This document describes the lease-based authorization model and the three module types that govern lifecycle, tenancy, and execution guarantees.

---

## Protocolo de Comunicação

### Mecanismo Único de Comunicação

All communication between ANIMA Core and Modules MUST occur over:

> **gRPC over mutually authenticated TLS (mTLS) Connections**

No alternative transport, IPC mechanism, or protocol is permitted.

### Segurança de Transporte e Identidade

* All connections MUST use TLS encryption
* Plaintext communication is forbidden, including on `localhost`
* Both Core and Module MUST present verifiable certificates
* Certificates MUST encode stable identities:
  * **Core Instance URN**
  * **Module URN**

---

## Modelo de Autorização Baseado em Lease

### O que é um Lease?

A lease is understood as a:

> **Cryptographic Warrant of a Connection Between a Core and a Module, along with Allowed Actions taken through it**

A lease MUST contain:

* Connection ID (Lease ID)
* Consumer identity (Core / Core Instance URN)
* Provider identity (Module URN)
* Permitted capability methods (scope)
* Expiration time (bounded by the maximum duration declared in the Module's Capability Contract)

### Autoridade de Lease (Invariante Crítico)

> **The Core is the sole and canonical authority capable of issuing, renewing, modifying, or revoking leases.**

Modules:

* MUST NOT issue, extend, or modify leases
* MAY only acknowledge, enforce, or refuse execution based on a Core-issued lease

### Handshake e Estabelecimento de Lease

1. A secure mTLS channel is established
2. The Core initiates a handshake declaring intent and requested capabilities
3. The Module responds with **attestation only**:
   * Identity
   * Capability contract hash
   * Declared module type
   * Maximum allowed lease duration
4. The Core validates the attestation **off-wire**
5. If accepted, the Core issues a **signed Lease Grant**
6. The Module acknowledges the lease and binds it to internal state

> **A lease becomes valid only after explicit acknowledgment by the Module.**

### Épocas de Lease e Anti-Dessincronização

Every lease includes a **monotonic Lease Epoch**.

* Epochs start at `1` and are incremented on:
  * Renewal
  * Scope change
  * Revocation
* The Core is the sole epoch authority

**Every request MUST carry cryptographic proof bound to:**

* Lease ID
* Lease Epoch
* Request ID / nonce

Requests with stale or mismatched epochs MUST be rejected immediately.

This prevents:

* Replay attacks
* Silent desynchronization
* Partial revocation failures

### Promoção e Rebaixamento de Escopo de Lease

Lease scope defines the **exact set of capability methods** a Module is authorized to execute for a given Core.

> **Scope may only change through an explicit Core-issued lease update.**

Rules:

* Only the **Core** may grant, promote, or demote lease scope
* Modules MUST NOT autonomously widen scope
* Scope updates require a new lease epoch
* The updated scope fully replaces the previous scope
* The previous epoch is immediately invalid

### Estado Zero-Lease

A Module is in **Zero-Lease State** when it has no currently valid lease.

> **No Lease, No Execution**

A module with no valid lease:

* MUST reject all capability execution
* MUST NOT perform side effects
* MUST NOT process background tasks on behalf of a Core

---

## Tipos de Módulos

Every ANIMA Module MUST declare **exactly one Module Type** in its Capability Contract.

> **A Module Type is a first-class declaration of tenancy, lifecycle authority, failure semantics, and isolation guarantees.**

### Tipo I — Efêmero Private Module

**Type I Modules are:**

* **Ephemeral**
* **Single-tenant**
* **Strictly lease-bound**
* **Core-lifecycle-controlled**

They exist *only* to serve a **single Core instance for the duration of an active lease**.

#### Declarative Properties

| Property             | Value                            |
| -------------------- | -------------------------------- |
| Tenancy model        | Single Core                      |
| Lifecycle authority  | Core                             |
| Lease dependency     | Mandatory                        |
| Side effects         | Must be reversible or disallowed |
| Startup mode         | Core-issued only                 |

#### Lifecycle Semantics

* The Core is the **sole authority** allowed to start the module
* The module MUST accept leases from **exactly one Core Instance URN** defined at startup
* Zero-Lease State inevitably leads to graceful termination after a Task Recovery Grace Period
* Task Recovery Grace Periods:
  * MUST be bounded
  * MUST be non-renewable
  * MUST reject all non-lease traffic

> **A Type I module without a lease is considered dead, even if its process is currently still running.**

---

### Type II — Resident Private Module

**Type II Modules are:**

* **Resident**
* **Single-tenant**
* **Lease-gated**
* **Core-affiliated**

They are long-lived processes that serve **exactly one Core**, but whose existence is not strictly bound to lease duration.

#### Declarative Properties

| Property             | Value                      |
| -------------------- | -------------------------- |
| Tenancy model        | Single Core                |
| Lifecycle authority  | Core or Infrastructure     |
| Lease dependency     | Mandatory for execution    |
| Side effects         | Allowed within lease scope |
| Startup mode         | Core-issued or pre-started |

#### Lifecycle Semantics

* The module MUST bind permanently to a single Core URN
* Zero-Lease State places the module into **Standby Mode**
* In Standby Mode:
  * Execution is forbidden
  * Any execution is halted at the next declared interruption point
* Lease restoration and creation MUST come from the same Core
* Any in-flight task state MAY be retained only for the duration of the Task Recovery Grace Period

> **A Type II module without a lease is inert, not terminated.**

---

### Type III — Resident Shared Module

**Type III Modules are:**

* **Resident**
* **Multi-tenant**
* **Lease-partitioned**
* **Infrastructure-governed**

They are designed to safely serve **multiple independent Cores concurrently**.

> Type III is an **opt-in trust class**, not a default.

#### Declarative Properties

| Property             | Value                 |
| -------------------- | --------------------- |
| Tenancy model        | Multi-Core            |
| Lifecycle authority  | Infrastructure        |
| Lease dependency     | Mandatory per tenant  |
| Side effects         | Lease-isolated only   |
| Startup mode         | Infrastructure-issued |

#### Isolation & Behavior Semantics

* Each Core MUST be isolated by:
  * Lease ID
  * Core URN
* All behavior MUST be scoped to an active lease
* Presence or behavior of one Core MUST NOT affect another
* Task Recovery Grace Period state MUST be isolated per Lease ID and destroyed independently

> **Any cross-tenant influence is considered a security breach and will cause Module Blacklisting.**

Regular compliance checks and formal verification may be mandated for Type III modules by ANIMA teams.

#### Lifecycle Semantics

* Lease expiration affects only the owning Core
* Global Zero-Lease transitions the module into Standby Mode, similar to Type II
* Termination is controlled exclusively by infrastructure or administrators

---

## State Persistence & Task Recovery Grace Periods

ANIMA Modules must be architecturally stateless. All observable behavior must be attributable to explicit inputs, an active lease, and deterministic computation.

All modules MUST be designed with no state persistence beyond the following:

* In-memory state during active tasks
* Ephemeral storage scoped to active leases
* Initialization heavy read-only resources loaded at startup (MUST be deterministic, immutable, and identical across restarts)
* Caches MUST be:
  * scoped to a single task execution, or
  * scoped to a single active lease and destroyed on lease expiration

Persistent state storage across restarts or lease cycles is forbidden.

Task Recovery Grace Periods MUST be:

* A bounded interval during which in-flight task state may be retained solely to support rollback, retry, or error reporting for a single lease
* Non-renewable and non-extendable
* Non-accessible for new leases or tasks

---

## Resumo

> **ANIMA Modules are Ephemeral Private (Type I), Resident Private (Type II), or Resident Shared (Type III).**
> **All execution is gated by Core-issued cryptographic leases over mTLS.**
> **Without a valid lease, a module is inert or dead. There are no exceptions.**
> **Scope changes are explicit, auditable, and unidirectional.**
> **Module types define strict lifecycle and tenancy semantics.**
> **State persistence is forbidden beyond bounded Task Recovery Grace Periods.**
