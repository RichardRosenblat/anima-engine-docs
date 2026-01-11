# Sistema de Referência com Escopo JSON (JSRS)

## 1. Visão Geral

O **Sistema de Referência com Escopo JSON (JSRS)** é um protocolo de endereçamento de caminho dinâmico projetado para ambientes JSON modulares. Ele permite que estruturas de dados sejam movidas, mescladas ou aninhadas sem quebrar a lógica interna, usando navegação relativa e namespaces definidos pelo usuário.

---

## 2. Sintaxe e Delimitadores

Referências são incorporadas em strings usando a sintaxe de chave-porcentagem.

* **Wrapper de Referência:** `%{caminho}%`
* **Escape:** Para tratar um delimitador como texto literal, prefixe o `%` com um `%` adicional.
* `%%{` torna-se `%{`
* `}%%` torna-se `}%`



---

## 3. Escopos Predefinidos

JSRS fornece dois pontos de partida imutáveis para garantir portabilidade.

### `$root` (Raiz Absoluta)

Representa o objeto de nível superior absoluto do **ambiente ativo atual**.

**Atenção, isto não é seguro em termos de contexto!**

* **Mudança de Contexto:** Se `Arquivo_A` for lido isoladamente, `$root` é o topo de `Arquivo_A`. Se `Arquivo_A` for mesclado em um atributo de `Arquivo_B`, `$root` automaticamente muda para representar o topo de `Arquivo_B`.

### `$here` (Contexto Atual)

Representa o objeto ou array imediato no qual a string de referência reside.

* Isto é o equivalente a `./` em um sistema de arquivos.

---

## 4. Lógica de Navegação

JSRS usa um sistema de caminho estilo pasta para percorrer a árvore JSON.

| Operador | Ação |
| --- | --- |
| **`/`** | Travessia descendente para uma chave ou índice de array. |
| **`..`** | Travessia ascendente para o objeto/array pai. |
| **`[chave]`** | Acessa o valor da propriedade especificada. |

**Exemplo de Travessia:**
Se localizado em `$root/users/0/profile/bio`, o caminho `../../id` resolve para `$root/users/0/id`.

---

## 5. Namespaces Definidos pelo Usuário

Além dos escopos predefinidos, JSRS permite pontos de entrada personalizados definidos dentro do próprio JSON usando uma propriedade `$namespace` em objetos. Isto é útil para criar aliases de estruturas profundas ou barramentos de dados externos.

### Definição

Inclua uma chave `$namespace` em qualquer nível de objeto:

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

Uma vez definidos, estes podem ser chamados como um prefixo:

* `%{$foo/bar}%`
* `%{$data/value}%`

---

## 6. Regras de Implementação

1. **Ordem de Resolução:**
* O resolvedor primeiro verifica se o prefixo é `$root` ou `$here`.
* Se não, ele procura uma correspondência na definição `$namespace`.


2. **Recorte de Pai:** Navegar `..` no nível `$root` não causará erro; permanecerá na raiz (recorte).
3. **Modo Estrito:** Se um caminho não puder ser resolvido (chave não existe), o resolvedor deve retornar `null` ou lançar um `ReferenceError` dependendo da rigorosidade da implementação.

---

## 7. Não-Objetivos

JSRS NÃO:
- Executa código
- Resolve capacidades
- Contorna fronteiras de segurança
- Acessa estado de runtime fora do ambiente de dados atual
- Implica permissão para ler dados referenciados

JSRS é um mecanismo de referência puramente estrutural.

---

## 8. Exemplos Complexos de JSRS

### Exemplo 1: Relatividade Profunda Dentro de Componentes Reutilizáveis

#### Cenário

Você tem um **esquema de componente reutilizável** que pode ser montado em qualquer lugar em um documento maior. Ele deve:

* Referir-se aos seus próprios metadados
* Referir-se à configuração global compartilhada
* Permanecer válido se movido

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

### Exemplo 2: Namespaces como Âncoras Semânticas (Não Caminhos)

#### Cenário

Múltiplos sistemas profundamente aninhados expõem pontos de entrada *conceituais* em vez de estruturais.

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

### Exemplo 3: Navegação Relativa Através de Arrays e Objetos

### Cenário

Um sistema de regras onde cada regra referencia seus irmãos e contexto pai.

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

## 9. Padrões Comuns

### 1. Componentes Auto-Descritivos (Recomendado)

**Padrão**
Use `$here` para conexão interna, namespaces apenas para contratos externos.

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

### 2. Namespaces Semânticos Sobre Caminhos Estruturais

**Padrão**
Namespaces representam *significado*, não *localização*.

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

### 3. `$root` Explícito Apenas em Fronteiras de Integração

**Padrão**
Use `$root` em lugares que *esperam* mudanças de contexto.

```json
{
  "manifest": {
    "entry": "%{$root/app/main}%"
  }
}
```

---

### 4. Padrão Namespace-como-API

**Padrão**
Exponha um namespace estável que atua como uma superfície de API somente leitura.

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

## 10. Anti-Padrões 🚨

### 1. Uso Excessivo de `$root` em Todos os Lugares

❌ **Ruim**

```json
{
  "config": {
    "value": "%{$root/a/b/c/d/e/value}%"
  }
}
```

**Por que isso é ruim**

* Quebra imediatamente quando mesclado
* Acoplamento global implícito
* Destrói modularidade

✅ **Melhor**

```json
"value": "%{$namespace/value}%"
```

---

### 2. Matemática de Caminho em Vez de Significado

❌ **Ruim**

```json
"value": "%{../../../../settings/flags/enabled}%"
```

**Por que isso é ruim**

* Frágil
* Impossível de raciocinar depois
* Refatorações tornam-se perigosas

✅ **Melhor**

```json
"value": "%{$settings/flags/enabled}%"
```

---

### 3. Colisões de Namespace 

❌ **Ruim**

```json
{
  "$namespace": "data",
  "data": {
    "$namespace": "data"
  }
}
```

**Por que isso é ruim**

* Resolução ambígua
* Quebra o modelo mental
* Pesadelo de depuração

✅ **Regra geral**

> Um namespace = uma autoridade conceitual

---

### 4. Tratar JSRS como Lógica

❌ **Ruim**

```json
"enabled": "%{$config/flags/is_admin && $config/flags/is_active}%"
```

**Por que isso é ruim**

* JSRS não é uma linguagem de expressão
* Viola não-objetivos
* Leva a semânticas de execução ocultas

✅ **Abordagem correta**
Resolva referências primeiro, compute lógica externamente.

---

### 5. Acesso Oculto Entre Fronteiras

❌ **Ruim**

```json
"token": "%{$root/secrets/internal/admin_token}%"
```

**Por que isso é ruim**

* Parece inofensivo
* Vaza estrutura sensível
* Encoraja suposições implícitas de privilégio

JSRS **não concede permissão** — mas humanos interpretarão mal isso.

---
