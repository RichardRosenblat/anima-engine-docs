# Documentación de Arquitectura ANIMA

Esta carpeta contiene documentación arquitectónica detallada para el Motor ANIMA, derivada de los Architecture Decision Records (ADRs) y principios de diseño.

---

## Documentos Principales de Arquitectura

### [Visión General de Arquitectura ANIMA](anima-architecture.md)
Visión general completa de toda la arquitectura ANIMA, incluyendo todas las capas y conceptos clave.

**Temas cubiertos:**
* Separación de Motor vs. Identidad
* Implementación de Arquitectura Hexagonal
* Observabilidad y entrada basadas en eventos
* Modelo de Kernel Cognitivo
* Flujo completo del sistema

---

## Documentación Detallada de Temas

### [Sistema de Seeds](seed-system.md)
**Relacionado:** [ADR-001](../adr/ADR-001.md)

Explica cómo ANIMA separa el motor de la identidad a través de Seeds declarativos.

**Temas cubiertos:**
* Qué es un Seed
* Motor vs. Identidad
* Invariantes arquitectónicos
* Casos de uso (Instancias privadas, ANIMA Prime)

---

### [Tipos de Módulo y Leases](module-types-and-leases.md)
**Relacionado:** [ADR-003](../adr/ADR-003.md)

Detalla el modelo de autorización basado en lease y tres tipos de módulos.

**Temas cubiertos:**
* Comunicación gRPC sobre mTLS
* Autorización basada en lease
* Épocas de lease y anti-desincronización
* Módulos Tipo I, II y III
* Estado Zero-Lease
* Reglas de persistencia de estado

---

### [Arquitectura de Eventos](event-architecture.md)
**Relacionado:** [ADR-004](../adr/ADR-004.md), [ADR-011](../adr/ADR-011.md)

Describe la arquitectura unificada basada en eventos para observabilidad y procesamiento de entrada.

**Temas cubiertos:**
* Modelo de observabilidad basado en eventos
* Particionamiento de ejecución
* Estructura y campos de eventos
* Categorías canónicas de eventos (input.nl, input.system, input.semantic)
* Concurrencia y multithreading
* Participación de módulos

---

### [Kernel Cognitivo](cognitive-kernel.md)
**Relacionado:** [ADR-005](../adr/ADR-005.md), [ADR-008](../adr/ADR-008.md)

Explica cómo el ANIMA Core opera como un kernel cognitivo multitarea.

**Temas cubiertos:**
* Tareas como unidades de ejecución administradas
* Modelo de interrupción cooperativa
* Interrupciones como eventos de primera clase
* Períodos de ejecución y enfoque
* Políticas de interruptibilidad
* Kernel vs. espacio de Usuario
* Modelo de multitarea

---

### [Dominio e Infraestructura](domain-and-infrastructure.md)
**Relacionado:** [ADR-002](../adr/ADR-002.md), [ADR-006](../adr/ADR-006.md), [ADR-007](../adr/ADR-007.md)

Formaliza la implementación de la Arquitectura Hexagonal en ANIMA.

**Temas cubiertos:**
* Principio central (los dominios nunca hablan con el mundo externo)
* Reglas de dirección de dependencia
* Formas de dependencia entre dominios aprobadas
* Traducción de errores entre dominios
* Esquemas auto-validables
* Capa de infraestructura y adaptadores
* Composición en tiempo de ejecución

---

### [Topología del Modelo de IA](ai-model-topology.md)
**Relacionado:** [ADR-009](../adr/ADR-009.md)

Detalla la topología de modelo de IA de dos roles: Córtex y Arcuate.

**Temas cubiertos:**
* Córtex (modelo cognitivo obligatorio)
* Arcuate (modelo de PLN opcional)
* PLN como una responsabilidad configurable
* Configuraciones soportadas (modelo único, modelo dual, solo Córtex)
* Arquitectura basada en interfaz
* Restricciones e invariantes

---

### [División Adaptador-Actuador](adapter-actuator-split.md)
**Relacionado:** [ADR-003](../adr/ADR-003.md), [ADR-004](../adr/ADR-004.md), [ADR-005](../adr/ADR-005.md), [ADR-010](../adr/ADR-010.md), [ADR-011](../adr/ADR-011.md)

Especifica cómo los Módulos se estructuran con Adaptadores y Actuadores ejecutándose fuera de proceso.

