# Domain and Infrastructure — Hexagonal Architecture in ANIMA

**Related ADRs:**
* [ADR-002: Self-Validating Schemas with Internal-Only Constraints](../adr/ADR-002.md)
* [ADR-006: Domain Dependency & Sharing Rules](../adr/ADR-006.md)
* [ADR-007: Infrastructure Layer & Adapter Rules](../adr/ADR-007.md)

---

## Overview

ANIMA follows **Hexagonal Architecture** combined with **Domain-Driven Design (DDD)**. This document formalizes the strict rules governing:

* Domain-to-domain dependencies
* What may be shared
* How infrastructure relates to domains
* Runtime composition

---

## Core Principle

> **Domains must never talk directly to the outside world.**

The outside world includes:

* Databases (Postgres, SQLite, Redis)
* File systems
* Network services
* Message brokers
* OS time, environment, entropy

Domains express their needs through **interfaces (ports)**. Infrastructure fulfills those needs through **adapters**.

---

## Dependency Direction (Non-Negotiable)

```
infra  ─────▶  domains
runtime ────▶  infra + domains

domains ─╳──▶  infra
```

Rules:

* Domains **define ports** (interfaces / abstractions)
* Infra **implements ports**
* Domains **never import infra**
* Runtime is the only layer allowed to import and wire everything

---

## Domain Layer

### What Domains Contain

Domains contain:

* Business logic and invariants
* Domain models and value objects
* Domain services
* Ports (interfaces for external dependencies)
* Domain-specific exceptions

### What Domains Must NOT Contain

* IO operations
* Database queries
* Network calls
* Infrastructure implementations
* Framework dependencies

---

## Approved Cross-Domain Dependency Forms

Domains **do not share code freely**. Domains may only depend on other domains through **explicit public contracts**.

A domain may expose the following as part of its **public API**:

### 1. Free Functions (Stateless Domain Logic)

Used for:

* Deterministic rules
* Parsing, validation, generation
* Value transformations

**Characteristics:**

* Pure or near-pure
* No lifecycle
* No external IO

**Example (URN domain):**

```python
## domains/urn/generate.py

def generate_urn() -> URN:
    ...
```

```python
## domains/core/process.py
from domains.urn import generate_urn

class CoreProcess:
    def __init__(self) -> None:
        self.urn = generate_urn()
```

✔ Allowed

---

### 2. Primitive Type Wrappers / Value Objects

Used to encode **semantic meaning** and prevent misuse of primitives.

**Examples:**

* `URN`
* `ExecutionId`
* `TraceId`
* `ParsedURN`

```python
## domains/urn/types.py
from typing import NewType

URN = NewType("URN", str)
```

✔ Allowed

Value objects are safe to import directly across domains.

---

### 3. Base Classes (Conceptually Abstract)

Base classes define **invariants and contracts**.

#### 3.1 Behavioral Base Classes → MUST be Abstract

Used when:

* Other domains specialize behavior
* The class is not meant to be instantiated

```python
from abc import ABC, abstractmethod

class BaseProcess(ABC):
    @abstractmethod
    def run(self) -> None:
        ...
```

✔ Allowed

---

#### 3.2 Exception Base Classes → MAY be Concrete

Exceptions are **values**, not behaviors.

They:

* Must be instantiable
* Encode domain semantics
* Are explicitly designed for inheritance

```python
class DomainError(Exception):
    pass

class URNError(DomainError):
    pass
```

✔ Allowed

This is an intentional exception to abstraction rules.

---

### 4. Interfaces / Protocols

Used when:

* Behavior must be supplied externally
* Multiple implementations are possible
* Runtime wiring is required

```python
from typing import Protocol

class Clock(Protocol):
    def now(self) -> datetime: ...
```

```python
class CoreProcess:
    def __init__(self, clock: Clock):
        self.clock = clock
```

✔ Allowed

Domains depend on **interfaces**, not implementations.

---

## Cross-Domain Error Translation (Mandatory)

When a domain depends on another domain and invokes its public API, **domain-specific errors originating from the dependency MUST NOT leak unmodified**.

Instead:

* The consuming domain **MUST catch** dependency domain errors
* The consuming domain **MUST raise its own domain-specific error**
* The raised error **MUST inherit from the original error type**
* The raised error **MUST preserve the original exception as the cause**

Example:

```python
# domains/urn/errors.py
class URNError(DomainError):
    pass
```

```python
# domains/order/errors.py
from domains.urn.errors import URNError

class OrderURNResolutionError(URNError):
    pass
```

