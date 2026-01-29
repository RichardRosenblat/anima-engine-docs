# Comenzando con la Documentación de ANIMA

**Propósito:** Esta guía ayuda a nuevos lectores a navegar la documentación de ANIMA de manera eficiente.

ANIMA es un motor de IA privado y modular diseñado para alojar identidades artificiales de larga duración y evolutivas bajo estrictas restricciones de seguridad, memoria y capacidades. Esta documentación explica la arquitectura, decisiones de diseño y fundamentos filosóficos del sistema.

---

## ¿Para Quién es Esta Documentación?

Esta documentación sirve a múltiples audiencias:

### 1. **Revisores de Arquitectura**
Deseas comprender el diseño del sistema de ANIMA, patrones arquitectónicos y la razón fundamental del diseño.

**Comienza aquí:**
* [Vision](vision/vision.md) - Comprende qué intenta habilitar ANIMA
* [ANIMA Architecture](architecture/anima-architecture.md) - Vista general completa del sistema
* [ADRs](adr/) - Registros de Decisiones de Arquitectura (lee en orden, ADR-001 hasta ADR-011)

### 2. **Revisores de Seguridad y Gobernanza**
Necesitas evaluar el modelo de seguridad de ANIMA, límites de amenazas y enfoque de gobernanza.

**Comienza aquí:**
* [Safety Model](safety/safety-model.md) - Arquitectura de seguridad y protección
* [Threat Model](safety/threat-model.md) - Análisis de amenazas y estrategias de mitigación
* [Memory Integrity](safety/memory-integrity.md) - Restricciones de gestión de memoria
* [Non-Goals](vision/non-goals.md) - Límites explícitos y lo que ANIMA se rehúsa a convertirse

### 3. **Futuros Implementadores**
Eventualmente podrías implementar, integrar o extender componentes de ANIMA.

**Comienza aquí:**
* [Canonical Glossary](foundations/canonical-glossary.md) - Terminología autoritativa (lee primero)
* [Seed System](architecture/seed-system.md) - Cómo se inicializan las identidades
* [Module Types and Leases](architecture/module-types-and-leases.md) - Ciclo de vida y autorización de módulos
* [Event Architecture](architecture/event-architecture.md) - Sistema de entrada y observabilidad

### 4. **Lectores Técnicos Generales**
Deseas una comprensión amplia de qué es ANIMA y por qué existe.

**Comienza aquí:**
* [README](README.md) - Introducción de alto nivel y FAQ
* [Vision](vision/vision.md) - Objetivos a largo plazo y fundamentos filosóficos
* [Why Not an Agent?](vision/why-not-an-agent.md) - Qué hace diferente a ANIMA
* [Project Charter](vision/project-charter.md) - Propósito y valores centrales

---

## Órdenes de Lectura Sugeridos

### Vista General Rápida (15-30 minutos)
Para una comprensión rápida de los conceptos centrales de ANIMA:

1. [README](README.md) - Introducción y vista general de alto nivel
2. [Vision](vision/vision.md) - Propósito y filosofía
3. [ANIMA Architecture](architecture/anima-architecture.md) - Vista general del diseño del sistema
4. [Non-Goals](vision/non-goals.md) - Lo que ANIMA no es

### Revisión Completa (2-4 horas)
Para una comprensión exhaustiva de todo el sistema:

1. **Fundamentos** (30 minutos)
   * [Vision](vision/vision.md)
   * [Project Charter](vision/project-charter.md)
   * [Canonical Glossary](foundations/canonical-glossary.md)
   * [System Boundaries](foundations/system-boundaries.md)

2. **Arquitectura** (1-2 horas)
   * [ANIMA Architecture](architecture/anima-architecture.md)
   * [Cognitive Kernel](architecture/cognitive-kernel.md)
   * [Seed System](architecture/seed-system.md)
   * [Module Types and Leases](architecture/module-types-and-leases.md)
   * [Event Architecture](architecture/event-architecture.md)

3. **Seguridad y Gobernanza** (30-45 minutos)
   * [Safety Model](safety/safety-model.md)
   * [Threat Model](safety/threat-model.md)
   * [Memory Integrity](safety/memory-integrity.md)
   * [Licensing Model](governance/licensing-model.md)

4. **Decisiones de Diseño** (45-60 minutos)
   * Lee los ADRs en orden: [ADR-001](adr/ADR-001.md) hasta [ADR-011](adr/ADR-011.md)

### Inmersión Arquitectónica Profunda (4-6 horas)
Para implementadores y revisores de arquitectura que necesitan una comprensión completa:

