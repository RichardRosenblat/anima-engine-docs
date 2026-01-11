# Especificación URN ANIMA

**Autoridad:** ANIMA  
**Versión:** 0.1.0

---

## 1. Propósito

Este documento define la estructura canónica, semántica y reglas para
**Nombres de Recursos Uniformes (URNs) ANIMA**.

Las URNs ANIMA proporcionan:
- Identificadores globalmente únicos
- Identidad estable e inmutable
- Autoridad semántica explícita
- Referencias de larga duración independientes de ubicación o implementación

Esta especificación es **normativa**.  
Todas las implementaciones DEBEN cumplir con las reglas definidas aquí.

---

## 2. Principios de Diseño

Las URNs ANIMA están diseñadas de acuerdo con los siguientes principios:

1. **Inmutabilidad**  
   Una vez emitida, una URN NUNCA DEBE cambiar de significado.

2. **Identidad Centrada en la Autoridad**  
   El significado está gobernado por el conjunto de reglas, especificaciones y suposiciones de confianza que definen cómo se interpreta una URN ANIMA, la autoridad ANIMA mencionada anteriormente, no por herramientas o código.

3. **Semántica Explícita**  
   Toda interpretación semántica DEBE ser derivable de la estructura URN y especificaciones asociadas.

4. **Contratos Versionados**
    Cada URN define una versión de la identidad que representa. Los cambios de versión implican semántica de identidad distinta.

5. **Identificadores Opacos**
    El segmento identificador final NO DEBE codificar semántica. Sirve únicamente como una referencia única.

6. **Especificación Central**
    Este documento es la única fuente de verdad para estructura y semántica de URN ANIMA. Las implementaciones de generadores son secundarias.

---

## 3. Formato Canónico

El formato canónico de URN ANIMA es:

```
urn:anima:<alcance>:<namespace>@<versión>:<id>
```

### 3.1 Visión General de los Segmentos

| Segmento    | Descripción |
|------------|-------------|
| `urn`      | Identificador de esquema URN estándar |
| `anima`    | Autoridad de namespace raíz |
| `alcance`  | Límite de confianza y ciclo de vida |
| `namespace`| Dominio de identidad semántica |
| `versión`  | Versión del contrato de identidad semántica |
| `id`       | Identificador único opaco |

---

## 4. Alcance

El segmento `alcance` define las **garantías de autoridad y ciclo de vida** del
recurso identificado.

Valores permitidos:

- `core`  
  Identidades canónicas de primera clase reconocidas por el propio runtime ANIMA.

- `module`  
  Identidades propiedad de extensiones, plugins o módulos externos.

Ejemplos:

```
urn:anima:core:seed@0.1.0:...
urn:anima:module:CLI.capability@0.2.1:...
```

---

## 5. Namespace

El segmento `namespace` define el **dominio semántico** de la identidad.

### 5.1 Reglas

- DEBE ser minúscula
- DEBE usar caracteres alfanuméricos y puntos (`.`)
- NO DEBE contener dos puntos (`:`)
- DEBERÍA reflejar jerarquía conceptual, no estructura de implementación

### 5.2 Ejemplos

```
seed
cortex.reasoning
schema.intent
runtime.instance
```

---

## 6. Versionado

El segmento `versión` especifica el **contrato de identidad semántica** que gobierna
la interpretación de la URN.

### 6.1 Formato de Versión

Las versiones DEBEN seguir versionado semántico estricto:

```
<major>.<minor>.<patch>
```

Ejemplo:
```
0.0.2
1.0.0
2.3.1
```

Si el versionado no tiene significado semántico, use `0.0.0`.


### 6.2 Significado Semántico

Dentro de una URN:

- CUALQUIER cambio de versión representa una **semántica de identidad distinta**
- La compatibilidad NO DEBE inferirse solo de los números de versión
- Incluso cambios a nivel de patch (`0.0.1` → `0.0.2`) implican una nueva clase de identidad

Las relaciones de compatibilidad, si las hay, DEBEN declararse explícitamente fuera
de la URN.

---

## 7. Identificador (`id`)

El segmento final es un **identificador único opaco**.

### 7.1 Formas Permitidas

- UUID v4

El identificador DEBE:
- Ser globalmente único
- Ser opaco y no derivable
- NO codificar semántica, estructura, versionado o información de contenido
- NO ser determinísticamente derivado del recurso que identifica

Hashes criptográficos, checksums o cualquier identificador derivado del contenido del recurso
NO DEBEN usarse como identificadores URN, ya que violan el requisito de opacidad.

### 7.2 Justificación (Informativa)

Los identificadores opacos garantizan que:
- La identidad es independiente de la representación
- Los identificadores permanecen estables a pesar de los cambios en el recurso
- No se puede inferir ninguna suposición del identificador mismo

Ejemplos:

```
6f9619ff-8b86-d011-b42d-00cf4fc964ff
9a31c4b2-4f2d-4e9e-a7f9-8b32d3e1f9a1
```

---

## 8. Reglas de Inmutabilidad

Una vez emitida:

- Una URN DEBE permanecer válida indefinidamente
- Una URN NO DEBE ser reutilizada
- Una URN NO DEBE cambiar de significado
- Una URN NO DEBE ser reemitida con semántica diferente

Cualquier evolución semántica requiere emitir una **nueva URN** con una nueva versión.

---

## 9. Implementaciones de Generadores

Los generadores de URN son **implementaciones de esta especificación**, no
definiciones autoritativas.

- Los números de versión de generadores NO DEBEN aparecer en URNs
- Múltiples generadores PUEDEN coexistir
- Todos los generadores DEBEN producir URNs conformes con este documento

Esta especificación es la única fuente de verdad.

---

## 10. Validación (Informativa)

Una URN ANIMA válida DEBE coincidir con el siguiente patrón *conceptual*:

```
urn:anima:(core|module):[a-z0-9.]+@[0-9]+.[0-9]+.[0-9]+:<opaque-id>
```

---

## 11. Ejemplos

### Instancia de Seed Central
```
urn:anima:core:seed@0.0.2:6f9619ff-8b86-d011-b42d-00cf4fc964ff
```

### Córtex de Razonamiento Central
```
urn:anima:core:cortex.reasoning@0.1.0:9a31c4b2-4f2d-4e9e-a7f9-8b32d3e1f9a1
```

### Identidad de Esquema
```
urn:anima:core:schema.intent@1.0.0:ba3bf79c-381e-409b-9450-12e241100d3a
```

---

## 12. Extensiones Futuras

Las revisiones futuras PUEDEN definir:
- Declaraciones de compatibilidad explícitas
- Reglas de vinculación de firma
- Mecanismos de resolución
- Federación entre autoridades

Tales extensiones NO DEBEN violar las garantías de inmutabilidad definidas aquí.

---

## 13. Notas Finales

Las URNs ANIMA son **contratos de identidad**, no referencias, no ubicaciones,
y no artefactos de implementación.

Existen para preservar significado a través del tiempo, sistemas e interpretaciones.
