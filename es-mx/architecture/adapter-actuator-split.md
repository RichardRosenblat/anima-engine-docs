# División Adapter–Actuator (Propiedad del Módulo, Fuera de Proceso)

**ADRs Relacionados:**
* [ADR-003: Protocolo de Comunicación Core ↔ Módulo, Tipos de Módulos y Ciclo de Vida de Lease](../adr/ADR-003.md)
* [ADR-004: Observabilidad, Registro de Eventos y Rastreabilidad de Ejecución](../adr/ADR-004.md)
* [ADR-005: Modelo de Interrupción & Preempción](../adr/ADR-005.md)
* [ADR-010: División Adapter–Actuator con Frontera de Proceso Estricta](../adr/ADR-010.md)
* [ADR-011: Arquitectura de Entrada Basada en Eventos](../adr/ADR-011.md)

---

## Visión General

Este documento especifica cómo se estructuran los Módulos para proteger el Core del código externo mientras preserva contratos limpios.

Cada Módulo se divide en dos partes, ambas ejecutando **fuera de proceso** del Core:

* **Adapter** — Traducción pura y validación de schema
* **Actuator** — Ejecución con efectos que toca APIs/hardware

El Core **nunca carga o ejecuta código de propiedad del módulo**. Toda interacción Core ↔ Módulo ocurre sobre **gRPC con mTLS** y **leases emitidos por el Core**.

---

## Principio de Seguridad Central

> **El Core ejecuta solo código confiable, de propiedad del motor.**

* Adapters y Actuators ejecutan en procesos de propiedad del módulo
* Toda comunicación usa gRPC sobre mTLS con identidades atestadas (Core URN, Module URN) y leases emitidos por el Core
* Sin importaciones dinámicas, plugins o ejecución Python de módulos dentro del Core

---

## Estructura de Paquete de Módulo

Ejemplo de paquete de módulo:

```
module-package/
├── adapter/
│   ├── input/  (señales → eventos input.*; mapeo puro)
│   └── output/ (intención → comandos; mapeo puro + validación de schema)
├── actuator/ (ejecución con efectos: APIs/hardware)
├── capability_contract/ (JSON Schemas + manifiesto)
├── transport/ (servidor/cliente gRPC; mTLS; imposición de lease)
└── proto/ (IDL para servicios CapabilityGateway y Adapter)
```

---

## Adapter (Lado del Módulo, Traducción Pura)

### Responsabilidades

**Camino de entrada:**
* Mapea señales capturadas → eventos de entrada del Core

**Camino de salida:**
* Mapea Intención del Core (URN de capacidad + payload) → schema de comando del módulo

### Deberes Principales

* Traducción determinística
* Validación de JSON Schema contra el contrato de capacidad
* Imposición de alcance de lease en llamadas de salida

### Prohibiciones

* Sin planificación, sin política
* Sin I/O externo más allá de gRPC al Core o invocación de actuator local
* Sin acceso a memoria o internals del Core

---

## Actuator (Lado del Módulo, Ejecución con Efectos)

### Responsabilidades

* Ejecuta efectos secundarios del mundo real (APIs, hardware, plataformas)
* Impone alcance de lease y políticas de interrupción (ADR-005)
* Emite eventos de observabilidad con sobre ADR-004
* Contiene cero lógica o razonamiento del Core

---

## Flujo de Entrada (Inputs → Core)

1. Runtime del módulo captura señales (texto, audio, eventos de plataforma)
2. Adapter mapea señales a eventos de entrada con sobre ADR-004
3. Adapter publica eventos vía gRPC
4. Core ingiere eventos (nunca IO crudo)

Tipos de eventos (vea [Arquitectura de Eventos](event-architecture.md)):
* `input.nl` — lenguaje natural (pre-semántica)
* `input.system` — hechos no-lingüísticos, mecánicos/de sistema
* `input.semantic` — significado afirmado post-interpretación

---

## Flujo de Salida (Intención del Core → Comando del Módulo)

1. Core produce una Intención con URN de capacidad + payload
2. Core llama al gateway de capacidad en el Adapter del Módulo
3. Adapter valida payload contra JSON Schema de capacidad
4. Adapter mapea a comando del Actuator
5. Adapter impone alcance de lease
6. Adapter invoca Actuator
7. Actuator ejecuta y emite eventos

---

## Procesamiento de Lenguaje Natural en Módulos

Los módulos que manejan entrada y salida de lenguaje natural PUEDEN enviar una implementación NLP o delegar responsabilidades NLP al Arcuate del Core (vea [Topología de Modelos de IA](ai-model-topology.md)).

### Entrada de Lenguaje Natural

