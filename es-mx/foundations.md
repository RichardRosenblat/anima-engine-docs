# Fundamentos de ANIMA

Este documento proporciona navegación hacia los tres documentos fundamentales que definen la ley constitucional, el vocabulario canónico y los límites arquitectónicos de ANIMA.

---

## Documentos Fundamentales

### [1️⃣ Carta del Proyecto](project-charter.md)

**Propósito:** Define *"qué existe"* y *"qué está prohibido"*.

**Contenido:**
* Qué es ANIMA (y qué no es)
* Propósito central y valores
* No-objetivos explícitos
* Invariantes arquitectónicas
* Criterios de éxito
* Modelo de gobernanza

**Principio Fundamental:** Cada funcionalidad debe ser justificable contra esta carta.

**Leer:** [Carta del Proyecto →](project-charter.md)

---

### [2️⃣ Glosario Canónico](canonical-glossary.md)

**Propósito:** Establece el vocabulario canónico de ANIMA.

**Contenido:**
* Componentes del sistema (Engine, Core, Seed, Memory, etc.)
* Conceptos arquitectónicos (Module, Adapter, Actuator, Capability, etc.)
* Modelos de IA (Cortex, Arcuate)
* Conceptos de observabilidad (Event, Execution, Lease, etc.)
* Proceso y estado (Zero-Lease State, Task Recovery Grace Period)

**Principio Fundamental:** Estas definiciones **nunca deben desviarse**.

**Leer:** [Glosario Canónico →](canonical-glossary.md)

---

### [3️⃣ Límites del Sistema](system-boundaries.md)

**Propósito:** Define qué puede y no puede hacer ANIMA por diseño.

**Contenido:**
* Lo que el motor NUNCA puede hacer (10 invariantes arquitectónicas)
* Lo que SIEMPRE DEBE requerir confirmación (5 categorías)
* Lo que se delega a los módulos (5 responsabilidades)
* Límites de seguridad
* Modos de falla y límites

**Principio Fundamental:** Los límites son impuestos por la arquitectura, no por convención. Las violaciones son bugs.

**Leer:** [Límites del Sistema →](system-boundaries.md)

---

## Relación con la Arquitectura

Estos documentos fundamentales trabajan junto con la [Documentación de Arquitectura](architecture/README.md) para definir completamente ANIMA:

* **Fundamentos** (este) → Principios constitucionales y vocabulario
* **Arquitectura** → Implementación detallada y patrones de diseño
* **ADRs** → Registros formales de decisión explicando el porqué

---

## Referencia Rápida

### Valores Centrales

1. **Verdad sobre confianza** — Admitir incertidumbre en lugar de fabricar
2. **Intención sobre ejecución** — Producir intención, no efectos directos
3. **Modularidad sobre monolito** — Componentes intercambiables
4. **Seguridad sobre capacidad** — Permisos primero, funcionalidades después
5. **Configurabilidad sobre hardcoding** — Comportamiento orientado por datos (Seeds)
6. **Aislamiento sobre conveniencia** — Seguridad a través de la separación

### Invariantes Arquitectónicas

1. El motor es agnóstico de identidad (personalidad viene de Seeds)
2. El Core nunca carga código de terceros (módulos fuera de proceso)
3. Toda ejecución requiere leases válidos
4. Los dominios nunca importan infraestructura (arquitectura hexagonal)
5. Toda observabilidad está basada en eventos (sin logs tradicionales)
6. La memoria está estrictamente limitada a la instancia
7. Cortex es obligatorio, Arcuate es opcional
8. Core supervisa, no ejecuta (kernel cognitivo)
9. Todas las acciones son interrumpibles (interrupción cooperativa)
10. Los eventos son la fuente de verdad (inmutables, auditables)

### Límites Principales

* **Core nunca toca el mundo** — Intención, no efectos
* **Sin lease, sin ejecución** — Zero-Lease State
* **Sin memoria entre instancias** — Solo local a la instancia
* **Sin auto-modificación** — Código y Seeds estables
* **Acciones peligrosas requieren confirmación** — Aprobación explícita del usuario

---

## Resumen

> **ANIMA es un motor para identidades en crecimiento, no una personalidad única.**
>
> **Estos fundamentos son constitucionales. Definen qué es ANIMA y qué nunca puede llegar a ser.**
>
> **La precisión en el lenguaje previene confusión en la arquitectura.**
