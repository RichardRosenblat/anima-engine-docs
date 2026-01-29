# Documentación del Motor ANIMA

![Status](https://img.shields.io/badge/status-desarrollo%20activo-blue)
![Architecture](https://img.shields.io/badge/architecture-hexagonal%20%2F%20ports--and--adapters-purple)

**ANIMA** es un motor de IA privado y modular diseñado para alojar identidades artificiales de larga duración y en evolución, bajo estrictas restricciones de seguridad, memoria y capacidades.

No es un chatbot, no es un envoltorio de prompts y no es una personalidad única.

ANIMA es un **motor para cultivar identidades**.

---

## Comenzando

**¿Nuevo en ANIMA?** Comienza aquí:

* **[Guía de Inicio](es-mx/GETTING_STARTED.md)** - Navega la documentación de manera eficiente con órdenes de lectura sugeridos para diferentes audiencias
* **[FAQ](es-mx/FAQ.md)** - Preguntas comunes y respuestas rápidas
* **[Visión](es-mx/vision/vision.md)** - Comprende lo que ANIMA está tratando de habilitar

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

## Visión General de la Arquitectura

ANIMA sigue una arquitectura hexagonal / puertos-y-adaptadores.

```
┌────────────────────┐
│  Módulos de Entrada│  (texto, voz, discord, sensores)
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│   Motor ANIMA      │
│                    │
│  - Razonamiento    │
│  - Planificación   │
│  - Sistema de      │
│    Tareas          │
│  - Capas de        │
│    Memoria         │
│  - Seguridad       │
│  - Capacidades     │
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│    Adaptadores     │  (Conversión de intención a comando)
└────────────────────┘
          │
┌─────────▼──────────┐
│ Módulos de         │  (herramientas, streaming, robots, etc)
│ Capacidad          │
└────────────────────┘
```

El motor **no tiene conocimiento** de plataformas, personalidades o encarnación.

---

## El Sistema de Seeds

Un **Seed** define una identidad ANIMA.

Un seed es **datos**, no código.

### Un seed puede definir:

* Parámetros de personalidad (tono, verbosidad, expresividad emocional)
* Políticas de comportamiento (tolerancia al riesgo, manejo de incertidumbre)
* Listas de permitir/denegar capacidades
* Estilo de interacción predeterminado
* Marco narrativo inicial
* Reglas de decaimiento y promoción de memoria

### Un seed no debe incluir:

* Memorias aprendidas
* Registros de conversación
* Datos de usuario
* Lógica de ejecución
* Estado entre instancias

Después de la inicialización, cada instancia ANIMA desarrolla **memoria exclusiva de la instancia**.

---

## ANIMA Prime vs Instancias de Usuario

* **ANIMA Prime**
  Una identidad protegida utilizada para encarnación en streaming / VTuber.
  Implementada como:

  ```
  Motor ANIMA + Seed Prime
  ```

  El Seed Prime nunca se distribuye.

* **Instancias ANIMA de Usuario**
  Instancias privadas inicializadas con seeds seleccionados por el usuario.
  Los usuarios poseen la memoria y evolución de sus instancias.

Todas las instancias se ejecutan en el **mismo motor**, pero permanecen totalmente aisladas.

---

## Modelo de Memoria

ANIMA usa memoria en capas para equilibrar realismo, seguridad y costo.

Tipos de memoria, políticas de promoción y decaimiento especificadas en el [documento de integridad de memoria](safety/memory-integrity.md)

ANIMA rastrea si el conocimiento es:

* observado
* recordado
* inferido
* desconocido

Ella nunca adivina silenciosamente.

---

## Capacidades

Las capacidades son módulos explícitos y descubribles.

El motor:

* razona sobre capacidades
* verifica permisos
* solicita confirmación cuando es necesario
* ejecuta en un sandbox controlado

Los seeds pueden **restringir** capacidades, pero nunca otorgar nuevo poder de ejecución.

---

## Seguridad y Permisos

* Identidades de usuario estables
* Acceso basado en roles
* Verificaciones de permisos a nivel de capacidad
* Confirmación humana para acciones peligrosas
* Registro de auditoría completo

ANIMA explica *por qué* puede o no puede actuar.

---

## Lo que ANIMA No Es

* ❌ Un agente autónomo de propósito general
* ❌ Una IA conectada a internet por defecto
* ❌ Un sistema de memoria compartida
* ❌ Una personalidad codificada en prompts
* ❌ Un reemplazo para el juicio humano

---

## Estado del Proyecto

ANIMA está en desarrollo activo.

Enfoque actual:

* Corrección del motor central
* Integridad de la memoria
* Límites de capacidades
* Validación de seeds

Este proyecto está evolucionando intencionalmente de forma lenta.

---

## Documentos de Diseño

Las decisiones arquitectónicas clave se rastrean como ADRs en [`/adr`](adr):

* ADR-001: Separación del Motor / Identidad a través del Sistema de Seeds
* ADR-002: Esquemas Auto-Validables con Restricciones Solo Internas
* ADR-003: Protocolo de Comunicación Core ↔ Módulo, Tipos de Módulo y Ciclo de Vida de Lease
* ADR-004: Observabilidad, Registro de Eventos y Trazabilidad de Ejecución
* ADR-005: Modelo de Interrupción y Preempción
* ADR-006: Reglas de Dependencia y Compartición de Dominio
* ADR-007: Capa de Infraestructura y Reglas de Adaptador
* ADR-008: ANIMA Core se Comporta como un Kernel Cognitivo
* ADR-009: Topología de Modelo de IA Configurable con Córtex Obligatorio y Arcuate Opcional
* ADR-010: División Adaptador–Actuador con Frontera de Proceso Estricta
* ADR-011: Arquitectura de Entrada Basada en Eventos

### Documentación de Arquitectura

Documentación integral de arquitectura derivada de los ADRs:

* [Visión General de Arquitectura](architecture/directory-overview.md) - Guía de navegación completa
* [Arquitectura ANIMA](architecture/anima-architecture.md) - Visión general integral del sistema
* [Sistema de Seeds](architecture/seed-system.md) - Inicialización y separación de identidad
* [Tipos de Módulo y Leases](architecture/module-types-and-leases.md) - Ciclo de vida y autorización de módulos
* [Arquitectura de Eventos](architecture/event-architecture.md) - Observabilidad y sistema de entrada
* [Kernel Cognitivo](architecture/cognitive-kernel.md) - Core como supervisor multitarea
* [Dominio e Infraestructura](architecture/domain-and-infrastructure.md) - Implementación de arquitectura hexagonal
* [Topología del Modelo de IA](architecture/ai-model-topology.md) - Córtex y Arcuate
* [División Adaptador-Actuador](architecture/adapter-actuator-split.md) - Estructura de módulo

### Documentos Fundamentales

Principios constitucionales y límites:

* [Glosario Canónico](foundations/canonical-glossary.md) - Terminología y conceptos definitivos
* [Límites del Sistema](foundations/system-boundaries.md) - Lo que ANIMA puede y no puede hacer

### Documentación Adicional

* [Visión](vision/vision.md) - Visión y objetivos a largo plazo
* [Carta del Proyecto](vision/project-charter.md) - Propósito central, valores y no-objetivos
* [No-Objetivos](vision/non-goals.md) - No-objetivos y límites explícitos
* [Integridad de la Memoria](safety/memory-integrity.md) - Gestión e integridad de la memoria
* [Modelo de Seguridad](safety/safety-model.md) - Arquitectura de seguridad
* [Modelo de Amenazas](safety/threat-model.md) - Análisis y mitigación de amenazas
* [Modelo de Licenciamiento](governance/licensing-model.md) - Licenciamiento y distribución
* [Roadmap](roadmaps/roadmap.md) - Hoja de ruta del desarrollo
* [Anuncios](announcements) - Anuncios y actualizaciones del proyecto
* [Especificaciones](specs) - Especificaciones técnicas

---

## Filosofía

ANIMA no está diseñada para parecer inteligente a toda costa.

Está diseñada para parecer **consistente, honesta y segura para crecer junto a ella**.

---

## Preguntas Frecuentes

**Para el FAQ completo, consulta [FAQ.md](es-mx/FAQ.md)**

### ¿Qué es ANIMA, exactamente?

ANIMA es un **motor de IA privado y modular** diseñado para alojar identidades artificiales de larga duración y en evolución bajo estrictas restricciones de seguridad, memoria y capacidades.

**No es**:
* un chatbot
* una personalidad única
* un agente autónomo que actúa por sí mismo

ANIMA es un *motor para cultivar identidades*, no un personaje preempaquetado.

### ¿Dónde puedo encontrar el motor ANIMA?

El motor ANIMA está en un repositorio separado llamado `engine-core`, que está actualmente **no público** mientras los límites centrales de seguridad, memoria e identidad están en desarrollo.

Este repositorio de documentación pública existe para explicar la arquitectura, documentar decisiones de diseño y hacer transparente la visión a largo plazo.

### ¿Cómo descargo o ejecuto ANIMA?

ANIMA **aún no está disponible para descarga**. Cuando se lance, se distribuirá como un runtime de motor donde los usuarios inicializan sus propias instancias privadas usando Seeds.

### ¿Qué es un "Seed"?

Un Seed es un **blueprint de identidad estructurado** usado en la inicialización. Define parámetros de personalidad, restricciones de comportamiento, capacidades permitidas y marco narrativo inicial—pero no incluye memorias, historial de conversaciones o comportamiento aprendido.

### ¿Es seguro usar ANIMA?

La seguridad es un **requisito arquitectónico central**. ANIMA impone permisos explícitos, listas de permitir/denegar capacidades, confirmación humana para acciones peligrosas, registro de auditoría y separación estricta entre identidad y ejecución.

### ¿Más Preguntas?

Consulta el [FAQ](es-mx/FAQ.md) completo para respuestas detalladas a preguntas comunes sobre arquitectura, seguridad, privacidad y desarrollo.

---

## Contribuyendo a la Documentación

¿Interesado en mejorar la documentación de ANIMA? Consulta [DOCUMENTATION_MAINTENANCE.md](es-mx/DOCUMENTATION_MAINTENANCE.md) para:

* Cómo proponer cambios en la documentación
* Cuándo crear nuevos ADRs vs. actualizar existentes
* Pautas de revisión de documentación
* Pautas de estilo y tono

---

**Vea también**: [Documentación completa en Inglés](../README.md)
