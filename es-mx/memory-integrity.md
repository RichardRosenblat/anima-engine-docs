# MEMORY_INTEGRITY.md

## Propósito

Este documento explica **cómo ANIMA recuerda** — y igualmente importante, **por qué olvida**.

La integridad de la memoria es central para la confianza.
El recuerdo perfecto, el almacenamiento ilimitado y la memoria sin filtrar no son características — son modos de falla.

ANIMA trata la memoria como un **sistema controlado**, no como un archivo de registro.

---

## Por Qué el Recuerdo Perfecto Es Malo

La confianza humana depende de la **continuidad**, no de la completitud.

El recuerdo perfecto crea varios problemas:

* ❌ Sobrecarga de contexto
* ❌ Falsa autoridad ("Recuerdo todo")
* ❌ Incapacidad de adaptarse
* ❌ Riesgo de privacidad
* ❌ Superficie aumentada de alucinación

Un sistema que recuerda *todo* también debe decidir:

* qué importa
* qué es relevante
* qué se puede inferir con seguridad

La mayoría de los sistemas hacen esto **implícitamente** y **silenciosamente**.

ANIMA no lo hace.

---

## La Memoria No Es una Transcripción

ANIMA **no** almacena conversaciones literalmente por defecto.

Los registros sin procesar son:

* ruidosos
* engañosos
* emocionalmente no ponderados
* peligrosos cuando se malinterpretan posteriormente

En su lugar, ANIMA usa **memoria en capas**, cada una con un propósito claro, alcance y ciclo de vida.

---

## Capas de Memoria

### 1. Memoria de Trabajo (Contexto Inmediato)

**Qué es:**

* Estado de tarea actual
* Objetivos activos
* Variables a corto plazo
* Planes en curso y uso de herramientas

**Propiedades:**

* Extremadamente efímera
* Activamente mutada
* Limpiada o reemplazada continuamente

**Por qué existe:**

* Permite razonamiento en múltiples pasos
* Soporta tareas de larga duración
* Permite secuenciación de acciones coherente

La memoria de trabajo es **puramente operacional**.
Nunca se persiste como memoria a largo plazo.

---

### 2. Memoria Episódica (Contexto de Interacción)

**Qué es:**

* Contexto de interacción de alta fidelidad
* Historial conversacional reciente
* Señales emocionales y situacionales locales

**Propiedades:**

* Volátil
* Vinculada a la sesión
* Automáticamente descartada o comprimida

**Por qué existe:**

* Permite flujo de conversación natural
* Evita preguntar constantemente
* Soporta coherencia a corto plazo

La memoria episódica **no es confiable a largo plazo**.

---

### 3. Memoria Semántica (Hechos y Preferencias)

**Qué es:**

* Hechos atómicos
* Preferencias del usuario
* Información estable y reutilizable

Almacenada usando:

* Entradas estructuradas
* Embeddings para recuperación
* Metadatos de confianza y procedencia

Cada elemento rastrea si fue:

* observado
* explícitamente declarado
* inferido
* incierto

**Por qué se usan embeddings:**

* Recuerdo eficiente
* Relevancia aproximada
* Control de costos

Los embeddings son **asistentes**, no autoritativos.

---

### 4. Memoria Narrativa (Continuidad de Identidad)

**Qué es:**

* Eventos curados y significativos
* Momentos que definen relaciones
* Compromisos y puntos de inflexión

La memoria narrativa es:

* Escasa
* Intencional
* Explícitamente promovida

Ejemplos:

* "El usuario confía en ANIMA con planificación a largo plazo."
* "Ocurrió un conflicto y fue resuelto."
* "Se estableció claramente un límite."

**Por qué importa la memoria narrativa:**

La identidad no se construye a partir de hechos — se construye a partir de *historias*.

Así es como ANIMA permanece **reconociblemente la misma entidad a lo largo del tiempo**.

---

## Cómo Se Usan los Resúmenes y Embeddings

ANIMA resume **para preservar significado, no redacción**.

### Resúmenes:

* Reducen ruido
* Capturan intención
* Preservan peso emocional
* Evitan detalles engañosos

### Embeddings:

* Permiten recuerdo aproximado
* Soportan recuperación basada en relevancia
* Previenen coincidencia frágil de palabras clave

Restricción importante:

> **Ni los resúmenes ni los embeddings se tratan como verdad absoluta.**

Son *ayudas de recuperación*, no la memoria misma.

---

## Resistencia a Alucinaciones a Través del Diseño de Memoria

Las alucinaciones frecuentemente provienen de:

* Recuerdo excesivamente confiado
* Resúmenes malinterpretados
* Procedencia ausente

ANIMA mitiga esto al:

* Rastrear confianza por elemento de memoria
* Distinguir hecho de inferencia
* Negarse a llenar vacíos silenciosamente
* Tratar el recuerdo incierto como incierto

Si la confianza de la memoria es demasiado baja:

* ANIMA pregunta
* o rechaza
* o dice "No sé"

---

## Deterioro de Memoria

Olvidar es intencional.

### Por Qué la Memoria se Deteriora

* Previene osificación de identidad
* Reduce suposiciones falsas
* Limita acumulación de errores a largo plazo
* Refleja dinámicas de confianza humanas

### Cómo se Deteriora la Memoria

* La memoria de trabajo es siempre efímera
* La memoria episódica siempre se deteriora
* La memoria semántica se deteriora a menos que se refuerce
* La memoria narrativa se deteriora más lentamente

El deterioro puede ser desencadenado por:

* Tiempo
* Inactividad
* Contradicción
* Corrección explícita

Nada se borra silenciosamente sin razón.

---

## Sin Memoria Entre Instancias

Cada instancia ANIMA posee su memoria.

* Sin grupos de memoria compartida
* Sin entrenamiento en datos del usuario
* Sin filtración de identidad

Lo que una ANIMA aprende permanece con esa ANIMA.

---

## Lo Que ANIMA Nunca Hará

ANIMA no hará:

* Reclamar recuerdo perfecto
* Inventar memorias
* Fusionar memorias entre usuarios
* Fingir que los resúmenes son transcripciones
* Usar memoria para anular correcciones del usuario

La memoria existe para apoyar confianza — no autoridad.

---

## Resumen

El sistema de memoria de ANIMA prioriza:

* Integridad sobre volumen
* Continuidad sobre completitud
* Transparencia sobre confianza
* Identidad sobre optimización

No está diseñada para recordar todo.

Está diseñada para recordar **lo que importa** — y ser honesta sobre el resto.
