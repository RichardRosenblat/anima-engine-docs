# ANIMA — Preguntas Frecuentes

Este documento responde preguntas comunes sobre ANIMA. Para detalles arquitectónicos más profundos, consulta la documentación canónica enlazada.

---

## Preguntas Generales

### ¿Qué es ANIMA, exactamente?

ANIMA es un **motor de IA privado y modular** diseñado para alojar identidades artificiales de larga duración y evolutivas bajo estrictas restricciones de seguridad, memoria y capacidades.

ANIMA te permite crear tu propia identidad de IA única al proporcionar un **Seed** (configuración declarativa) y otorgar capacidades específicas, mientras asegura que el sistema opere de manera segura y respete tu privacidad.

**ANIMA es:**
* Un motor para cultivar identidades a lo largo del tiempo
* Privado por diseño (cada instancia posee su propia memoria)
* Seguridad primero (control de capacidades, autorización basada en leases)
* Modular (entradas, salidas y capacidades son conectables)

**ANIMA no es:**
* Un chatbot o wrapper conversacional
* Una personalidad única y fija
* Un agente autónomo que actúa independientemente
* Un sistema de memoria compartida o mente colmena

**Conceptos Clave:**
Para definiciones completas, consulta [Glosario Canónico](foundations/canonical-glossary.md):
* **ANIMA Instance** - Una ejecución en curso del motor (efímera)
* **ANIMA Identity** - Seed + Memory (persistente, evoluciona con el tiempo)
* **ANIMA Seed** - Priors de identidad sin memoria (sin memoria, portátil)

**Relacionado:** [Vision](vision/vision.md), [ANIMA Architecture](architecture/anima-architecture.md)

---

### ¿Dónde puedo encontrar el Engine de ANIMA?

El Engine de ANIMA vive en un repositorio separado llamado `engine-core`.

Ese repositorio actualmente **no es público** mientras los límites centrales de seguridad, memoria e identidad están en desarrollo.

Este repositorio público de documentación existe para:
* Explicar la arquitectura de manera transparente
* Documentar decisiones de diseño
* Hacer que la visión a largo plazo sea revisable

Una vez que el Engine alcance fases estables, más partes del ecosistema podrían abrirse.

**Relacionado:** [Roadmap](roadmaps/roadmap.md)

---

### ¿Cómo descargo o ejecuto ANIMA?

En este momento, ANIMA **aún no está disponible para descarga**.

Cuando se lance:
* ANIMA se distribuirá como un runtime de motor
* Los usuarios inicializarán sus propias instancias privadas usando Seeds aprobados
* La memoria siempre permanecerá local a la instancia
* Las capacidades deben ser otorgadas explícitamente

Los detalles sobre instalación y plataformas compatibles se publicarán más cerca del lanzamiento.

**Relacionado:** [Project Viability](governance/project-viability.md)

---

### ¿ANIMA será gratuito una vez lanzado?

El plan actual es un **modelo mixto**:

* **ANIMA Engine (Runtime):** Código abierto (probablemente MIT o Apache 2.0)
* **ANIMA Prime Seed:** Disponible bajo términos específicos (por determinar)
* **Seeds de terceros:** Creados y distribuidos por usuarios o proveedores

El objetivo es hacer que el motor esté disponible gratuitamente mientras se permite diversidad de identidades y autonomía del usuario.

**Relacionado:** [Licensing Model](governance/licensing-model.md)

---

## Preguntas Conceptuales

### ¿Qué es un "Seed"?

Un **Seed** es un archivo de configuración estático y declarativo que define una identidad de ANIMA.

**Un Seed contiene:**
* Parámetros de personalidad (tono, verbosidad, expresividad emocional)
* Políticas de comportamiento (tolerancia al riesgo, manejo de incertidumbre)
* Listas de capacidades permitidas/denegadas
* Estilo de interacción por defecto
* Auto-concepto inicial y marco narrativo
* Políticas de degradación y promoción de memoria

**Un Seed NO debe contener:**
* Memorias aprendidas
* Registros de conversaciones
* Datos de usuario
* Lógica de ejecución
* Estado entre instancias

Después de la inicialización, cada instancia de ANIMA desarrolla **memoria local a la instancia únicamente**. El Seed proporciona la configuración inicial, no la experiencia continua.

**Relacionado:** [Seed System](architecture/seed-system.md), [ADR-001](adr/ADR-001.md)

---

