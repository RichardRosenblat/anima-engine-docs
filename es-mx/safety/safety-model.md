# SAFETY_MODEL.md

## Propósito

Este documento describe **cómo ANIMA aplica la seguridad mecánicamente**.

No define valores morales, teorías éticas o ideales de comportamiento.
La seguridad en ANIMA se logra a través de **restricciones explícitas, pasos de verificación y límites de ejecución** aplicados por el motor.

---

## Seguridad como una Propiedad del Motor

La seguridad en ANIMA **no está basada en prompts** y **no está basada en personalidad**.

Se aplica mediante:

* Control de capacidades
* Verificaciones de permiso
* Flujos de confirmación
* Validación epistémica
* Manejo explícito de incertidumbre
* Aislamiento de ejecución

Ningún componente es confiable por defecto — incluidos adaptadores, entradas o el modelo de lenguaje.

---

## Validación Epistémica

ANIMA rastrea la procedencia de todo conocimiento utilizado en la toma de decisiones.

### Estados de Conocimiento

Todo conocimiento se clasifica como uno de un conjunto de estados (puede evolucionar con el tiempo):

* **Observado:** Datos percibidos directamente o de entrada.
* **Recordado:** Recuperado de capas de memoria con metadatos de confianza.
* **Inferido:** Derivado a través del razonamiento con niveles de incertidumbre.

Todo conocimiento está etiquetado con su fuente y nivel de confianza.

### Comprendiendo significado y procedencia sin confianza ciega

ANIMA previene la confianza ciega total (ej: utilizar otros modelos de lenguaje como aplicadores) a través de técnicas de procesamiento de lenguaje natural y detección de no cumplimiento.

A través de estos mecanismos, ANIMA puede identificar:
* Contradicciones
* Inconsistencias
* Ambigüedades
* Cuando la confianza del conocimiento es insuficiente para una decisión

## Control de Capacidades

ANIMA no puede ejecutar acciones arbitrarias.

Todas las acciones deben mapear a un **módulo de capacidad declarado**.

### Reglas de Capacidad

* Las capacidades son:

  * Explícitas
  * Descubribles
  * Auditables

* Las capacidades deben ser:

  * Habilitadas por el motor
  * Permitidas por la seed activa
  * Permitidas para el rol del usuario actual

### Restricción Importante

> **Las Seeds pueden restringir capacidades, pero nunca otorgar nuevo poder de ejecución.**

Esto garantiza:

* Ninguna identidad puede exceder los límites rígidos del motor
* Los límites de licenciamiento y seguridad permanecen aplicables
* Las capacidades no pueden ser contrabandadas a través de personalidad o memoria

---

## Flujos de Confirmación

Algunas capacidades requieren **confirmación humana** antes de la ejecución.

### Evaluación de Nivel de Riesgo

ANIMA evalúa cada capacidad a lo largo de dos ejes independientes:

* **Nivel de Riesgo del Sistema** — Impacto potencial máximo a la integridad, seguridad, disponibilidad o límites de confianza del sistema si la operación se comporta incorrectamente o es abusada. (Ver [Glosario Canónico](../foundations/canonical-glossary.md))

* **Nivel de Riesgo del Usuario** — Daño potencial máximo a la privacidad, finanzas, reputación, autonomía o resultados irreversibles del usuario si la operación se ejecuta incorrectamente o sin comprensión completa. (Ver [Glosario Canónico](../foundations/canonical-glossary.md))

Estos niveles de riesgo expresan **severidad del impacto**, no probabilidad, y son independientes uno del otro.

### Cuándo Se Requiere Confirmación

Los requisitos de confirmación son determinados por el **Nivel de Riesgo del Usuario**:

* Operaciones de alto Nivel de Riesgo del Usuario (ej: transacciones financieras, acciones irreversibles)
* Operaciones que afectan la privacidad del usuario o datos personales
* Acciones con consecuencias externas significativas
* Operaciones donde la intención del usuario es ambigua

El Nivel de Riesgo del Sistema influye en sandboxing, aislamiento y mecanismos de aplicación, pero no desencadena directamente confirmación del usuario.

### Propiedades de Confirmación

* Explícita (no inferida)
* Limitada en tiempo
* Multi-factor cuando el Nivel de Riesgo del Usuario lo justifique
* Registrada
* Vinculada a una instancia de acción específica

Los adaptadores **no pueden auto-afirmar consentimiento**.
La confirmación debe ser validada por el motor a través de uno o más canales confiables.

---

## Sin Ejecución Directa desde Lenguaje Natural

El lenguaje natural **nunca desencadena ejecución directamente**.

El pipeline de ejecución es estrictamente:

```
Entrada → Interpretación → Intención → Propuesta de Capacidad → Validación → Confirmación → Ejecución
```

Esto previene:

* Inyección de prompt
* Ambigüedad lingüística causando acciones
* Adaptadores escalando intención
* Modos de falla "simplemente hazlo"

Si la intención no puede ser mapeada con confianza, **la ejecución no ocurre**.

---

## Las Alucinaciones Se Tratan como Fallas

ANIMA trata las alucinaciones como **errores del sistema**, no como peculiaridades estilísticas.

### Qué Cuenta como Alucinación

* Afirmar hechos sin evidencia suficiente
* Reclamar capacidades que no están disponibles
* Inventar memorias o permisos
* Adivinar la intención del usuario

### Aplicación

* El motor rastrea la procedencia del conocimiento:

  * observado
  * recordado
  * inferido

* Si la confianza es insuficiente:

  * ANIMA debe decirlo explícitamente
  * La acción es bloqueada o degradada

> Una mentira confiada se considera peor que una respuesta rechazada.

---

## Manejo de Incertidumbre

La incertidumbre está **explícitamente representada**, no oculta.

### Mecanismos

* Umbrales de confianza para clasificación de intención
* Clarificación requerida cuando la ambigüedad es alta
* Estados explícitos de "No sé"
* Rechazo cuando el riesgo de ejecución excede la certeza

La incertidumbre no se trata como falla —
**actuar sin reconocer la incertidumbre sí lo es.**

---

## Seguridad de Memoria

La memoria está restringida para prevenir inferencia insegura y deriva.

* Sin memoria entre instancias
* Sin promoción implícita de memoria
* La memoria narrativa requiere curación explícita
* Las Seeds son inmutables después de la inicialización

Esto previene:

* Corrupción de identidad
* Deriva silenciosa de personalidad
* Formación accidental de creencias

---

## Auditabilidad

Cada decisión relevante para la seguridad se registra:

* Verificaciones de capacidad
* Denegaciones de permiso
* Solicitudes de confirmación
* Intentos de ejecución
* Rechazos

Los registros están diseñados para responder:

> "¿Por qué ANIMA actuó — o rechazó — en este caso?"

---

## Lo Que el Modelo de Seguridad *No* Hace

Este modelo **no** intenta:

* Aplicar alineación moral
* Juzgar la intención del usuario más allá del riesgo de ejecución
* Prevenir el mal uso en hosts comprometidos
* Actuar como sandbox o enclave seguro
* Reemplazar la responsabilidad humana

Esas preocupaciones están explícitamente fuera del alcance.

---

## Resumen

El modelo de seguridad de ANIMA se basa en:

* Límites explícitos de capacidad
* Aplicación a nivel del motor
* Confirmación humana para riesgo
* Incertidumbre transparente
* Tratar las alucinaciones como errores

La seguridad no es comportamiento emergente.
Es **diseñada, aplicada y auditable**.
