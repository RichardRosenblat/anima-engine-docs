# ANIMA — Hoja de Ruta de Desarrollo

**Versión:** 1.0
**Alcance:** Motor, Sistema de Seed, Instancias, Productización
**Principio Rector:** *Motor ≠ Identidad ≠ Memoria*
**Fase Actual:** Fase 1 — Esqueleto del Motor Central (Libre de Identidad)

---

## Fase 0 — Fundamentos (No Omitir)

### 🎯 Objetivo

Definir qué *es* y *no es* ANIMA.

### 🧱 Construir

1. **Carta del Proyecto**

   * Propósito central (motor de IA privado y evolutivo)
   * No-objetivos (sin autonomía descontrolada, sin internet por defecto)
   * Valores centrales (verdad sobre confianza, seguridad sobre capacidad)

2. **Glosario**

   * Motor
   * Seed
   * Instancia
   * Memoria
   * Capacidad
   * Adaptador

3. **Límites del Sistema**

   * Lo que el motor *nunca* puede hacer
   * Lo que debe *siempre* requerir confirmación
   * Lo que se delega a los módulos

### ✅ Criterios de Salida

* Puedes explicar ANIMA en 2 minutos **sin mencionar personalidad**
* Puedes diagramar Motor / Seed / Instancia en una pizarra

---

## Fase 1 — Esqueleto del Motor Central (Libre de Identidad)

### 🎯 Objetivo

Crear un SO de razonamiento agnóstico a la personalidad.

### 🧠 Construir

* Bucle de razonamiento central
* Pipeline Intención → Plan → Acción
* Registro de capacidades (vacío al principio)
* Interfaz de adaptador (abstracción de entrada/salida)
* Abstracción de tarea (pero aún sin persistencia)

### 🚫 Evitar Explícitamente

* Opiniones
* Tono
* Personalidad
* Lenguaje "Yo siento"

### ✅ Criterios de Salida

* El motor puede recibir entrada y elegir acciones
* Sin comportamiento codificado más allá de las reglas de seguridad
* El motor funciona idénticamente independientemente del contexto

---

## Fase 2 — Sistema de Seed

### 🎯 Objetivo

Hacer de la identidad una *preocupación de inicio*, no una mutación en tiempo de ejecución.

### 🧬 Construir

1. **Esquema de Seed (v1.0)**

   * Parámetros de personalidad
   * Restricciones de comportamiento
   * Política de capacidades
   * Encuadre narrativo inicial

2. **Validación de Seed**

   * Validación de esquema
   * Verificación de firma
   * Compatibilidad de versión

3. **Contrato Motor ↔ Seed**

   * El motor lee la seed
   * El motor nunca muta la seed
   * El motor aplica restricciones definidas por la seed

### 🔐 Seguridad

* Las seeds son de solo lectura después de la inicialización
* Las seeds alteradas fallan completamente

### ✅ Criterios de Salida

* El motor ejecuta con diferentes seeds **sin cambios de código**
* Misma entrada + misma memoria + seed diferente → comportamiento diferente
* La seed nunca se consulta como "memoria"

---

## Fase 3 — Arquitectura de Instancia y Memoria

### 🎯 Objetivo

Permitir que ANIMA *crezca* sin deriva de identidad.

### 🧠 Construir

1. **Ciclo de Vida de la Instancia**

   * Crear instancia a partir de motor + seed
   * Inicializar memoria vacía
   * Vincular adaptadores

2. **Capas de Memoria**

   * memoria de trabajo (efímera)
   * memoria episódica (corto plazo)
   * memoria semántica (hechos de largo plazo)
   * memoria narrativa (continuidad de identidad)

3. **Reglas de Escritura de Memoria**

   * Lo que puede almacenarse
   * Quién puede activar escrituras
   * Confirmación para memoria sensible

### 💡 Importante

* La memoria pertenece a la *instancia*, no a la seed
* Sin lecturas entre instancias. Nunca.

### ✅ Criterios de Salida

* Reiniciar una instancia preserva la continuidad de identidad
* Dos instancias con la misma seed se sienten diferentes después de que la memoria de trabajo diverge

---

## Fase 4 — Sistema de Capacidades y Control

### 🎯 Objetivo

