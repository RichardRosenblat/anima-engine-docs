# Sistema de Seeds — Inicialización de Identidad en ANIMA

**ADR Relacionado:** [ADR-001: Separación del Motor ANIMA e Identidad vía Sistema de Seed](../adr/ADR-001.md)

---

## Visión General

El **Sistema de Seed** es el mecanismo de ANIMA para separar el motor de la identidad. Un seed es una configuración declarativa usada para inicializar una identidad ANIMA, asegurando que el motor permanezca agnóstico de identidad mientras permite personalidades diversas y personalizables.

**Conceptos Clave:**
* **ANIMA Seed** - Priors de identidad sin memoria (configuración portátil y declarativa)
* **ANIMA Identity** - Seed + Memory (persistente, evoluciona con el tiempo)
* **ANIMA Instance** - Ejecución en curso que aloja una Identity (efímera)

**Oración Canónica:** "Una ANIMA Instance ejecuta una ANIMA Identity derivada de un ANIMA Seed y Memorias."

---

## ¿Qué es un Seed?

Un **Seed** es un blueprint estructurado, orientado a datos, que define:

* Parámetros de personalidad (tono, verbosidad, expresividad emocional)
* Políticas comportamentales (tolerancia al riesgo, manejo de incertidumbre)
* Listas de allow/deny de capacidades
* Estilo de interacción predeterminado
* Auto-concepto inicial y encuadre narrativo
* Políticas de decaimiento y promoción de memoria

Un seed es **datos, no código**. Configura el motor sin modificar su lógica.

Un Seed es **sin memoria** - no contiene experiencias aprendidas, historial de conversación o comportamiento evolucionado.

---

## ¿Qué es una ANIMA Identity?

Una **ANIMA Identity** es una identidad materializada compuesta por:
* **ANIMA Seed** (priors de identidad y configuración)
* **Memory** (capas episódica, semántica y narrativa que evolucionan con el tiempo)

Una Identity es **persistente** entre ejecuciones de Instance y **evoluciona** a través de la experiencia.

---

## ¿Qué es una ANIMA Instance?

Una **ANIMA Instance** es una única ejecución en curso del motor ANIMA.

Una Instance es **efímera** - existe solo durante la ejecución y se destruye al apagarse.

Una Instance **aloja** una ANIMA Identity (Seed + Memory) y proporciona el ciclo de vida de ejecución.

---

## Lo que un Seed NO Debe Incluir

Los seeds son estrictamente datos de inicialización. NO DEBEN contener:

* Memorias aprendidas
* Datos del usuario
* Registros de conversación
* Evolución comportamental privada
* Lógica de ejecución
* Estado entre instancias

---

## Motor vs. Identidad

### Motor ANIMA

El motor es un runtime privado responsable de:

* Comprensión y razonamiento de intención
* Planificación de tareas y orquestación de tareas de larga duración
* MTL (subsistema de memoria; almacenamiento, recuperación, decaimiento, mecánicas de promoción)
* Descubrimiento, permisos y ejecución de capacidades
* Seguridad, flujos de confirmación y registro de auditoría
* Manejo de entrada/salida agnóstico de adaptador

El motor:

* No codifica personalidad
* No contiene tono comportamental codificado
* No almacena memoria entre instancias
* Impone todos los límites de seguridad y permisos

### Identidad ANIMA (vía Seed)

Cada instancia ANIMA se inicializa con un seed y evoluciona independientemente usando **solo memoria local de la instancia**.

Una vez inicializada:

* La instancia desarrolla sus propias memorias
* El comportamiento evoluciona dentro de las restricciones definidas por el seed
* No ocurre compartición o aprendizaje entre instancias

---

## Invariantes Arquitectónicas

Las siguientes restricciones ahora son invariantes arquitectónicas:

* El motor debe permanecer agnóstico de persona
* Los seeds pueden ajustar o restringir comportamiento pero no pueden extender poder de ejecución
* La imposición de capacidades ocurre exclusivamente en el nivel del motor
* La memoria está estrictamente en el ámbito de la instancia
* La evolución de identidad debe ser explícita y revisable (sin desviación silenciosa)

---

## Casos de Uso

### Producto ANIMA Privado

* Los usuarios licencian el runtime del motor
* Los usuarios inicializan con un seed elegido
* Los usuarios son propietarios de los datos y memoria de su instancia
* El código fuente del motor permanece privado

### ANIMA de Streaming / VTuber (ANIMA Prime)

* Implementado como `Motor ANIMA + Prime Seed`
* Prime seed es IP protegida y nunca distribuida
* No comparte memoria o estado de identidad con instancias de clientes

---

## Beneficios

* Permite múltiples instancias ANIMA independientes con lógica de motor compartida
* Soporta comercialización sin distribuir código fuente
* Previene fuga de identidad y memoria entre instancias
* Permite una identidad "Prime" protegida para uso en streaming / VTuber
* Hace que la personalidad sea totalmente orientada a datos y configurable
* Simplifica pruebas, actualizaciones y evolución del motor

---

## Trabajo Futuro

* Definir esquema de seed y estrategia de versionamiento
* Implementar validación de seed y verificaciones de compatibilidad
* Formalizar garantías de actualización del motor entre versiones de seed
* Definir mecanismos de licenciamiento y control de capacidades
