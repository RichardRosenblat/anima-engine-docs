# Topología de Modelos de IA — Cortex y Arcuate

**ADR Relacionado:** [ADR-009: Topología de Modelo de IA Configurable con Cortex Obligatorio y Arcuate Opcional (NLP)](../adr/ADR-009.md)

---

## Visión General

ANIMA adopts a **two-role AI model topology**:

1. **Cortex** — mandatory, responsible for cognition
2. **Arcuate** — optional, responsible for natural language processing

This separation ensures that:

* Cognition is always present and consistent
* Language processing can be configured or delegated
* Resource constraints don't dictate architectural compromise
* Safety and memory boundaries remain enforceable

---

## Cortex (Obligatorio)

The **Cortex** is the cognitive model wrapper and is a required component of the ANIMA Core.

### Responsabilidades

The Cortex is responsible for:

* Plan generation and refinement
* Task expansion and contingency modeling
* Semantic spine reasoning
* Capability-aware decision making
* Enforcing seed-defined behavioral constraints
* Reasoning over controlled memory slices

Without a Cortex, ANIMA cannot function as a cognitive system.

---

## Arcuate (Opcional)

**Arcuate** is the natural language processing model wrapper.

### Responsabilidades

Arcuate MAY be provided by the Core to handle:

* Natural language parsing
* Semantic frame extraction
* Natural language generation
* Linguistic normalization and realization

### Restricciones

Arcuate MUST NOT:

* Perform autonomous planning
* Generate or mutate tasks
* Access episodic or narrative memory
* Override Cortex decisions

---

## NLP como una Responsabilidad Configurable

NLP processing in ANIMA can exist in two forms:

### 1. Core-level Arcuate

* NLP is centralized
* Ensures consistency in language handling
* Reduces duplication across modules
* Subject to strict memory and context isolation

### 2. Module-level NLP

* Modules implement their own NLP capabilities
* Core Arcuate slot is not used
* Modules may use lightweight or specialized NLP tools
* Core remains language-agnostic beyond cognition

This represents a deliberate shift from **NLP as a module-only concern** to **NLP as a configurable Core responsibility**.

---

## Arquitectura Basada en Interfaces

The Core defines stable interfaces, such as:

* `ICognitive` → implemented by Cortex
* `INLP` → implemented by Arcuate or by modules

The Core and modules depend only on interfaces, never on concrete models.

This ensures:

* Model agnosticism
* Hot-swappable implementations
* Safe delegation of responsibilities

---

## Configuraciones Soportadas

### 1. Single Model, Dual Role (Cortex + Arcuate)

* One AI model implements both interfaces
* Operates in explicitly declared modes
* Cortex mode has access to controlled memory slices
* Arcuate mode operates with restricted or empty memory
* Ideal for low-resource environments

---

### 2. Dual Model Core (Dedicated Cortex + Dedicated Arcuate)

* Separate models for cognition and NLP
* Allows specialization and parallelism
* Suitable for high-resource deployments

---

### 3. Cortex-Only Core

* Cortex is present and mandatory
* Arcuate slot is empty
* NLP is handled entirely by modules
* Core provides no centralized language processing

---

## Restricciones e Invariantes

The following rules are architectural invariants:

* Cortex MUST always be present
* Arcuate is optional and replaceable
* Modules MUST NOT access Cortex directly
* All reasoning access is mediated by the Core
* NLP calls MUST NOT implicitly access memory
* Cognitive calls receive only validated memory slices
* Language understanding does not imply cognitive authority

These constraints preserve safety, clarity of responsibility, and identity integrity.

---

## Rol del Arcuate en el Procesamiento de Entrada

The Core's Arcuate is a translator (NLP), not a controller.

### Responsabilidades

* Consume `input.nl` events
* Produce `input.semantic` events

### Restricciones

* Optional and swappable
* MUST NOT access Core memory directly
* MUST NOT perform planning
* Semantic payloads MUST include confidence and provenance

Modules may produce `input.semantic` directly if they embed NLP; the Core remains agnostic to the origin of meaning.

See [Event Architecture](event-architecture.md) for details on event types.

---

## Beneficios

* Guarantees a stable cognitive core in all deployments
* Allows NLP to scale independently of cognition
* Supports minimal, modular, and high-performance configurations
* Prevents memory leakage through language processing
* Keeps modules lightweight and optional
* Clarifies responsibility boundaries between reasoning and language

---

## Justificación

This decision formalizes a critical insight:

> **Cognition is essential. Language is a capability.**

By making Cortex mandatory and Arcuate optional:

* ANIMA remains fundamentally a thinking system
* Language becomes an attachable interface, not a defining trait
* Resource constraints no longer dictate architectural compromise
* Safety and memory boundaries remain enforceable by design

---

## Resumen

> **The Cortex thinks. The Arcuate translates.**

> **Cortex is mandatory. Arcuate is optional.**

> **Language is a capability, not a core requirement.**