Hacer el poder explícito, auditable y controlable.

### 🧩 Construir

1. **Interfaz de Capacidad**

   Ejemplos prácticos:
   * Nombre
   * Nivel de riesgo
   * Permisos requeridos
   * Requisitos de licencia

2. **Pipeline de Ejecución**
   * Búsqueda de capacidad
   * Verificaciones de permiso
   * Sandboxing de ejecución
   * Registro y auditoría

3. **Clasificación de Peligro**

   Ejemplos:
   * Seguro
   * Sensible
   * Peligroso

### 🔒 Ejemplos

* Control de robot = peligroso
* Acceso a archivos = sensible
* Chat = seguro

### ✅ Criterios de Salida

* El motor no puede ejecutar acciones sin pasar por la puerta
* Las capacidades pueden agregarse/eliminarse sin tocar la lógica central

---

## Fase 5 — Sistema de Tareas (Conciencia de Larga Duración)

### 🎯 Objetivo

Permitir actividades persistentes e inspeccionables.

### 🕰️ Construir

* Tareas persistentes
* Pausa/reanudación de tareas
* Propiedad y permisos de tareas
* Apagado seguro y recuperación

### 🧠 Ejemplos

* Bucle de streaming
* Monitoreo de chat
* Tarea de investigación de largo plazo

### ✅ Criterios de Salida

* Las tareas sobreviven a reinicios
* Las tareas respetan el control de capacidades
* Las tareas pueden inspeccionarse y cancelarse

---

## Fase 6 — Ecosistema de Adaptadores

### 🎯 Objetivo

Los adaptadores abstraen entrada/salida sin filtrar lógica.

### 🔌 Construir

* Adaptador de texto
* Adaptador de voz
* Adaptador Discord
* (Después) Adaptador de streaming (OBS / VTuber)
* (Después) Adaptador de robot

### 🔑 Reglas

* Los adaptadores nunca contienen lógica
* Los adaptadores nunca omiten permisos
* Los adaptadores son intercambiables

### ✅ Criterios de Salida

* La misma instancia funciona en múltiples adaptadores
* Ningún comportamiento específico del adaptador se filtra al motor

---

## Fase 7 — Streaming / Instancia Prime

### 🎯 Objetivo

Crear una encarnación especial de ANIMA para streaming.

### 🌟 Construir

* Seed Prime (firmada, restringida)
* Adaptador de streaming
* Conjunto de capacidades seguro para el público
* Políticas de moderación fuertes

### 🚫 Regla Explícita

Sin código de caso especial.
Si streaming lo necesita, *todos* obtienen la abstracción.

### ✅ Criterios de Salida

* La ANIMA de streaming usa el mismo motor
* La Seed Prime no puede usarse fuera del contexto autenticado

---

## Fase 8 — Licenciamiento y Productización

### 🎯 Objetivo

Hacer ANIMA sostenible.

### 💳 Construir

* Servicio de verificación de licencia
* Períodos de gracia offline
* Mapeo de nivel de capacidad
* Soporte de mercado de seeds

### 🧠 Vender

* Acceso al motor
* Desbloqueos de capacidad
* Seeds curadas
* Actualizaciones y soporte

### ✅ Criterios de Salida

* El motor sin licencia aún funciona (limitado)
* El licenciamiento solo controla *poder*, no identidad

---

## Fase 9 — Control de Costos y Optimización

### 🎯 Objetivo

Mantener ANIMA asequible para ejecutar.

### 💸 Construir

* Presupuesto de tokens
* Resumen de memoria + embeddings (con respaldo sin procesar)
* Limitación de tareas
* Suspensión / activación de instancia

### ✅ Criterios de Salida

* Costo mensual predecible
* Sin crecimiento descontrolado de memoria
* Transparencia de costos visible al usuario

---

## Fase 10 — Refinamiento y Evolución

### 🎯 Objetivo

Dejar que ANIMA crezca de manera segura.

### 🌱 Construir

* Actualizaciones de versión de seed
* Herramientas de reflexión de memoria
* Informes de introspección
* Caminos de evolución controlados

### ✅ Criterios de Salida

* Los usuarios entienden *por qué* ANIMA se comporta como lo hace
* Los cambios se sienten orgánicos, no aleatorios