**Temas cubiertos:**
* Principio de seguridad central (ningún código de terceros en Core)
* Estructura de paquete de módulo
* Responsabilidades del adaptador (traducción pura)
* Responsabilidades del actuador (ejecución con efectos)
* Flujos de entrada y salida
* Procesamiento de lenguaje natural en módulos
* Descubrimiento y confianza
* Observabilidad
* Invariantes de seguridad

---

## Principios Arquitectónicos Clave

1. **Motor ≠ Identidad** — La personalidad viene de Seeds, no del motor
2. **Ningún código de terceros en Core** — Todos los módulos se ejecutan fuera de proceso
3. **Autorización basada en lease** — Toda ejecución requiere leases válidos
4. **Los dominios nunca importan infraestructura** — Fronteras hexagonales limpias
5. **Todo basado en eventos** — No hay logs tradicionales, solo eventos estructurados
6. **Memoria con alcance de instancia** — Sin compartir entre instancias
7. **Córtex obligatorio, Arcuate opcional** — Separación de cognición vs. lenguaje
8. **Core supervisa, no ejecuta** — Modelo de kernel cognitivo
9. **Interrupción cooperativa** — Todas las acciones son interrumpibles por diseño
10. **Los eventos son la fuente de la verdad** — Hechos inmutables y auditables

---

## Architecture Decision Records (ADRs)

Para los registros de decisión formales que definen esta arquitectura, vea la [carpeta ADR](../adr/):

* [ADR-001](../adr/ADR-001.md) — Separación del Motor ANIMA e Identidad vía Sistema de Seeds
* [ADR-002](../adr/ADR-002.md) — Esquemas Auto-Validables con Restricciones Solo Internas
* [ADR-003](../adr/ADR-003.md) — Protocolo de Comunicación Core ↔ Módulo, Tipos de Módulo y Ciclo de Vida de Lease
* [ADR-004](../adr/ADR-004.md) — Observabilidad, Registro de Eventos y Trazabilidad de Ejecución
* [ADR-005](../adr/ADR-005.md) — Modelo de Interrupción y Preempción
* [ADR-006](../adr/ADR-006.md) — Reglas de Dependencia y Compartición de Dominio
* [ADR-007](../adr/ADR-007.md) — Capa de Infraestructura y Reglas de Adaptador
* [ADR-008](../adr/ADR-008.md) — ANIMA Core se Comporta como un Kernel Cognitivo
* [ADR-009](../adr/ADR-009.md) — Topología de Modelo de IA Configurable con Córtex Obligatorio y Arcuate Opcional
* [ADR-010](../adr/ADR-010.md) — División Adaptador–Actuador con Frontera de Proceso Estricta
* [ADR-011](../adr/ADR-011.md) — Arquitectura de Entrada Basada en Eventos

---

## Referencia Rápida

### Flujo del Sistema

```
Usuario/Ambiente
  ↓ (captura)
Módulo
  ↓ (traducir a eventos)
Adaptador
  ↓ (input.nl / input.system / input.semantic)
Seguridad
  ↓
Kernel Cognitivo (Córtex + Arcuate opcional)
  ↔ Memoria / Seed
  ↓ (intención + URN de capacidad)
Seguridad (validar)
  ↓
Adaptador (validar esquema)
  ↓
Actuador (ejecutar)
  ↓
Efecto + Eventos
```

### Tipos de Módulo

* **Tipo I** — Privado Efímero (vinculado a lease, termina en Zero-Lease)
* **Tipo II** — Privado Residente (standby en Zero-Lease, Core único)
* **Tipo III** — Compartido Residente (multi-tenant, gobernado por infraestructura)

### Tipos de Evento

* **input.nl** — Lenguaje natural (pre-semántico)
* **input.system** — Hechos del sistema/mecánicos
* **input.semantic** — Significado afirmado post-interpretación
* **capability.invoked** — Ejecución de capacidad iniciada
* **capability.completed** — Ejecución de capacidad completada
* **capability.interrupted** — Ejecución de capacidad interrumpida

---

## Navegación

* [README Principal](../README.md)
* [Índice de ADR](../adr/)
* [Visión](../vision/vision.md)
* [Fundamentos](../foundations/canonical-glossary.md)
* [Integridad de la Memoria](../safety/memory-integrity.md)
* [Modelo de Seguridad](../safety/safety-model.md)
* [Modelo de Amenazas](../safety/threat-model.md)
