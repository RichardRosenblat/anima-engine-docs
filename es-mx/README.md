# Documentación del Motor ANIMA

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

## Preguntas Frecuentes (FAQ)

### ¿Qué es ANIMA, exactamente?

ANIMA es un **motor de IA privado y modular** diseñado para alojar identidades artificiales de larga duración y en evolución bajo estrictas restricciones de seguridad, memoria y capacidades.

Lo que esto significa es que puedes crear tu propia identidad de IA única proporcionando un **Seed** y dándole capacidades específicas, asegurando al mismo tiempo que opere de forma segura y respete tu privacidad.

**No es**:

* un chatbot
* una personalidad única
* un agente autónomo que actúa por sí mismo

ANIMA es un *motor para cultivar identidades*, no un personaje preempaquetado.

---

### ¿Dónde puedo encontrar el motor ANIMA?

El motor ANIMA está en un **repositorio separado** llamado `engine-core`.

Ese repositorio está actualmente **no público** mientras los límites centrales de seguridad, memoria e identidad aún están en desarrollo.

Este repositorio de documentación pública existe para:

* explicar la arquitectura
* documentar decisiones de diseño
* hacer transparente la visión a largo plazo

Una vez que el motor alcance fases estables, más partes del ecosistema pueden abrirse.

---

### ¿Cómo descargo o ejecuto ANIMA?

En este momento, ANIMA **aún no está disponible para descarga**.

Cuando se lance:

* ANIMA se distribuirá como un runtime de motor
* los usuarios inicializarán sus propias instancias privadas usando seeds aprobados
* la memoria siempre permanecerá local de la instancia

Los detalles sobre instalación y plataformas compatibles se publicarán más cerca del lanzamiento.

---

### ¿ANIMA será gratis una vez lanzado?

El plan actual es un **modelo mixto**:

* Algunos componentes (documentación, formatos de seed, adaptadores) pueden ser públicos
* Ejecutar el propio motor ANIMA probablemente requerirá una licencia o suscripción
* Ciertos seeds curados pueden ser premium

ANIMA está diseñada como un **sistema a largo plazo**, no una aplicación única, y la sostenibilidad es una consideración central.

El precio exacto aún no está finalizado.

---

### ¿Es seguro usar ANIMA?

La seguridad es un **requisito arquitectónico central**, no una idea tardía.

ANIMA impone:

* permisos explícitos
* listas de permitir/denegar capacidades
* confirmación humana para acciones peligrosas
* registro de auditoría
* separación estricta entre identidad y ejecución

Si ANIMA no puede realizar una acción de forma segura, debe explicar *por qué* y negarse.

---

### ¿Puede ANIMA eliminar mis archivos?

No, a menos que lo permitas explícitamente.

ANIMA:

* **no tiene acceso a tu sistema por defecto**
* no puede ejecutar capacidades que no posee
* no puede escalar permisos por sí misma

Cualquier capacidad que pueda afectar archivos, dispositivos o sistemas debe ser:

1. explícitamente instalada
2. explícitamente permitida por el seed
3. explícitamente permitida por el usuario por al menos dos canales separados
4. confirmada en tiempo de ejecución si es peligrosa

---

### ¿Puede ANIMA espiarme, grabarme o acceder a internet?

No — no por defecto.

ANIMA:

* no tiene acceso a internet a menos que una capacidad lo proporcione
* no graba audio o video a menos que un adaptador esté instalado
* no comparte memoria entre instancias
* no sube tus datos a instancias de otros usuarios
* no te monitorea pasivamente a menos que se lo pidas explícitamente

Si ANIMA puede percibir algo, debe ser porque **conectaste explícitamente** esa entrada y le dijiste que la usara.

---

### ¿ANIMA comparte memoria entre usuarios?

No. Nunca.

Cada instancia ANIMA:

* tiene su propia memoria aislada
* evoluciona independientemente
* no puede acceder o influir en otras instancias

No hay aprendizaje entre usuarios o "memoria global" compartida más allá del entrenamiento del modelo.

---

### ¿ANIMA alucinará o inventará cosas?

ANIMA está diseñada para **evitar alucinaciones silenciosas**.

El motor rastrea si la información es:

* observada
* recordada
* inferida
* desconocida

Si ANIMA no está segura, se espera que lo diga.

La precisión perfecta no está garantizada, pero **la fabricación confiada se trata como un error**.

---

### ¿Es ANIMA un agente autónomo?

No.

ANIMA no:

* se auto-implementa
* auto-modifica su núcleo
* persigue objetivos sin participación del usuario
* actúa sin intención y permiso explícitos

Las tareas de larga duración (como streaming o monitoreo) existen, pero son:

* iniciadas por el usuario
* interrumpibles
* auditables
* limitadas por permisos

---

### ¿Qué es un "Seed"?

Un seed es un **modelo de identidad estructurado** usado solo en la inicialización.

Define:

* parámetros de personalidad
* restricciones de comportamiento
* capacidades permitidas
* marco narrativo inicial

Un seed **no** incluye:

* memorias
* historial de conversación
* comportamiento aprendido
* lógica de ejecución

Después de la inicialización, cada instancia cultiva su propia identidad a través de la experiencia.

---

### ¿Qué es ANIMA Prime?

ANIMA Prime es una **identidad interna protegida futura** utilizada para encarnación en streaming / VTuber.

Se ejecutará en el mismo motor que todas las demás instancias, pero usa un seed privado y no distribuido.

ANIMA Prime no se venderá ni se compartirá.

---

### ¿Es ANIMA de código abierto?

Partes del ecosistema ANIMA pueden ser abiertas.

El núcleo del motor en sí está actualmente **cerrado** para asegurar:

* seguridad de identidad
* integridad de la memoria
* evolución controlada de capacidades

Esto puede cambiar con el tiempo, pero el motor nunca se lanzará de una manera que comprometa la confianza o privacidad del usuario.

---

### ¿Cuándo se lanzará ANIMA?

ANIMA está en desarrollo activo, pero no hay **fecha de lanzamiento fija** todavía.

El enfoque actual es en:
* Corrección del motor central
* Integridad de la memoria
* Límites de capacidades
* Validación de seeds
* Características de seguridad

Los anuncios futuros se harán a medida que se alcancen hitos y se acerquen fases estables.
Las demostraciones se compartirán cuando sea apropiado.

---

### ¿Por qué el desarrollo es lento?

Intencionalmente.

ANIMA prioriza:

* corrección sobre velocidad
* seguridad sobre novedad
* confianza a largo plazo sobre demostraciones a corto plazo

Algunas cosas son fáciles de construir rápidamente — y muy difíciles de arreglar después.
ANIMA se está construyendo para durar.

---

### ¿Cómo puedo seguir el desarrollo?

Actualizaciones, decisiones arquitectónicas y filosofía están documentadas en este repositorio.

Los hitos importantes se anunciarán una vez que el motor alcance fases estables.

Los anuncios se agregarán en la carpeta `/announcements` y se compartirán a través de canales oficiales futuros.

---

**Vea también**: [Documentación completa en Inglés](../README.md)
