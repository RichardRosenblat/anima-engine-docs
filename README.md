# ANIMA Engine Documentation

## Table of Contents

- [English](#english)
- [Português (Brasil)](#português-brasil)
- [Español (México)](#español-méxico)

---

# English

![Status](https://img.shields.io/badge/status-active%20development-blue)
![Architecture](https://img.shields.io/badge/architecture-hexagonal%20%2F%20ports--and--adapters-purple)

**ANIMA** is a private, modular AI engine designed to host long-lived, evolving artificial identities under strict safety, memory, and capability constraints.

It is not a chatbot, not a prompt wrapper, and not a single personality.

ANIMA is an **engine for growing identities**.

---

## Getting Started

**New to ANIMA?** Start here:

* **[Getting Started Guide](GETTING_STARTED.md)** - Navigate the documentation efficiently with suggested reading orders for different audiences
* **[FAQ](FAQ.md)** - Common questions and quick answers
* **[Vision](vision/vision.md)** - Understand what ANIMA is trying to enable

---

## Core Principles

* **Engine ≠ Identity**
  Reasoning and execution are strictly separated from personality and behavior.

* **Private by Design**
  Each ANIMA instance owns its memory and evolves independently.

* **Safety over Capability**
  Permissions, confirmations, and boundaries are enforced at the engine level.

* **Modularity First**
  Inputs, outputs, capabilities, and embodiment are pluggable.

* **Continuity over Recall**
  ANIMA prioritizes consistent identity and trust over perfect memory.

---

## Architecture Overview

ANIMA follows a hexagonal / ports-and-adapters architecture.

```
┌────────────────────┐
│   Input Modules    │  (text, voice, discord, sensors)
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│  ANIMA Engine   │
│                    │
│  - Reasoning       │
│  - Planning        │
│  - Task System     │
│  - Memory Layers   │
│  - Security        │
│  - Capabilities    │
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│      Adapters       │  (Intention to command conversion)
└────────────────────┘
          │
┌─────────▼──────────┐
│ Capability Modules │  (tools, streaming, robots, etc)
└────────────────────┘
```

The engine has **no knowledge of platforms, personalities, or embodiment**.

---

## The Seed System

A **Seed** defines an ANIMA identity.

A seed is **declarative configuration data**, not executable code.

Seeds define parameters, policies, and constraints that the engine interprets—they do not contain scripts, functions, or executable logic.

### A seed may define:

* Personality parameters (tone, verbosity, emotional expressiveness)
* Behavioral policies (risk tolerance, uncertainty handling)
* Capability allow/deny lists
* Default interaction style
* Initial narrative framing
* Memory decay and promotion rules

### A seed must not include:

* Learned memories
* Conversation logs
* User data
* Execution logic
* Cross-instance state

After initialization, each ANIMA instance develops **instance-local memory only**.

---

## ANIMA Prime vs User Instances

* **ANIMA Prime**
  The first canonical identity designed for the ANIMA Engine.
  Used for streaming / VTuber embodiment and as a reference implementation.
  Implemented as:

  ```
  ANIMA Engine + Prime Seed
  ```

  The Prime seed is never distributed.

* **User ANIMA Instances**
  Private instances initialized with user-selected seeds.
  Users own their instance's memory and evolution.

All instances run on the **same engine**, but remain fully isolated.

---

## Memory Model

ANIMA uses layered memory to balance realism, safety, and cost.

Memory types, promotion, and decay policies are specified in the [memory integrity document](safety/memory-integrity.md)

ANIMA tracks whether knowledge is:

* observed
* remembered
* inferred
* unknown

She never silently guesses.

---

## Capabilities

Capabilities are explicit, discoverable modules.

The engine:

* reasons about capabilities
* checks permissions
* requests confirmation when required
* executes in a controlled sandbox

Seeds may **restrict** capabilities, but never grant new execution power.

---

## Security & Permissions

* Stable user identities
* Role-based access
* Capability-level permission checks
* Human confirmation for dangerous actions
* Full audit logging

ANIMA explains *why* she can or cannot act.

---

## What ANIMA Is Not

* ❌ A general-purpose autonomous agent
* ❌ An internet-connected AI by default
* ❌ A shared-memory system
* ❌ A personality hardcoded into prompts
* ❌ A replacement for human judgment

---

## Project Status

ANIMA is under active development.

Current focus:

* Core engine correctness
* Memory integrity
* Capability boundaries
* Seed validation

This project is intentionally evolving slowly.

---

## Design Documents

Key architectural decisions are tracked as ADRs in [`/adr`](adr):

* ADR-001: Separation of ANIMA Engine and Identity via Seed System
* ADR-002: Self-Validating Schemas with Internal-Only Constraints
* ADR-003: Core ↔ Module Communication Protocol, Module Types, and Lease Lifecycle
* ADR-004: Observability, Event Logging, and Execution Traceability
* ADR-005: Interruption & Preemption Model
* ADR-006: Domain Dependency & Sharing Rules
* ADR-007: Infrastructure Layer & Adapter Rules
* ADR-008: ANIMA Core Behaves as a Cognitive Kernel with Multitasking and Interruptible Execution
* ADR-009: Configurable AI Model Topology with Mandatory Cortex and Optional Arcuate (NLP)
* ADR-010: Adapter–Actuator Split with Strict Process Boundary (No Third-Party Code in Core)
* ADR-011: Event-Based Input Architecture with Natural Language, System, and Semantic Events

### Architecture Documentation

Comprehensive architecture documentation derived from ADRs:

* [Architecture Overview](architecture/directory-overview.md) - Complete navigation guide
* [ANIMA Architecture](architecture/anima-architecture.md) - Comprehensive system overview
* [Seed System](architecture/seed-system.md) - Identity initialization and separation
* [Module Types and Leases](architecture/module-types-and-leases.md) - Module lifecycle and authorization
* [Event Architecture](architecture/event-architecture.md) - Observability and input system
* [Cognitive Kernel](architecture/cognitive-kernel.md) - Core as a multitasking supervisor
* [Domain and Infrastructure](architecture/domain-and-infrastructure.md) - Hexagonal architecture implementation
* [AI Model Topology](architecture/ai-model-topology.md) - Cortex and Arcuate
* [Adapter-Actuator Split](architecture/adapter-actuator-split.md) - Module structure

### Foundation Documents

Constitutional principles and boundaries:

* [Foundations](foundations/) - Navigation to all foundation documents
* [Project Charter](vision/project-charter.md) - Core purpose, values, and non-goals
* [Canonical Glossary](foundations/canonical-glossary.md) - Definitive terminology and concepts
* [System Boundaries](foundations/system-boundaries.md) - What ANIMA can and cannot do

### Additional Documentation

* [Vision](vision/vision.md) - Long-term vision and goals
* [Memory Integrity](safety/memory-integrity.md) - Memory management and integrity
* [Safety Model](safety/safety-model.md) - Safety and security architecture
* [Threat Model](safety/threat-model.md) - Threat analysis and mitigation
* [Licensing Model](governance/licensing-model.md) - Licensing and distribution
* [Non-Goals](vision/non-goals.md) - Explicit non-goals and boundaries
* [Roadmap](roadmaps/roadmap.md) - Development roadmap
* [Announcements](announcements) - Project announcements and updates

---

## Philosophy

ANIMA is not designed to feel intelligent at all costs.

She is designed to feel **consistent, honest, and safe to grow with**.

## Frequently Asked Questions

**For complete FAQ, see [FAQ.md](FAQ.md)**

### What is ANIMA, exactly?

ANIMA is a **private, modular AI engine** designed to host long-lived, evolving artificial identities under strict safety, memory, and capability constraints.

It is **not**:
* a chatbot
* a single personality
* an autonomous agent that acts on its own

ANIMA is an *engine for growing identities*, not a prepackaged character.

### Where can I find the ANIMA engine?

The ANIMA engine lives in a separate repository called `engine-core`, which is currently **not public** while core safety, memory, and identity boundaries are under development.

This public docs repository exists to explain the architecture, document design decisions, and make the long-term vision transparent.

### How do I download or run ANIMA?

ANIMA is **not yet available for download**. When released, it will be distributed as an engine runtime where users initialize their own private instances using Seeds.

### What is a "Seed"?

A Seed is a **structured identity blueprint** used at initialization. It defines personality parameters, behavioral constraints, allowed capabilities, and initial narrative framing—but does not include memories, conversation history, or learned behavior.

### Is ANIMA safe to use?

Safety is a **core architectural requirement**. ANIMA enforces explicit permissions, capability allow/deny lists, human confirmation for dangerous actions, audit logging, and strict separation between identity and execution.

### More Questions?

See the complete [FAQ](FAQ.md) for detailed answers to common questions about architecture, safety, privacy, and development.

---

### How can I follow development?

Updates, architectural decisions, and philosophy are documented in this repository.

Major milestones will be announced once the engine reaches stable phases.

Announcements will be added in the `/announcements` folder and shared via future official channels.

---

## Contributing to Documentation

Interested in improving ANIMA documentation? See [DOCUMENTATION_MAINTENANCE.md](DOCUMENTATION_MAINTENANCE.md) for:

* How to propose documentation changes
* When to create new ADRs vs. update existing ones
* Documentation review guidelines
* Style and tone guidelines

---

# Português (Brasil)

![Status](https://img.shields.io/badge/status-desenvolvimento%20ativo-blue)
![Architecture](https://img.shields.io/badge/architecture-hexagonal%20%2F%20ports--and--adapters-purple)

**ANIMA** é um motor de IA privado e modular projetado para hospedar identidades artificiais de longa duração e em evolução, sob restrições rígidas de segurança, memória e capacidades.

Não é um chatbot, não é um wrapper de prompt e não é uma personalidade única.

ANIMA é um **motor para cultivar identidades**.

---

## Princípios Fundamentais

* **Motor ≠ Identidade**
  Raciocínio e execução são estritamente separados de personalidade e comportamento.

* **Privado por Design**
  Cada instância ANIMA possui sua própria memória e evolui independentemente.

* **Segurança sobre Capacidade**
  Permissões, confirmações e limites são aplicados no nível do motor.

* **Modularidade em Primeiro Lugar**
  Entradas, saídas, capacidades e incorporação são conectáveis.

* **Continuidade sobre Recordação**
  ANIMA prioriza identidade consistente e confiança sobre memória perfeita.

---

## Documentação

Explore a documentação completa em português:

### Documentos Principais

* [Visão](pt-br/vision/vision.md) - Visão de longo prazo e objetivos
* [Fundamentos](pt-br/foundations/) - Fundamentos arquiteturais centrais
* [Integridade da Memória](pt-br/safety/memory-integrity.md) - Gerenciamento e integridade da memória
* [Modelo de Segurança](pt-br/safety/safety-model.md) - Arquitetura de segurança
* [Modelo de Ameaças](pt-br/safety/threat-model.md) - Análise e mitigação de ameaças
* [Modelo de Licenciamento](pt-br/governance/licensing-model.md) - Licenciamento e distribuição
* [Não-Objetivos](pt-br/vision/non-goals.md) - Não-objetivos e limites explícitos

### Arquitetura e Especificações

* [Arquitetura](pt-br/architecture/anima-architecture.md) - Documentação detalhada da arquitetura
* [Especificações](pt-br/specs) - Especificações técnicas
* [ADRs (Registros de Decisões Arquiteturais)](pt-br/adr) - Decisões arquiteturais importantes

### Planejamento

* [Roadmap](pt-br/roadmaps/roadmap.md) - Roteiro de desenvolvimento
* [Anúncios](pt-br/announcements) - Anúncios e atualizações do projeto

---

## Status do Projeto

ANIMA está em desenvolvimento ativo.

Foco atual:

* Correção do motor central
* Integridade da memória
* Limites de capacidades
* Validação de seeds

Este projeto está evoluindo intencionalmente de forma lenta, priorizando correção e segurança.

---

# Español (México)

![Status](https://img.shields.io/badge/status-desarrollo%20activo-blue)
![Architecture](https://img.shields.io/badge/architecture-hexagonal%20%2F%20ports--and--adapters-purple)

**ANIMA** es un motor de IA privado y modular diseñado para alojar identidades artificiales de larga duración y en evolución, bajo estrictas restricciones de seguridad, memoria y capacidades.

No es un chatbot, no es un envoltorio de prompts y no es una personalidad única.

ANIMA es un **motor para cultivar identidades**.

---

## Principios Fundamentales

* **Motor ≠ Identidad**
  El razonamiento y la ejecución están estrictamente separados de la personalidad y el comportamiento.

* **Privado por Diseño**
  Cada instancia ANIMA posee su propia memoria y evoluciona independientemente.

* **Seguridad sobre Capacidad**
  Los permisos, confirmaciones y límites se aplican a nivel del motor.

* **Modularidad Primero**
  Las entradas, salidas, capacidades y encarnación son conectables.

* **Continuidad sobre Recordación**
  ANIMA prioriza identidad consistente y confianza sobre memoria perfecta.

---

## Documentación

Explora la documentación completa en español:

### Documentos Principales

* [Visión](es-mx/vision/vision.md) - Visión a largo plazo y objetivos
* [Fundamentos](es-mx/foundations/) - Fundamentos arquitectónicos centrales
* [Integridad de la Memoria](es-mx/safety/memory-integrity.md) - Gestión e integridad de la memoria
* [Modelo de Seguridad](es-mx/safety/safety-model.md) - Arquitectura de seguridad
* [Modelo de Amenazas](es-mx/safety/threat-model.md) - Análisis y mitigación de amenazas
* [Modelo de Licenciamiento](es-mx/governance/licensing-model.md) - Licenciamiento y distribución
* [No-Objetivos](es-mx/vision/non-goals.md) - No-objetivos y límites explícitos

### Arquitectura y Especificaciones

* [Arquitectura](es-mx/architecture/anima-architecture.md) - Documentación detallada de la arquitectura
* [Especificaciones](es-mx/specs) - Especificaciones técnicas
* [ADRs (Registros de Decisiones Arquitectónicas)](es-mx/adr) - Decisiones arquitectónicas importantes

### Planificación

* [Roadmap](es-mx/roadmaps/roadmap.md) - Hoja de ruta del desarrollo
* [Anuncios](es-mx/announcements) - Anuncios y actualizaciones del proyecto

---

## Estado del Proyecto

ANIMA está en desarrollo activo.

Enfoque actual:

* Corrección del motor central
* Integridad de la memoria
* Límites de capacidades
* Validación de seeds

Este proyecto está evolucionando intencionalmente de forma lenta, priorizando corrección y seguridad.

---