1. **Definiciones Canónicas Primero**
   * [Canonical Glossary](foundations/canonical-glossary.md) - Lee esto antes que cualquier otra cosa
   * [System Boundaries](foundations/system-boundaries.md)

2. **Fundamentación Filosófica**
   * [Vision](vision/vision.md)
   * [Why Not an Agent?](vision/why-not-an-agent.md)
   * [Reinvention Rationale](foundations/reinvention-rationale.md)
   * [Non-Goals](vision/non-goals.md)

3. **Conjunto Completo de ADRs** (en orden)
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

4. **Documentación Completa de Arquitectura**
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

5. **Revisión Completa de Seguridad y Gobernanza**
   * [Safety Model](safety/safety-model.md)
   * [Threat Model](safety/threat-model.md)
   * [Memory Integrity](safety/memory-integrity.md)
   * [Licensing Model](governance/licensing-model.md)
   * [Project Viability](governance/project-viability.md)

6. **Especificaciones**
   * [ANIMA URN Specification](specs/anima-urn.md)
   * [JSON Reference System (JSRS)](specs/json_reference_system.md)

---

## Conceptos Clave a Comprender

Antes de profundizar, familiarízate con estos conceptos fundamentales:

### Engine ≠ Identity
ANIMA separa el motor de razonamiento de la identidad. El Engine es agnóstico de identidad; las personalidades se definen mediante **Seeds** (archivos de configuración declarativos).

### Seguridad Sobre Capacidad
ANIMA trata la capacidad como riesgo. Cada acción requiere autorización explícita, puede ser inspeccionada y deja un rastro de auditoría. La seguridad es arquitectónica, no basada en prompts.

### Privado por Diseño
Cada instancia de ANIMA posee su memoria y evoluciona independientemente. No hay un pool de memoria compartido ni aprendizaje entre instancias.

### Continuidad Sobre Recuerdo
ANIMA prioriza la identidad consistente y la continuidad narrativa sobre la memoria fotográfica perfecta. La memoria tiene políticas de degradación intencionales.

### Arquitectura Hexagonal
ANIMA usa arquitectura de puertos y adaptadores. El Core no tiene conocimiento de plataformas, personalidades o encarnación. Toda la E/S ocurre a través de módulos.

---

## Preguntas Comunes

Para preguntas frecuentes, consulta [FAQ.md](FAQ.md).

Para preguntas sobre qué ANIMA no está diseñado a hacer, consulta [Non-Goals](vision/non-goals.md).

---

## Consejos de Navegación

* **Canonical Glossary** ([foundations/canonical-glossary.md](foundations/canonical-glossary.md)) - Usa esto como tu referencia para toda la terminología. Las definiciones son autoritativas y nunca deben desviarse.

* **Los ADRs están numerados** - Léelos en orden (001-011) para comprender la evolución del pensamiento arquitectónico.

* **Las referencias cruzadas son intencionales** - Los documentos se vinculan entre sí deliberadamente. Sigue los enlaces para comprender las relaciones.

* **Secciones de "Documentación Relacionada"** - La mayoría de los documentos incluyen estas al inicio. Úsalas para contexto.

---

## Lo Que ANIMA No Es

Para evitar malentendidos sobre el propósito de ANIMA, comprende lo que explícitamente **no** es:

* No es un chatbot o un wrapper de IA conversacional
* No es un agente autónomo de propósito general
* No es una personalidad única y fija
* No es un sistema de memoria compartida o mente colmena
* No es un framework de ingeniería de prompts
* No es un sistema RAG (Retrieval-Augmented Generation)

Consulta [Why Not an Agent?](vision/why-not-an-agent.md) para explicaciones detalladas.

---

## Estado de la Documentación

ANIMA está actualmente en desarrollo activo. La implementación del Engine vive en un repositorio separado y privado (`engine-core`).

Este repositorio de documentación existe para:
* Explicar la arquitectura de manera transparente
* Documentar decisiones de diseño públicamente
* Hacer que la visión y el modelo de seguridad sean revisables antes del lanzamiento

Para el estado actual de desarrollo, consulta [Roadmap](roadmaps/roadmap.md).

---

## Contribuyendo a la Documentación

Para orientación sobre proponer cambios en la documentación, consulta [DOCUMENTATION_MAINTENANCE.md](DOCUMENTATION_MAINTENANCE.md).

---

## Recursos Adicionales

* [Announcements](announcements/) - Anuncios y actualizaciones del proyecto
* [Roadmap](roadmaps/roadmap.md) - Hoja de ruta de desarrollo
* [Project Viability](governance/project-viability.md) - Evaluación de sostenibilidad a largo plazo

---

**Última Actualización:** 28 de enero de 2026
