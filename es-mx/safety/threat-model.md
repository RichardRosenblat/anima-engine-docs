# ANIMA — Modelo de Amenazas

## Propósito

Este documento delinea el **modelo de amenazas explícito** para ANIMA.

Define:

* Contra qué está diseñada ANIMA para defenderse
* Lo que ANIMA explícitamente *no* intenta defender
* Las suposiciones hechas sobre el entorno del host

Esto no es una afirmación de seguridad perfecta.
Es una declaración de **límites y responsabilidad intencionales**.

---

## Visión General del Sistema (Contexto de Seguridad)

ANIMA es un **motor de IA privado y modular** ejecutándose dentro de un entorno controlado por el usuario.

Características clave:

* Memoria local de la instancia
* Ejecución controlada por capacidades
* Sin acceso a internet por defecto
* Sin estado global compartido
* Confirmación humana explícita para acciones riesgosas

ANIMA asume **ninguna confianza implícita** en entradas, adaptadores o capacidades.

---

## Activos a Proteger

ANIMA prioriza la protección de los siguientes activos:

1. **Memoria de la Instancia**

   * Conversaciones del usuario
   * Preferencias aprendidas
   * Continuidad de identidad narrativa

2. **Integridad de la Seed**

   * Restricciones de identidad
   * Políticas de capacidad
   * Límites de comportamiento

3. **Límites de Capacidad**

   * Prevención de ejecución no autorizada
   * Aplicación de verificaciones de permiso

4. **Intención del Usuario**

   * Representación precisa de solicitudes del usuario
   * Protección contra acciones no intencionadas

5. **Auditabilidad**

   * Rutas de razonamiento claras
   * Trazabilidad de acciones

---

## Amenazas Contra las Que ANIMA Se Defiende

### 1. Fuga de Datos Entre Instancias

**Amenaza:**
Fuga de información de memoria o identidad entre instancias ANIMA.

**Mitigación:**

* Aislamiento estricto de instancias
* Sin capas de memoria compartida
* Sin grupo de aprendizaje o entrenamiento global
* Prohibición explícita de lecturas entre instancias

---

### 2. Escalación de Capacidad

**Amenaza:**
Un usuario, adaptador o módulo intentando ejecutar acciones más allá de las capacidades autorizadas.

**Mitigación:**

* Registro centralizado de capacidades
* Verificaciones de permiso obligatorias
* Restricciones de capacidad a nivel de Seed
* Confirmación humana para acciones sensibles o peligrosas

---

### 3. Inyección de Prompt y Manipulación de Entrada

**Amenaza:**
Entrada maliciosa intentando anular el comportamiento, políticas o restricciones de seguridad.

**Mitigación:**

* Separación de razonamiento y ejecución
* Aplicación no textual de reglas de seguridad
* Restricciones de Seed aplicadas fuera de prompts
* Sin ejecución directa desde lenguaje natural

---

### 4. Autoridad o Conocimiento Alucinado

**Amenaza:**
ANIMA presentando suposiciones o fabricaciones como hechos, especialmente en contextos de alto riesgo.

**Mitigación:**

* Seguimiento explícito de incertidumbre
* Etiquetas de procedencia de conocimiento (observado / inferido / desconocido)
* Rechazo a actuar con información incierta cuando la seguridad se ve afectada

---

### 5. Deriva Silenciosa de Comportamiento

**Amenaza:**
Cambios de identidad ocurriendo sin visibilidad o explicación.

**Mitigación:**

* Seeds de solo lectura después de la inicialización
* Reglas de promoción de memoria
* Curación de memoria narrativa
* Cambios de estado inspeccionables

---

### 6. Observación o Monitoreo No Autorizado

**Amenaza:**
ANIMA observando o grabando usuarios o entornos sin consentimiento.

**Mitigación:**

* Sin detección pasiva
* Requisitos de permiso a nivel de adaptador
* Iniciación explícita del usuario para observación
* Visibilidad clara de las entradas activas

---

## Amenazas Explícitamente Fuera del Alcance

ANIMA **no** intenta defenderse contra lo siguiente:

### 1. Compromiso del Sistema Host

Si el sistema operativo host, contenedor o hardware está comprometido, ANIMA asume:

* La confidencialidad de la memoria no puede garantizarse
* Los límites de capacidad pueden ser evadidos

ANIMA no es un sandbox o enclave seguro.

---

### 2. Modificación Maliciosa del Motor

Si el código fuente o binarios del motor ANIMA son modificados:

* Todas las garantías quedan anuladas
* Los mecanismos de seguridad pueden ser deshabilitados

La integridad del código es responsabilidad del distribuidor y del host.

---

### 3. Ataques de Canal Lateral

ANIMA no protege contra:

* Ataques de temporización
* Análisis de energía
* Canales laterales de hardware

Estos están fuera de la superficie de amenaza pretendida.

---

### 4. Extracción Adversaria de Modelo

ANIMA no intenta prevenir:

* Inversión de modelo
* Extracción de pesos
* Sondeo estadístico de modelos subyacentes

El motor trata el modelo subyacente como un componente reemplazable.

---

### 5. Ingeniería Social Humana

ANIMA no puede defender completamente contra:

* Usuarios convenciendo a otros humanos de usar indebidamente el sistema
* Confianza depositada fuera de las garantías del sistema

El comportamiento humano sigue siendo una responsabilidad humana.

---

## Suposiciones Sobre el Entorno del Host

ANIMA asume:

* El SO host aplica aislamiento de procesos
* Los permisos del sistema de archivos son respetados
* El acceso a la red es controlado externamente
* Los secretos (claves, licencias) se almacenan de manera segura
* Los adaptadores no son maliciosos por defecto

Violar estas suposiciones invalida partes de este modelo de amenazas.

---

## Filosofía de Diseño

ANIMA favorece:

* **Denegación explícita sobre falla silenciosa**
* **Auditabilidad sobre opacidad**
* **Capacidad limitada sobre poder máximo**

La seguridad se logra a través de **restricciones arquitectónicas**, no a través de confianza en inteligencia o intención.

---

## Declaración Final

Este modelo de amenazas es intencionalmente conservador.

ANIMA no intenta resolver todos los problemas de seguridad.
Intenta resolver **los problemas correctos**, de manera clara y honesta.

Al documentar lo que está en el alcance y lo que no, ANIMA permanece como un sistema sobre el cual los usuarios pueden razonar, confiar y crecer junto a él.
