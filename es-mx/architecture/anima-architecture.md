# 🧩 Arquitectura ANIMA — Visión General Completa

**Documentación Relacionada:**
* [Sistema de Seeds](seed-system.md) — Inicialización y separación de identidad
* [Tipos de Módulos y Leases](module-types-and-leases.md) — Ciclo de vida y autorización de módulos
* [Arquitectura de Eventos](event-architecture.md) — Sistema de observabilidad y entrada
* [Kernel Cognitivo](cognitive-kernel.md) — Core como supervisor multitarea
* [Dominio e Infraestructura](domain-and-infrastructure.md) — Implementación de arquitectura hexagonal
* [Topología de Modelos de IA](ai-model-topology.md) — Cortex y Arcuate
* [División Adapter-Actuator](adapter-actuator-split.md) — Estructura de módulos

**ADRs Relacionados:** Todos los ADRs (ADR-001 hasta ADR-011)

---

## Introducción

ANIMA es un **motor de IA privado y modular** diseñado para alojar identidades artificiales de larga duración y en evolución bajo estrictas restricciones de seguridad, memoria y capacidad.

La arquitectura sigue principios de **Arquitectura Hexagonal** y **Diseño Orientado por Dominio (DDD)**, con fuerte énfasis en:

* Separación de responsabilidades
* Seguridad por diseño
* Comportamiento observable y auditable
* Aislamiento de identidad
* Extensibilidad modular

---

## Principios Arquitectónicos Centrales

### 1. Motor ≠ Identidad

El Motor ANIMA es agnóstico de identidad. La personalidad y el comportamiento se introducen a través de **Seeds** (vea [Sistema de Seeds](seed-system.md)).

* Motor: razonamiento, planificación, mecánica de memoria, seguridad
* Identidad: personalidad, tono, políticas de comportamiento (definidas por Seed)

### 2. Arquitectura Hexagonal

> **Los dominios nunca deben hablar directamente con el mundo externo.**

* Los dominios definen **puertos** (interfaces)
* La infraestructura proporciona **adaptadores** (implementaciones)
* El runtime compone el sistema

Vea [Dominio e Infraestructura](domain-and-infrastructure.md) para detalles.

### 3. Observabilidad y Entrada Basadas en Eventos

* Toda la observabilidad se expresa como **eventos estructurados**
* Todas las entradas se transforman en **eventos** antes de llegar al Core
* Sin logs tradicionales, solo hechos inmutables

Vea [Arquitectura de Eventos](event-architecture.md) para detalles.

### 4. Core como Kernel Cognitivo

El Core ANIMA se comporta como un **Kernel Cognitivo**, supervisando tareas concurrentes en lugar de ejecutarlas directamente.

Vea [Kernel Cognitivo](cognitive-kernel.md) para detalles.

### 5. Autorización Basada en Lease

Toda comunicación Core ↔ Módulo está controlada por **leases criptográficos** sobre **gRPC con mTLS**.

Vea [Tipos de Módulos y Leases](module-types-and-leases.md) para detalles.

---

## 🌍 MUNDO EXTERNO

```
[ Usuario ]        [ Plataformas / Hardware / APIs ]
```

Sin inteligencia aquí. Solo realidad.

---

## 🟦 CAPA DE MÓDULOS (Frontera con Efectos)

> **Única capa que toca el mundo real**

```
┌───────────────────────────────────────────┐
│               MÓDULOS                     │
│                                           │
│  • Módulo de Entrada Discord              │
│  • Módulo de Entrada CLI                  │
│  • Módulo de Entrada Micrófono            │
│                                           │
│  • Módulo de Salida Discord               │
│  • Módulo de Salida CLI                   │
│  • Módulo de Salida TTS                   │
│  • Módulo de Salida Live2D                │
│                                           │
│  (APIs, hardware, streaming, dispositivos)│
└───────────────────────────────────────────┘
```

📌 **Reglas de Módulos:**

* Los módulos **capturan** o **ejecutan**
* Los módulos **no piensan**
* Los módulos **no deciden**
* Todos los módulos ejecutan **fuera de proceso** del Core
* Comunicación a través de **gRPC con mTLS**

### Tipos de Módulos

Cada módulo declara uno de tres tipos:

* **Tipo I** — Privado Efímero (vinculado a lease, Core único)
* **Tipo II** — Privado Residente (larga duración, Core único)
* **Tipo III** — Compartido Residente (multi-tenant, gobernado por infraestructura)

Vea [Tipos de Módulos y Leases](module-types-and-leases.md) para especificaciones completas.
