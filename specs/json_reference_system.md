# JSON Scoped Reference System (JSRS)

## 1. Overview

The **JSON Scoped Reference System (JSRS)** is a dynamic path-addressing protocol designed for modular JSON environments. It allows data structures to be moved, merged, or nested without breaking internal logic by using relative navigation and user-defined namespaces.

---

## 2. Syntax & Delimiters

References are embedded within strings using the percentage-brace syntax.

* **Reference Wrapper:** `%{path}%`
* **Escaping:** To treat a delimiter as literal text, prefix the `%` it with an additional `%`.
* `%%{` becomes `%{`
* `}%%` becomes `}%`



---

## 3. Predefined Scopes

JSRS provides two immutable starting points to ensure portability.

### `$root` (Absolute Root)

Represents the absolute top-level object of the **current active environment**.

**Attention, this is not context safe!**

* **Context Shifting:** If `File_A` is read standalone, `$root` is the top of `File_A`. If `File_A` is merged into an attribute of `File_B`, `$root` automatically shifts to represent the top of `File_B`.

### `$here` (Current Context)

Represents the immediate object or array in which the reference string resides.

* This is the equivalent of `./` in a file system.

---

## 4. Navigation Logic

JSRS uses a folder-style pathing system to traverse the JSON tree.

| Operator | Action |
| --- | --- |
| **`/`** | Downward traversal into a key or array index. |
| **`..`** | Upward traversal to the parent object/array. |
| **`[key]`** | Accesses the value of the specified property. |

**Example Traversal:**
If located at `$root/users/0/profile/bio`, the path `../../id` resolves to `$root/users/0/id`.

---

## 5. User-Defined Namespaces

Beyond the predefined scopes, JSRS allows for custom entry points defined within the JSON itself using a `$namespace` property on objects. This is useful for aliasing deep structures or external data buses.

### Definition

Include a `$namespace` key on any object level:

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
### Usage

Once defined, these can be called as a prefix:

* `%{$foo/bar}%`
* `%{$data/value}%`

---

## 6. Implementation Rules

1. **Resolution Order:**
* The resolver first checks if the prefix is `$root` or `$here`.
* If not, it looks for a match in the `$namespace` definition.


2. **Parent Clipping:** Navigating `..` at the `$root` level will not error; it will remain at the root (clipping).
3. **Strict Mode:** If a path cannot be resolved (key does not exist), the resolver should return `null` or throw a `ReferenceError` depending on the implementation's strictness.

---

## 7. Non-Goals

JSRS does NOT:
- Execute code
- Resolve capabilities
- Bypass security boundaries
- Access runtime state outside the current data environment
- Imply permission to read referenced data

JSRS is a purely structural reference mechanism.

---

## 8. Complex JSRS Examples

### Example 1: Deep Relativity Inside Reusable Components

#### Scenario

You have a **reusable component schema** that can be mounted anywhere in a larger document. It must:

* Refer to its own metadata
* Refer to shared global config
* Remain valid if moved

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

### Example 2: Namespaces as Semantic Anchors (Not Paths)

#### Scenario

Multiple deeply nested systems expose *conceptual* entry points rather than structural ones.

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

### Example 3: Relative Navigation Across Arrays and Objects

### Scenario

A rule system where each rule references its siblings and parent context.

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

## 9. Common Patterns

### 1. Self-Describing Components (Recommended)

**Pattern**
Use `$here` for internal wiring, namespaces only for external contracts.

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

### 2. Semantic Namespaces Over Structural Paths

**Pattern**
Namespaces represent *meaning*, not *location*.

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

### 3. Explicit `$root` Only at Integration Boundaries

**Pattern**
Use `$root` in places that *expect* context shifts.

```json
{
  "manifest": {
    "entry": "%{$root/app/main}%"
  }
}
```

---

### 4. Namespace-as-API Pattern

**Pattern**
Expose a stable namespace that acts like a read-only API surface.

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

## 10. Anti-Patterns 🚨

### 1. Overusing `$root` Everywhere

❌ **Bad**

```json
{
  "config": {
    "value": "%{$root/a/b/c/d/e/value}%"
  }
}
```

**Why this is bad**

* Breaks immediately when merged
* Implicit global coupling
* Destroys modularity

✅ **Better**

```json
"value": "%{$namespace/value}%"
```

---

### 2. Path Math Instead of Meaning

❌ **Bad**

```json
"value": "%{../../../../settings/flags/enabled}%"
```

**Why this is bad**

* Brittle
* Impossible to reason about later
* Refactors become dangerous

✅ **Better**

```json
"value": "%{$settings/flags/enabled}%"
```

---

### 3. Namespace Collisions 

❌ **Bad**

```json
{
  "$namespace": "data",
  "data": {
    "$namespace": "data"
  }
}
```

**Why this is bad**

* Ambiguous resolution
* Breaks the mental model
* Debugging nightmare

✅ **Rule of thumb**

> One namespace = one conceptual authority

---

### 4. Treating JSRS as Logic

❌ **Bad**

```json
"enabled": "%{$config/flags/is_admin && $config/flags/is_active}%"
```

**Why this is bad**

* JSRS is not an expression language
* Violates non-goals
* Leads to hidden execution semantics

✅ **Correct approach**
Resolve references first, compute logic externally.

---

### 5. Hidden Cross-Boundary Access

❌ **Bad**

```json
"token": "%{$root/secrets/internal/admin_token}%"
```

**Why this is bad**

* Looks harmless
* Leaks sensitive structure
* Encourages implicit privilege assumptions

JSRS **does not grant permission** — but humans will misread this.

---

