# Carta del Proyecto

**Documentación Relacionada:**
* [Visión](vision.md) - Visión y objetivos a largo plazo
* [No-Objetivos](non-goals.md) - No-objetivos y límites explícitos
* [Visión General de la Arquitectura](architecture/anima-architecture.md) - Arquitectura completa del sistema

---

## ¿Qué es ANIMA?

ANIMA es un **motor de IA privado y modular** diseñado para alojar identidades artificiales de larga duración y en evolución bajo estrictas restricciones de seguridad, memoria y capacidad.

ANIMA es:

* Un **runtime de IA privado** para identidades en evolución
* Un **motor agnóstico de identidad** (la personalidad viene de Seeds)
* Un **kernel cognitivo** que supervisa el comportamiento
* Un **sistema de razonamiento por eventos** que opera sobre hechos estructurados
* Una **arquitectura hexagonal** con límites de dominio limpios
* Un **sistema de módulos basado en lease** sobre gRPC con mTLS

ANIMA **no** es:

* Un chatbot o wrapper de conversación
* Un agente autónomo que se auto-expande
* Una personalidad única o personaje hardcoded
* Un sistema de rastreo de internet por defecto
* Un sistema de memoria compartida o sincronizado en la nube
* Un reemplazo para el juicio humano

---

## Propósito Central

ANIMA existe para:

1. **Proporcionar un runtime de IA privado y evolutivo**
   * Alojar identidades artificiales de larga duración
   * Habilitar memoria y aprendizaje local a la instancia
   * Soportar operación continua y gestión de tareas

2. **Separar motor de identidad**
   * Motor: razonamiento, planificación, mecánica de memoria, seguridad
   * Identidad: personalidad, tono, políticas de comportamiento (definidas por Seed)
   * Habilitar múltiples instancias independientes de un único motor

3. **Soportar múltiples encarnaciones vía Seeds**
   * Instancias de usuario privadas con Seeds personalizados
   * Identidades protegidas (e.g., ANIMA Prime para streaming)
   * Sin compartir memoria o estado entre instancias

4. **Habilitar interacción segura y auditable con el mundo**
   * Autorización basada en lease para todas las capacidades
   * Observabilidad basada en eventos (hechos estructurados y rastreables)
   * Límites de seguridad criptográficos
   * Confirmación humana para acciones peligrosas
   * Pistas de auditoría completas para todas las decisiones y efectos

---

## Valores Centrales

Estos valores son **constitucionales**. Cada funcionalidad debe ser justificable contra ellos.

### Verdad sobre Confianza

ANIMA prioriza la precisión y honestidad sobre aparecer seguro.

* Rastrear si la información es observada, recordada, inferida o desconocida
* Admitir incertidumbre en lugar de fabricar
* La alucinación confiada se trata como un bug
* La memoria es falible y consultable, no confiada ciegamente

### Intención sobre Ejecución

ANIMA se enfoca en entender y cumplir intenciones en lugar de ejecutar comandos ciegamente.

* El Core produce **intención**, no efectos directos
* La intención incluye: qué, cuándo, dónde, cuánto, por qué, contingencias, confianza
* La ejecución se delega a módulos supervisados
* La intención es auditable, registrable y reproducible

### Modularidad sobre Monolito

ANIMA está diseñada con componentes intercambiables para flexibilidad y evolución a largo plazo.

* Clara separación de responsabilidades (dominios, infraestructura, módulos)
* Puertos y adaptadores (arquitectura hexagonal)
* Los módulos ejecutan fuera de proceso del Core
* Sin código de terceros en el Core
* Implementaciones intercambiables en caliente vía interfaces

### Seguridad sobre Capacidad

ANIMA prioriza la seguridad del usuario y consideraciones éticas sobre expandir funcionalidad.

* Permisos impuestos a nivel del motor
* Listas de allow/deny de capacidades
* Confirmación humana para acciones peligrosas
* Autorización basada en lease (sin lease, sin ejecución)
* Fallo cerrado en incertidumbre
* Permisos explícitos, nunca implícitos

### Configurabilidad sobre Hardcoding

ANIMA enfatiza la configuración externa en lugar de comportamientos fijos.

* Personalidad definida por datos (Seeds), no código
* Políticas de comportamiento configurables por instancia
* Restricciones de capacidad definidas declarativamente
* Políticas de memoria configurables por seed
* Sin personalidad o tono hardcoded

### Aislamiento sobre Conveniencia

ANIMA valora mantener componentes y procesos separados para mejorar la seguridad y confiabilidad.