### ¿Qué es ANIMA Prime Identity?

**ANIMA Prime Identity** es una **identidad especial y protegida** diseñada exclusivamente para uso público en el Motor ANIMA.

A diferencia de las identidades de usuario, ANIMA Prime Identity está diseñada para ser:
* **Orientada al público** - usada para streaming, encarnación VTuber, demostraciones de referencia
* **Protegida** - el Seed y Memory nunca se distribuyen públicamente
* **No-exportable** - no puede ser clonada, bifurcada o instanciada por terceros

**ANIMA Prime Identity NO es:**
* Compartible (Seed y Memory son propiedad intelectual protegida)
* Clonable (no puede ser duplicada o bifurcada)
* Disponible para uso privado (restringida solo a contextos autorizados)
* Requerida para usar el motor

**Restricciones Críticas:**
* **Prime Seed NUNCA se comparte** - Permanece como propiedad intelectual protegida
* **Prime Memory NUNCA se exporta** - Las experiencias y evolución permanecen privadas
* **Sin instanciación por terceros** - Solo sistemas autorizados pueden cargar Prime Identity
* **No-transferible** - No puede ser migrada a Instancias de usuario

**Cada ANIMA Identity** (incluyendo Prime) está compuesta por **Seed + Memory** y evoluciona independientemente basándose en sus experiencias cuando se carga en una Instance.

**Las identidades de usuario** pueden crearse con Seeds, personalidades y políticas de comportamiento completamente diferentes.

**Relacionado:** [Seed System](architecture/seed-system.md), [Vision](vision/vision.md)

---

### ¿Cómo difiere ANIMA de ChatGPT, Claude u otros asistentes LLM?

ANIMA es arquitectónicamente diferente de los sistemas típicos de IA conversacional:

| Aspecto | Asistentes LLM Típicos | ANIMA |
|--------|------------------------|-------|
| **Identidad** | Personalidad fija por modelo | Configurable vía Seed, evoluciona con el tiempo |
| **Memoria** | Ventana de contexto efímera | Memoria persistente en capas con degradación consciente |
| **Seguridad** | Barreras basadas en prompts | Control arquitectónico de capacidades y leases |
| **Continuidad** | Se reinicia entre sesiones | Identidad continua de larga duración |
| **Privacidad** | Entrenamiento/ajuste fino compartido | Estrictamente local a la instancia, sin aprendizaje entre instancias |
| **Capacidades** | Permisos amplios e implícitos | Autorización explícita, declarativa y auditable |

ANIMA está optimizado para **continuidad de identidad a largo plazo, confianza a través de consistencia y límites estrictos de seguridad**, no solo respuestas que suenan inteligentes.

**Relacionado:** [Why Not an Agent?](vision/why-not-an-agent.md), [Vision](vision/vision.md)

---

### ¿Por qué ANIMA no puede simplemente ser reemplazado por un framework de agentes existente?

La mayoría de los frameworks de agentes optimizan para:
* Máxima capacidad y autonomía
* Completar tareas y eficiencia
* Utilidad inmediata

ANIMA optimiza para:
* Continuidad de identidad a largo plazo
* Confianza a través de consistencia y honestidad
* Límites estrictos de seguridad
* Relaciones privadas y evolutivas

**Diferencias arquitectónicas clave:**

1. **Identidad de Larga Duración vs. Sesiones Efímeras**
   * Agentes: Se reinician entre sesiones, simulan memoria mediante recuperación
   * ANIMA: Identidad continua y evolutiva con degradación intencional de memoria

2. **Engine ≠ Identity (Separación)**
   * Agentes: Personalidad integrada en prompts/entrenamiento
   * ANIMA: Engine e identidad completamente separados vía Seed System

3. **Seguridad como Arquitectura vs. Seguridad como Prompt**
   * Agentes: Barreras a través de prompts y ajuste fino
   * ANIMA: Control de capacidades, autorización basada en leases, prueba criptográfica para ejecución

ANIMA no es un agente más capaz; es una categoría diferente de sistema optimizado para confiabilidad sobre maximización de utilidad.

**Relacionado:** [Why Not an Agent?](vision/why-not-an-agent.md), [Safety Model](safety/safety-model.md)

---

### ¿Es ANIMA un sistema RAG (Retrieval-Augmented Generation)?

No. Aunque ANIMA tiene un sistema de memoria sofisticado, no es un sistema RAG.

