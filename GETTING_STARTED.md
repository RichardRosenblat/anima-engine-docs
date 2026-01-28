# Getting Started with ANIMA Documentation

**Purpose:** This guide helps new readers navigate the ANIMA documentation efficiently.

ANIMA is a private, modular AI engine designed to host long-lived, evolving artificial identities under strict safety, memory, and capability constraints. This documentation explains the architecture, design decisions, and philosophical foundations of the system.

---

## Who Is This Documentation For?

This documentation serves multiple audiences:

### 1. **Architectural Reviewers**
You want to understand ANIMA's system design, architectural patterns, and design rationale.

**Start here:**
* [Vision](vision/vision.md) - Understand what ANIMA is trying to enable
* [ANIMA Architecture](architecture/anima-architecture.md) - Comprehensive system overview
* [ADRs](adr/) - Architecture Decision Records (read in order, ADR-001 through ADR-011)

### 2. **Safety & Governance Reviewers**
You need to evaluate ANIMA's safety model, threat boundaries, and governance approach.

**Start here:**
* [Safety Model](safety/safety-model.md) - Safety and security architecture
* [Threat Model](safety/threat-model.md) - Threat analysis and mitigation strategies
* [Memory Integrity](safety/memory-integrity.md) - Memory management constraints
* [Non-Goals](vision/non-goals.md) - Explicit boundaries and what ANIMA refuses to become

### 3. **Future Implementers**
You may eventually implement, integrate, or extend ANIMA components.

**Start here:**
* [Canonical Glossary](foundations/canonical-glossary.md) - Authoritative terminology (read first)
* [Seed System](architecture/seed-system.md) - How identities are initialized
* [Module Types and Leases](architecture/module-types-and-leases.md) - Module lifecycle and authorization
* [Event Architecture](architecture/event-architecture.md) - Input and observability system

### 4. **General Technical Readers**
You want a broad understanding of what ANIMA is and why it exists.

**Start here:**
* [README](README.md) - High-level introduction and FAQ
* [Vision](vision/vision.md) - Long-term goals and philosophical foundations
* [Why Not an Agent?](vision/why-not-an-agent.md) - What makes ANIMA different
* [Project Charter](vision/project-charter.md) - Core purpose and values

---

## Suggested Reading Orders

### Quick Overview (15-30 minutes)
For a rapid understanding of ANIMA's core concepts:

1. [README](README.md) - Introduction and high-level overview
2. [Vision](vision/vision.md) - Purpose and philosophy
3. [ANIMA Architecture](architecture/anima-architecture.md) - System design overview
4. [Non-Goals](vision/non-goals.md) - What ANIMA is not

### Comprehensive Review (2-4 hours)
For a thorough understanding of the entire system:

1. **Foundations** (30 minutes)
   * [Vision](vision/vision.md)
   * [Project Charter](vision/project-charter.md)
   * [Canonical Glossary](foundations/canonical-glossary.md)
   * [System Boundaries](foundations/system-boundaries.md)

2. **Architecture** (1-2 hours)
   * [ANIMA Architecture](architecture/anima-architecture.md)
   * [Cognitive Kernel](architecture/cognitive-kernel.md)
   * [Seed System](architecture/seed-system.md)
   * [Module Types and Leases](architecture/module-types-and-leases.md)
   * [Event Architecture](architecture/event-architecture.md)

3. **Safety & Governance** (30-45 minutes)
   * [Safety Model](safety/safety-model.md)
   * [Threat Model](safety/threat-model.md)
   * [Memory Integrity](safety/memory-integrity.md)
   * [Licensing Model](governance/licensing-model.md)

4. **Design Decisions** (45-60 minutes)
   * Read ADRs in order: [ADR-001](adr/ADR-001.md) through [ADR-011](adr/ADR-011.md)

### Deep Architectural Dive (4-6 hours)
For implementers and architectural reviewers who need complete understanding:

1. **Canonical Definitions First**
   * [Canonical Glossary](foundations/canonical-glossary.md) - Read this before anything else
   * [System Boundaries](foundations/system-boundaries.md)

2. **Philosophical Grounding**
   * [Vision](vision/vision.md)
   * [Why Not an Agent?](vision/why-not-an-agent.md)
   * [Reinvention Rationale](foundations/reinvention-rationale.md)
   * [Non-Goals](vision/non-goals.md)