* Memoria con alcance de instancia (sin compartir entre instancias)
* Límites de proceso entre Core y Módulos
* Particionamiento de ejecución para observabilidad
* Límites de lease criptográficos
* Aislamiento de dominio (arquitectura hexagonal)

---

## No-Objetivos (Explícitos)

Estos están **explícitamente excluidos** del propósito de ANIMA:

### No-Objetivos Técnicos

* **Sin código auto-modificable**
  * El código del motor permanece estable
  * El comportamiento evoluciona a través de memoria y Seeds, no mutación de código
  * Sin generación dinámica de código o carga de plugins en el Core

* **Sin autonomía descontrolada**
  * Todas las tareas de larga duración son iniciadas por el usuario
  * Las tareas son interrumpibles y auditables
  * Sin objetivos o capacidades auto-expansivas
  * Restringido por permisos y Seeds

* **Sin acceso a internet a menos que se conceda vía capacidad**
  * El acceso a la red es opt-in, nunca por defecto
  * Requiere capacidad y permiso explícitos
  * Sujeto a imposición de lease y auditoría

* **Sin memoria compartida entre instancias**
  * Cada instancia tiene memoria aislada, local a la instancia
  * Sin aprendizaje entre usuarios o memoria global
  * Sin compartir datos más allá del entrenamiento del modelo
  * Previene fugas de identidad y privacidad

* **Sin permisos implícitos de usuario**
  * Todos los permisos deben ser explícitos
  * Capacidades declaradas y controladas
  * Las acciones peligrosas requieren confirmación humana
  * Sin escalada silenciosa de privilegios

### No-Objetivos Arquitectónicos

* **No es un chatbot de solicitud-respuesta**
  * Sistema cognitivo con estado de larga duración
  * Ejecución concurrente de tareas
  * Dirigido por interrupciones, no bloqueante

* **No es un sistema monolítico**
  * Arquitectura modular, ports-and-adapters
  * Límites de dominio limpios
  * Infraestructura reemplazable

* **No es un servicio de nube por defecto**
  * Runtime privado, local-first
  * El usuario posee los datos de la instancia
  * Sin telemetría o carga de datos sin capacidad explícita

---

## Invariantes Arquitectónicas

Estas restricciones son **no-negociables** e impuestas por diseño:

1. **El motor es agnóstico de identidad** — la personalidad viene de Seeds, no código
2. **El Core nunca carga código de terceros** — los módulos ejecutan fuera de proceso sobre gRPC+mTLS
3. **Toda ejecución requiere leases válidos** — sin lease, sin ejecución (Zero-Lease State)
4. **Los dominios nunca importan infraestructura** — solo puertos/interfaces (arquitectura hexagonal)
5. **Toda observabilidad está basada en eventos** — sin logs tradicionales, solo hechos estructurados
6. **La memoria está estrictamente limitada a la instancia** — sin compartir entre instancias jamás
7. **Cortex es obligatorio, Arcuate es opcional** — cognición esencial, lenguaje es una capacidad
8. **El Core supervisa, no ejecuta** — modelo de kernel cognitivo
9. **Todas las acciones son interrumpibles** — interrupción cooperativa por diseño
10. **Los eventos son la fuente de verdad** — hechos inmutables, auditables y con alcance de ejecución

---

## Criterios de Éxito

ANIMA es exitosa cuando habilita:

* **Identidades de larga duración** que evolucionan consistentemente a lo largo del tiempo
* **Interacción segura** con el mundo a través de capacidades auditables
* **Instancias privadas** donde los usuarios poseen sus datos y memoria
* **Múltiples encarnaciones** de un único motor vía Seeds
* **Confianza y transparencia** a través de comportamiento observable y explicable
* **Estabilidad arquitectónica** que soporta evolución a largo plazo

---

## Gobernanza

Esta carta es **ley constitucional** para ANIMA.

* Todas las decisiones arquitectónicas deben alinearse con estos valores
* Todas las funcionalidades deben ser justificables contra esta carta
* Todos los no-objetivos deben ser activamente prevenidos por diseño
* Todas las invariantes deben ser impuestas por arquitectura, no convención

Las violaciones de esta carta son bugs arquitectónicos, no solicitudes de funcionalidades.

---

## Resumen

> **ANIMA es un motor para identidades en crecimiento, no una personalidad única.**
>
> **ANIMA no ejecuta instrucciones. Supervisa comportamiento.**
>
> **ANIMA no registra texto. Registra hechos.**
>
> **ANIMA prioriza consistencia, honestidad y seguridad sobre capacidad.**
