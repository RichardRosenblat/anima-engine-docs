# Sistema de Referencia con Alcance JSON (JSRS)

## 1. Visión General

El **Sistema de Referencia con Alcance JSON (JSRS)** es un protocolo de direccionamiento de ruta dinámico diseñado para entornos JSON modulares. Permite que las estructuras de datos se muevan, fusionen o aniden sin romper la lógica interna mediante el uso de navegación relativa y espacios de nombres definidos por el usuario.

---

## 2. Sintaxis y Delimitadores

Las referencias se incorporan dentro de cadenas usando la sintaxis de llave-porcentaje.

* **Envoltorio de Referencia:** `%{ruta}%`
* **Escape:** Para tratar un delimitador como texto literal, prefija el `%` con un `%` adicional.
* `%%{` se convierte en `%{`
* `}%%` se convierte en `}%`



---

## 3. Alcances Predefinidos

JSRS proporciona dos puntos de partida inmutables para garantizar la portabilidad.

### `$root` (Raíz Absoluta)

Representa el objeto de nivel superior absoluto del **entorno activo actual**.

**¡Atención, esto no es seguro en términos de contexto!**

* **Cambio de Contexto:** Si `Archivo_A` se lee de forma independiente, `$root` es la parte superior de `Archivo_A`. Si `Archivo_A` se fusiona en un atributo de `Archivo_B`, `$root` automáticamente cambia para representar la parte superior de `Archivo_B`.

### `$here` (Contexto Actual)

Representa el objeto o array inmediato en el que reside la cadena de referencia.

* Esto es el equivalente a `./` en un sistema de archivos.

---

## 4. Lógica de Navegación

JSRS usa un sistema de ruta estilo carpeta para recorrer el árbol JSON.

| Operador | Acción |
| --- | --- |
| **`/`** | Recorrido descendente hacia una clave o índice de array. |
| **`..`** | Recorrido ascendente hacia el objeto/array padre. |
| **`[clave]`** | Accede al valor de la propiedad especificada. |

**Ejemplo de Recorrido:**
Si está ubicado en `$root/users/0/profile/bio`, la ruta `../../id` resuelve a `$root/users/0/id`.

---

## 5. Espacios de Nombres Definidos por el Usuario

Además de los alcances predefinidos, JSRS permite puntos de entrada personalizados definidos dentro del propio JSON usando una propiedad `$namespace` en objetos. Esto es útil para crear alias de estructuras profundas o buses de datos externos.

### Definición

Incluya una clave `$namespace` en cualquier nivel de objeto:

```json
{
  "foo": {
    "bar": 42,
    "baz": "Hello World",
    "$namespace": "foo",
    
    "additional_data": {
      "value": 100,
      "$namespace": "data"
    }
  }
}

```
### Uso

Una vez definidos, estos pueden ser llamados como un prefijo:

* `%{$foo/bar}%`
* `%{$data/value}%`

---

## 6. Reglas de Implementación

1. **Orden de Resolución:**
* El resolvedor primero verifica si el prefijo es `$root` o `$here`.
* Si no, busca una coincidencia en la definición `$namespace`.


2. **Recorte de Padre:** Navegar `..` en el nivel `$root` no causará error; permanecerá en la raíz (recorte).
3. **Modo Estricto:** Si una ruta no puede resolverse (la clave no existe), el resolvedor debe retornar `null` o lanzar un `ReferenceError` dependiendo de la rigurosidad de la implementación.

---

## 7. No-Objetivos

JSRS NO:
- Ejecuta código
- Resuelve capacidades
- Evita fronteras de seguridad
- Accede a estado de runtime fuera del entorno de datos actual
- Implica permiso para leer datos referenciados

JSRS es un mecanismo de referencia puramente estructural.

---

## 8. Ejemplos Complejos de JSRS

### Ejemplo 1: Relatividad Profunda Dentro de Componentes Reutilizables

#### Escenario

Tienes un **esquema de componente reutilizable** que puede montarse en cualquier lugar de un documento más grande. Debe:

* Referirse a sus propios metadatos
* Referirse a la configuración global compartida
* Permanecer válido si se mueve

```json
{
  "app": {
    "config": {
      "version": "1.4.2",
      "features": {
        "beta": true
      },
      "$namespace": "config"
    },

    "modules": [
      {
        "$namespace": "module",
        "meta": {
          "name": "file_ingestor",
          "id": "mod-001"
        },
        "runtime": {
          "enabled": "%{$config/features/beta}%",
          "module_id": "%{$here/../meta/id}%",
          "app_version": "%{$config/version}%"
        }
      }
    ]
  }
}
```