**Sistemas RAG:**
* Recuperan documentos de fuentes externas
* Usan recuperación para aumentar el contexto de generación
* Típicamente sin estado entre sesiones

**Memoria de ANIMA:**
* Rastrea experiencias episódicas (qué sucedió)
* Mantiene conocimiento semántico (qué se conoce)
* Soporta memoria narrativa (continuidad de identidad)
* Usa memoria en capas con políticas de degradación intencionales
* Persiste entre sesiones como parte de la identidad

La memoria de ANIMA está **centrada en la identidad**, no centrada en documentos. Sirve a la continuidad y la confianza, no solo a la recuperación de información.

**Relacionado:** [Memory Integrity](safety/memory-integrity.md), [ADR-003](adr/ADR-003.md)

---

## Preguntas de Arquitectura

### ¿Qué significa "arquitectura hexagonal" para ANIMA?

ANIMA usa **arquitectura hexagonal** (también llamada puertos y adaptadores):

* El **Core** (Cognitive Kernel) no tiene conocimiento de plataformas, personalidades o encarnación
* Toda la E/S ocurre a través de **Módulos** (componentes fuera de proceso)
* **Adapters** traducen la intención a comandos específicos de la plataforma
* **Actuators** ejecutan comandos con efectos

Esto significa:
* El motor es agnóstico de plataforma
* Puedes conectar diferentes fuentes de entrada (texto, voz, Discord, sensores)
* Puedes conectar diferentes capacidades (herramientas, robots, servicios de streaming)
* El razonamiento central permanece idéntico independientemente de la encarnación

**Relacionado:** [ANIMA Architecture](architecture/anima-architecture.md), [ADR-002](adr/ADR-002.md)

---

### ¿Qué son las "capacidades" en ANIMA?

**Capabilities** son contratos declarativos que definen qué puede hacer una instancia de ANIMA.

Cada acción en ANIMA:
* Debe estar declarada en un contrato de capacidad
* Requiere autorización explícita (capability lease)
* Puede ser inspeccionada, denegada o revocada
* Deja un rastro de auditoría

**Las capacidades no son:**
* Otorgadas implícitamente
* Ocultas en prompts
* Habilitadas automáticamente
* Compartidas entre instancias

Los usuarios deben otorgar capacidades explícitamente. ANIMA no puede usar una capacidad sin un lease válido.

**Relacionado:** [ADR-004](adr/ADR-004.md), [Module Types and Leases](architecture/module-types-and-leases.md)

---

### ¿Cómo previene ANIMA las alucinaciones?

ANIMA trata la alucinación como un **error, no una característica**.

**Salvaguardas arquitectónicas:**
* Las consultas de memoria devuelven **segmentos limitados y verificados** (no volcados completos de contexto)
* El Core razona sobre eventos estructurados, no texto sin procesar
* Las acciones requieren **leases criptográficos** - las capacidades alucinadas fallan en la ejecución
* El conocimiento incierto se marca explícitamente (seguimiento de estado epistémico)
* La ejecución ocurre en módulos controlados por leases, no en el núcleo de razonamiento

ANIMA no puede ejecutar una acción para la cual no tiene permiso, incluso si "piensa" que puede. La arquitectura lo hace cumplir.

**La alucinación en respuestas** se mitiga a través de:
* Marcadores explícitos de incertidumbre
* Separación de especulación de afirmación
* Validación contra memoria y contratos de capacidades

**Relacionado:** [Safety Model](safety/safety-model.md), [Cognitive Kernel](architecture/cognitive-kernel.md)

---

## Preguntas de Seguridad y Privacidad

### ¿Cómo asegura ANIMA la seguridad?

ANIMA hace cumplir la seguridad **arquitectónicamente**, no a través de prompts:

1. **Control de Capacidades** - Ninguna acción sin autorización explícita
2. **Ejecución Basada en Leases** - Se requiere prueba criptográfica para todos los efectos
3. **Confirmación Humana** - Las acciones de alto riesgo requieren aprobación del usuario
4. **Aislamiento de Procesos** - Los módulos se ejecutan fuera de proceso, no pueden corromper el Core
5. **Límites de Ejecución** - El Core produce intención, no efectos
6. **Registro de Auditoría** - Todas las acciones son observables y rastreables

Las restricciones de seguridad están **impuestas a nivel del motor**, no delegadas a la personalidad o expectativa del usuario.