**Opción A — NLP de propiedad del módulo:**
* El Adapter del módulo realiza NLP local para transformar eventos `input.nl` → `input.semantic`
* Restricciones:
  * Solo traducción; sin planificación o política
  * Incluir confianza y proveniencia en payloads semánticos
  * Sin acceso a memoria del Core; proveniencia refleja "módulo"

**Opción B — Delegar al Arcuate:**
* El Adapter del módulo publica eventos `input.nl` al Core
* El Arcuate del Core consume `input.nl` y produce `input.semantic`
* Core razona sobre eventos semánticos
* Restricciones:
  * Adapter permanece puro; sin NLP incrustado
  * Frontera de proceso preservada; sin código de plugin en el Core

### Salida de Lenguaje Natural (e.g., TTS, entrega de chat)

**Opción A — NLG de propiedad del módulo:**
* El Adapter del módulo mapea intenciones del Core a schemas de comando de salida y realiza NLG local (realización de superficie) antes de la ejecución del Actuator
* Restricciones:
  * NLG es traducción; sin planificación, sin acceso a memoria
  * Eventos de observabilidad DEBEN capturar intención, metadata de realización y resultado de ejecución

**Opción B — Delegar al Arcuate:**
* El Core produce la superficie lingüística vía Arcuate del contenido semántico de una intención
* El Adapter del módulo traduce el texto realizado al comando del módulo
* El Actuator ejecuta (e.g., hablar/enviar)
* Restricciones:
  * Adapter permanece puro; Actuator realiza efectos secundarios
  * Uso de Arcuate sigue reglas de aislamiento ADR-009

En todos los casos:
* Adapters permanecen mapeo puro + validación
* Actuators realizan IO y efectos bajo leases válidos
* Eventos semánticos DEBEN incluir campos de confianza y proveniencia
* Lenguaje natural NO DEBE desencadenar ejecución directa; el pipeline permanece Entrada → Interpretación → Intención → Validación/Confirmación → Ejecución

---

## Descubrimiento & Confianza

Los módulos presentan un manifiesto con:
* URN del Módulo
* URNs de Capacidad y versiones
* Hashes de JSON Schema
* Tipo de Módulo Declarado (vea [Tipos de Módulos y Leases](module-types-and-leases.md): Tipo I/II/III)

Core valida manifiesto sobre mTLS en el handshake; **ningún código cargado del Módulo**.

Todas las solicitudes llevan LeaseID/Epoch; epochs obsoletos son rechazados.

---

## Protocolo & Contratos

### Entrada (Módulo → Core)
* Adapter publica eventos de entrada y tipos de eventos

### Salida (Core → Módulo)
* Core llama Adapter vía un Capability Gateway genérico:
  * `capability_urn` + `schema_version` + `payload` (validado por adapter)
  * `LeaseMeta` (lease_id, epoch)
  * `ExecMeta` (execution/trace/span)

### Contrato de Capacidad

JSON Schemas declarativos para payloads de comando conteniendo:
* Clase de efecto secundario del método (reversible/irreversible)
* Política de interrupción (soft-stop/hard-stop/checkpoint/non-interruptible)
* Duración máxima de lease

---

## Observabilidad

Tanto Adapter como Actuator emiten eventos estructurados conforme ADR-004:

Tipos de eventos:
* `capability.invoked`
* `capability.completed`
* `capability.interrupted`
* `validation.failed`

Todos los eventos incluyen:
* `execution_id`, `trace_id`, `span_id`, `thread_id`, `timestamp`
* `source=module:<name>`
* `type=<event.type.identifier>`
* `payload=<structured fields only>`

Sin logging de texto libre como autoridad; los eventos son la fuente de verdad.

---

## Invariantes de Seguridad

* Sin código de plugin en el Core — jamás
* Toda ejecución de módulo requiere un lease válido con epoch actual
* Promoción/degradación de alcance ocurre solo vía actualizaciones de lease emitidas por el Core
* Zero-Lease State: módulos son inertes (Tipo II en standby o Tipo I termina)
* Adapters no pueden ampliar alcance o eludir el control del Core

---

## Por Qué Esto es Seguro

* Core procesa solo eventos, nunca código de terceros
* Traducción y ejecución son fuera de proceso
* Schemas declarativos previenen payloads ad-hoc
* Leases y epochs previenen replay/escalación
* Elimina riesgo de inyección de código en el Core
* Preserva límites hexagonales y testabilidad
* División clara de responsabilidad (traducir vs ejecutar)
* Fuerte auditabilidad vía eventos estructurados

---

## Resumen

> **El Core razona y supervisa. Los módulos adaptan y actúan.**
> **Ningún código de terceros ejecuta dentro del Core.**