---

### Ejemplo 2: Espacios de Nombres como Anclas Semánticas (No Rutas)

#### Escenario

Múltiples sistemas profundamente anidados exponen puntos de entrada *conceptuales* en lugar de estructurales.

```json
{
  "world": {
    "$namespace": "world",
    "time": {
      "era": "Fourth Age",
      "year": 1207
    }
  },

  "character": {
    "$namespace": "actor",
    "profile": {
      "name": "Elarion",
      "origin": "%{$world/time/era}%"
    },
    "log": [
      {
        "event": "born",
        "year": "%{$world/time/year}%"
      }
    ]
  }
}
```


---

### Ejemplo 3: Navegación Relativa a Través de Arrays y Objetos

### Escenario

Un sistema de reglas donde cada regla referencia a sus hermanos y contexto padre.

```json
{
  "ruleset": {
    "rules": [
      {
        "id": "r1",
        "value": 10
      },
      {
        "id": "r2",
        "value": 20,
        "comparison": {
          "self": "%{$here/../value}%",
          "previous_rule": "%{$here/../../0/value}%",
          "ruleset_root": "%{$root/ruleset}%"
        }
      }
    ]
  }
}
```


---

## 9. Patrones Comunes

### 1. Componentes Auto-Descriptivos (Recomendado)

**Patrón**
Use `$here` para cableado interno, espacios de nombres solo para contratos externos.

```json
{
  "component": {
    "$namespace": "comp",
    "id": "c-77",
    "config": {
      "ref": "%{$here/../id}%"
    }
  }
}
```

---

### 2. Espacios de Nombres Semánticos Sobre Rutas Estructurales

**Patrón**
Los espacios de nombres representan *significado*, no *ubicación*.

```json
{
  "auth": {
    "$namespace": "auth",
    "token": "ZX-42"
  },
  "request": {
    "header": "%{$auth/token}%"
  }
}
```

---

### 3. `$root` Explícito Solo en Fronteras de Integración

**Patrón**
Use `$root` en lugares que *esperan* cambios de contexto.

```json
{
  "manifest": {
    "entry": "%{$root/app/main}%"
  }
}
```

---

### 4. Patrón Espacio-de-Nombres-como-API

**Patrón**
Exponga un espacio de nombres estable que actúe como una superficie de API de solo lectura.

```json
{
  "system": {
    "$namespace": "sys",
    "limits": {
      "max_tasks": 10
    }
  },
  "scheduler": {
    "cap": "%{$sys/limits/max_tasks}%"
  }
}
```

---

## 10. Anti-Patrones 🚨

### 1. Uso Excesivo de `$root` en Todas Partes

❌ **Malo**

```json
{
  "config": {
    "value": "%{$root/a/b/c/d/e/value}%"
  }
}
```

**Por qué esto es malo**

* Se rompe inmediatamente cuando se fusiona
* Acoplamiento global implícito
* Destruye la modularidad

✅ **Mejor**

```json
"value": "%{$namespace/value}%"
```

---

### 2. Matemáticas de Ruta en Lugar de Significado

❌ **Malo**

```json
"value": "%{../../../../settings/flags/enabled}%"
```

**Por qué esto es malo**

* Frágil
* Imposible de razonar después
* Las refactorizaciones se vuelven peligrosas

✅ **Mejor**

```json
"value": "%{$settings/flags/enabled}%"
```

---

### 3. Colisiones de Espacio de Nombres 

❌ **Malo**

```json
{
  "$namespace": "data",
  "data": {
    "$namespace": "data"
  }
}
```

**Por qué esto es malo**

* Resolución ambigua
* Rompe el modelo mental
* Pesadilla de depuración

✅ **Regla general**

> Un espacio de nombres = una autoridad conceptual

---

### 4. Tratar JSRS como Lógica

❌ **Malo**

```json
"enabled": "%{$config/flags/is_admin && $config/flags/is_active}%"
```

**Por qué esto es malo**

* JSRS no es un lenguaje de expresión
* Viola los no-objetivos
* Conduce a semánticas de ejecución ocultas

✅ **Enfoque correcto**
Resuelva referencias primero, compute lógica externamente.

---

### 5. Acceso Oculto Entre Fronteras

❌ **Malo**

```json
"token": "%{$root/secrets/internal/admin_token}%"
```

**Por qué esto es malo**

* Parece inofensivo
* Filtra estructura sensible
* Fomenta suposiciones implícitas de privilegio

JSRS **no concede permiso** — pero los humanos lo interpretarán mal.

---