**Relacionado:** [Safety Model](safety/safety-model.md), [Threat Model](safety/threat-model.md)

---

### ¿Mis datos se comparten entre instancias de ANIMA?

**No.** Las instancias de ANIMA están **estrictamente aisladas**.

**Garantías de privacidad:**
* Cada instancia tiene memoria privada y local a la instancia
* Sin pool de aprendizaje compartido
* Sin acceso a memoria entre instancias
* Sin registros de conversaciones globales
* Sin extracción de datos de personalidad

Cuando ejecutas una instancia de ANIMA, **tú posees su memoria** y evolución. No aprende de otros usuarios, y otros usuarios no aprenden de ti.

**Relacionado:** [Non-Goals](vision/non-goals.md), [Memory Integrity](safety/memory-integrity.md)

---

### ¿Puede ANIMA actuar autónomamente en internet?

**No.** ANIMA está explícitamente **no** diseñado para operar como un agente autónomo en internet.

**Restricciones:**
* Sin navegación web libre por defecto
* Sin acciones en línea autodirigidas sin aprobación explícita
* Cualquier interacción en internet está controlada por capacidades
* Limitado por contexto y completamente auditable

La autonomía en línea sin supervisión se trata como un **comportamiento de alto riesgo**, no una característica.

**Relacionado:** [Non-Goals](vision/non-goals.md), [Safety Model](safety/safety-model.md)

---

### ¿ANIMA monitorea o vigila a los usuarios?

**No.** ANIMA no es un sistema de vigilancia.

**Prohibiciones:**
* Sin monitoreo pasivo de personas, conversaciones o entornos
* Sin grabación sin consentimiento explícito
* Sin agregación de datos de comportamiento entre usuarios o instancias
* Sin rastreo en segundo plano

El diseño de ANIMA prioriza **privacidad y localidad**. La observación solo ocurre cuando se inicia intencionalmente y está claramente delimitada.

**Relacionado:** [Non-Goals](vision/non-goals.md), [Safety Model](safety/safety-model.md)

---

## Preguntas de Desarrollo y Comunidad

### ¿Por qué el desarrollo es lento?

El desarrollo de ANIMA prioriza **corrección sobre velocidad**.

Los límites de seguridad, integridad de memoria y separación de identidad son fundamentales. Estos no pueden agregarse después sin comprometer la arquitectura.

El proyecto deliberadamente:
* Publica decisiones de arquitectura antes de la implementación
* Hace que la visión sea revisable antes del lanzamiento
* Construye restricciones de seguridad en el núcleo, no como adiciones posteriores
* Valora la viabilidad a largo plazo sobre demos tempranos

**Relacionado:** [Project Viability](governance/project-viability.md), [Vision](vision/vision.md)

---

### ¿Puedo contribuir a ANIMA?

El Engine de ANIMA (`engine-core`) actualmente es privado durante el desarrollo fundamental.

**Puedes contribuir a:**
* Este repositorio de documentación (proponer mejoras, corregir errores)
* Discusiones de diseño (cuando los canales de la comunidad abran)

Cuando el motor alcance fases estables, se publicarán directrices de contribución.

**Relacionado:** [DOCUMENTATION_MAINTENANCE.md](DOCUMENTATION_MAINTENANCE.md)

---

### ¿Cuándo se lanzará ANIMA?

No hay una fecha de lanzamiento fija.

ANIMA se lanzará cuando:
* Los límites centrales de seguridad estén validados
* La integridad de memoria sea comprobadamente sólida
* El control de capacidades sea criptográficamente seguro
* El Seed System sea estable

Apresurar el lanzamiento comprometería los principios fundamentales que hacen diferente a ANIMA de los sistemas existentes.

**Relacionado:** [Roadmap](roadmaps/roadmap.md), [Project Viability](governance/project-viability.md)

---

## Recursos Adicionales

* **Para nuevos lectores:** [GETTING_STARTED.md](GETTING_STARTED.md)
* **Para inmersión arquitectónica:** [ANIMA Architecture](architecture/anima-architecture.md)
* **Para revisión de seguridad:** [Safety Model](safety/safety-model.md), [Threat Model](safety/threat-model.md)
* **Para razón fundamental de diseño:** [ADRs](adr/) (lee en orden, ADR-001 hasta ADR-011)
* **Para terminología:** [Canonical Glossary](foundations/canonical-glossary.md)

---

**Última Actualización:** 28 de enero de 2026