3. **Complete ADR Set** (in order)
   * [ADR-001: Engine/Identity Split & Seed System](adr/ADR-001.md)
   * [ADR-002: Hexagonal Architecture](adr/ADR-002.md)
   * [ADR-003: Memory Layers & Integrity](adr/ADR-003.md)
   * [ADR-004: Declarative Capability System](adr/ADR-004.md)
   * [ADR-005: Interruption & Preemption Model](adr/ADR-005.md)
   * [ADR-006: Domain Dependency & Sharing Rules](adr/ADR-006.md)
   * [ADR-007: Infrastructure Layer & Adapter Rules](adr/ADR-007.md)
   * [ADR-008: ANIMA Core Behaves as a Cognitive Kernel](adr/ADR-008.md)
   * [ADR-009: Configurable AI Model Topology](adr/ADR-009.md)
   * [ADR-010: Adapter–Actuator Split](adr/ADR-010.md)
   * [ADR-011: Event-Based Input Architecture](adr/ADR-011.md)

4. **Complete Architecture Documentation**
   * [ANIMA Architecture](architecture/anima-architecture.md)
   * [Cognitive Kernel](architecture/cognitive-kernel.md)
   * [Seed System](architecture/seed-system.md)
   * [Module Types and Leases](architecture/module-types-and-leases.md)
   * [Event Architecture](architecture/event-architecture.md)
   * [Domain and Infrastructure](architecture/domain-and-infrastructure.md)
   * [AI Model Topology](architecture/ai-model-topology.md)
   * [Adapter-Actuator Split](architecture/adapter-actuator-split.md)
   * [Explicit Semantics and Model Topology](architecture/explicit-semantics-and-model-topology.md)
   * [Directory Overview](architecture/directory-overview.md)

5. **Complete Safety & Governance Review**
   * [Safety Model](safety/safety-model.md)
   * [Threat Model](safety/threat-model.md)
   * [Memory Integrity](safety/memory-integrity.md)
   * [Licensing Model](governance/licensing-model.md)
   * [Project Viability](governance/project-viability.md)

6. **Specifications**
   * [ANIMA URN Specification](specs/anima-urn.md)
   * [JSON Reference System (JSRS)](specs/json_reference_system.md)

---

## Key Concepts to Understand

Before diving deep, familiarize yourself with these fundamental concepts:

### Engine ≠ Identity
ANIMA separates the reasoning engine from the identity. The engine is identity-agnostic; personalities are defined by **Seeds** (declarative configuration files).

### Safety Over Capability
ANIMA treats capability as risk. Every action requires explicit authorization, can be inspected, and leaves an audit trail. Safety is architectural, not prompt-based.

### Private by Design
Each ANIMA instance owns its memory and evolves independently. There is no shared memory pool or cross-instance learning.

### Continuity Over Recall
ANIMA prioritizes consistent identity and narrative continuity over perfect photographic memory. Memory has intentional decay policies.

### Hexagonal Architecture
ANIMA uses ports-and-adapters architecture. The core has no knowledge of platforms, personalities, or embodiment. All I/O happens through modules.

---

## Common Questions

For frequently asked questions, see [FAQ.md](FAQ.md).

For questions about what ANIMA is not designed to do, see [Non-Goals](vision/non-goals.md).

---

## Navigation Tips

* **Canonical Glossary** ([foundations/canonical-glossary.md](foundations/canonical-glossary.md)) - Use this as your reference for all terminology. Definitions are authoritative and must never drift.

* **ADRs are numbered** - Read them in order (001-011) to understand the evolution of architectural thinking.

* **Cross-references are intentional** - Documents link to each other deliberately. Follow links to understand relationships.

* **"Related Documentation" sections** - Most documents include these at the top. Use them for context.

---

## What ANIMA Is Not

To avoid misunderstanding ANIMA's purpose, understand what it explicitly is **not**:

* Not a chatbot or conversational AI wrapper
* Not an autonomous general-purpose agent
* Not a single fixed personality
* Not a shared-memory or hive-mind system
* Not a prompt engineering framework
* Not a RAG (Retrieval-Augmented Generation) system

See [Why Not an Agent?](vision/why-not-an-agent.md) for detailed explanations.

---

## Documentation Status

ANIMA is currently in active development. The engine implementation lives in a separate, private repository (`engine-core`).

This documentation repository exists to:
* Explain the architecture transparently
* Document design decisions publicly
* Make the vision and safety model reviewable before release

For current development status, see [Roadmap](roadmaps/roadmap.md).

---

## Contributing to Documentation

For guidance on proposing documentation changes, see [DOCUMENTATION_MAINTENANCE.md](DOCUMENTATION_MAINTENANCE.md).

---

## Additional Resources

* [Announcements](announcements/) - Project announcements and updates
* [Roadmap](roadmaps/roadmap.md) - Development roadmap
* [Project Viability](governance/project-viability.md) - Long-term sustainability assessment

---

**Last Updated:** January 28, 2026