```python
# domains/order/service.py
from domains.urn import generate_urn
from domains.urn.errors import URNError
from domains.order.errors import OrderURNResolutionError

def create_order() -> None:
    try:
        urn = generate_urn()
    except URNError as exc:
        raise OrderURNResolutionError(
            "Failed to generate URN during order creation"
        ) from exc
```

✔ Allowed
✔ Required
✔ Explicit

---

## Self-Validating Schemas

Schemas **MAY** include self-validation logic **only if**:

1. The validation:
   - Uses **only the data contained within the schema instance**
   - Relies exclusively on pure logic
2. The validation:
   - Does **not** query external state
   - Does **not** depend on other schemas' runtime instances
   - Does **not** perform IO, filesystem access, or network calls
3. The validation:
   - Is deterministic
   - Produces the same result for the same input data

### ✅ Allowed (Internal Consistency)

* Field range checks
* Enum membership validation
* Cross-field consistency within the same schema
* Structural integrity rules

```python
@dataclass(frozen=True)
class SemanticFact:
    content: str
    confidence: float

    def validate(self) -> None:
        if not (0.0 <= self.confidence <= 1.0):
            raise ValueError("confidence must be between 0 and 1")
```

### ❌ Forbidden (External Dependency)

* Checking whether a referenced entity exists in memory
* Verifying permissions
* Looking up historical interactions
* Calling engine services or adapters

---

## Infrastructure Layer

### Why the `infra/` Folder Exists

The `infra/` folder exists to:

* Isolate all IO and external system interaction
* Implement domain-defined interfaces (ports)
* Allow replacement of technologies without touching domains
* Keep domain logic pure, testable, and stable

Infrastructure is **not** part of the domain model.

---

### Domain Ports (Interfaces)

Domains define **ports** for anything that is external or variable.

Examples:

* Repositories
* Event buses
* Clocks
* File stores
* Message dispatchers

```python
# domains/orders/ports.py
from typing import Protocol

class OrderRepository(Protocol):
    def save(self, order: Order) -> None: ...
```

Domains depend **only** on these abstractions.

---

### Infrastructure Adapters

Infrastructure adapters:

* Implement exactly one domain port
* Contain zero business logic
* Translate between domain concepts and external systems

```python
# infra/persistence/postgres/order_repository.py
from domains.orders.ports import OrderRepository

class PostgresOrderRepository(OrderRepository):
    def save(self, order: Order) -> None:
        ...
```

---

### Infra Folder Structure

Infrastructure is structured by **technology**, then by **adapter responsibility**.

```
/infra
  __main__.py
  /persistence
    /postgres
      __main__.py
      order_repository.py
      user_repository.py
      memory_repository.py

    /sqlite
      __main__.py
      order_repository.py

  /messaging
    /kafka
      __main__.py
      domain_event_bus.py

  /clock
      __main__.py
    system_clock.py
```

### Structure Rules

1. One adapter per file
2. Each adapter implements exactly one domain port
3. No domain logic in infra
4. No cross-adapter imports
5. No domain-to-infra imports

---

## Runtime Responsibilities

Runtime is the **composition root**.

Runtime:

* Selects infrastructure implementations
* Instantiates adapters
* Injects them into domains

```python
repo = PostgresOrderRepository(pool)
service = OrderService(repo)
```

Domains must never perform wiring or instantiation of infra.

---

## Forbidden Practices

❌ Importing internal modules or private helpers  
❌ Sharing mutable state across domains  
❌ Importing infrastructure implementations directly  
❌ Circular domain dependencies  
❌ Runtime wiring inside domain logic  
❌ Domains importing infra implementations  
❌ Infra defining business rules  
❌ Infra selecting implementations  

---

## Shared Layer Rules

Some abstractions do not belong to any domain.

```
/shared
  #types.py        # ExecutionId, TraceId
  #protocols.py    # Clock, Logger, EventSink
  #base.py         # Abstract base classes
```

Rules:

* Domains may depend on `shared`
* `shared` depends on nothing

---

## Benefits

* Strong architectural boundaries
* Reduced accidental coupling
* Predictable dependency graph
* Easier testing and refactoring
* Clean dependency graph
* Replaceable infrastructure
* High testability
* Long-term architectural stability

---

## Summary

> **Domains do not depend on implementations. They depend on meaning.**

> **Domains describe reality. Infrastructure connects reality to the outside world. Runtime assembles the system.**

Meaning is expressed through contracts.
