# Especificação URN ANIMA

**Autoridade:** ANIMA  
**Versão:** 0.1.0

---

## 1. Propósito

Este documento define a estrutura canônica, semântica e regras para
**Nomes de Recursos Uniformes (URNs) ANIMA**.

URNs ANIMA fornecem:
- Identificadores globalmente únicos
- Identidade estável e imutável
- Autoridade semântica explícita
- Referências de longa duração independentes de localização ou implementação

Esta especificação é **normativa**.  
Todas as implementações DEVEM estar em conformidade com as regras definidas aqui.

---

## 2. Princípios de Design

URNs ANIMA são projetadas de acordo com os seguintes princípios:

1. **Imutabilidade**  
   Uma vez emitida, uma URN NUNCA DEVE mudar de significado.

2. **Identidade Centrada na Autoridade**  
   O significado é governado pelo conjunto de regras, especificações e suposições de confiança que definem como uma URN ANIMA é interpretada, a autoridade ANIMA mencionada anteriormente, não por ferramentas ou código.

3. **Semântica Explícita**  
   Toda interpretação semântica DEVE ser derivável da estrutura URN e especificações associadas.

4. **Contratos Versionados**
    Cada URN define uma versão da identidade que representa. Mudanças de versão implicam semântica de identidade distinta.

5. **Identificadores Opacos**
    O segmento identificador final NÃO DEVE codificar semântica. Serve apenas como uma referência única.

6. **Especificação Central**
    Este documento é a única fonte de verdade para estrutura e semântica de URN ANIMA. Implementações de geradores são secundárias.

---

## 3. Formato Canônico

O formato canônico de URN ANIMA é:

```
urn:anima:<escopo>:<namespace>@<versão>:<id>
```

### 3.1 Visão Geral dos Segmentos

| Segmento    | Descrição |
|------------|-------------|
| `urn`      | Identificador de esquema URN padrão |
| `anima`    | Autoridade de namespace raiz |
| `escopo`   | Limite de confiança e ciclo de vida |
| `namespace`| Domínio de identidade semântica |
| `versão`   | Versão do contrato de identidade semântica |
| `id`       | Identificador único opaco |

---

## 4. Escopo

O segmento `escopo` define as **garantias de autoridade e ciclo de vida** do
recurso identificado.

Valores permitidos:

- `core`  
  Identidades canônicas de primeira classe reconhecidas pelo próprio runtime ANIMA.

- `module`  
  Identidades de propriedade de extensões, plugins ou módulos externos.

Exemplos:

```
urn:anima:core:seed@0.1.0:...
urn:anima:module:CLI.capability@0.2.1:...
```

---

## 5. Namespace

O segmento `namespace` define o **domínio semântico** da identidade.

### 5.1 Regras

- DEVE ser minúsculo
- DEVE usar caracteres alfanuméricos e pontos (`.`)
- NÃO DEVE conter dois pontos (`:`)
- DEVERIA refletir hierarquia conceitual, não estrutura de implementação

### 5.2 Exemplos

```
seed
cortex.reasoning
schema.intent
runtime.instance
```

---

## 6. Versionamento

O segmento `versão` especifica o **contrato de identidade semântica** governando
a interpretação da URN.

### 6.1 Formato de Versão

Versões DEVEM seguir versionamento semântico estrito:

```
<major>.<minor>.<patch>
```

Exemplo:
```
0.0.2
1.0.0
2.3.1
```

Si el versionado no tiene significado semántico, use `0.0.0`.

### 6.2 Significado Semântico

Dentro de uma URN:

- QUALQUER mudança de versão representa uma **semântica de identidade distinta**
- Compatibilidade NÃO DEVE ser inferida apenas dos números de versão
- Mesmo mudanças no nível de patch (`0.0.1` → `0.0.2`) implicam uma nova classe de identidade

Relacionamentos de compatibilidade, se houver, DEVEM ser declarados explicitamente fora
da URN.

---

## 7. Identificador (`id`)

O segmento final é um **identificador único opaco**.

### 7.1 Formas Permitidas

- UUID v4

O identificador DEVE:
- Ser globalmente único
- Ser opaco e não derivável
- NÃO codificar semântica, estrutura, versionamento ou informação de conteúdo
- NÃO ser deterministicamente derivado do recurso que identifica

Hashes criptográficos, checksums ou qualquer identificador derivado do conteúdo do recurso
NÃO DEVEM ser usados como identificadores URN, pois violam o requisito de opacidade.

### 7.2 Justificativa (Informativa)

Identificadores opacos garantem que:
- Identidade é independente da representação
- Identificadores permanecem estáveis apesar de mudanças no recurso
- Nenhuma suposição pode ser inferida do próprio identificador

Exemplos:

```
6f9619ff-8b86-d011-b42d-00cf4fc964ff
9a31c4b2-4f2d-4e9e-a7f9-8b32d3e1f9a1
```

---

## 8. Regras de Imutabilidade

Uma vez emitida:

- Uma URN DEVE permanecer válida indefinidamente
- Uma URN NÃO DEVE ser reutilizada
- Uma URN NÃO DEVE mudar de significado
- Uma URN NÃO DEVE ser reemitida com semântica diferente

Qualquer evolução semântica requer a emissão de uma **nova URN** com uma nova versão.

---

## 9. Implementações de Geradores

Geradores de URN são **implementações desta especificação**, não
definições autoritativas.

- Números de versão de geradores NÃO DEVEM aparecer em URNs
- Múltiplos geradores PODEM coexistir
- Todos os geradores DEVEM produzir URNs em conformidade com este documento

Esta especificação é a única fonte de verdade.

---

## 10. Validação (Informativa)

Uma URN ANIMA válida DEVE corresponder ao seguinte padrão *conceitual*:

```
urn:anima:(core|module):[a-z0-9.]+@[0-9]+.[0-9]+.[0-9]+:<opaque-id>
```

---

## 11. Exemplos

### Instância de Seed Central
```
urn:anima:core:seed@0.0.2:6f9619ff-8b86-d011-b42d-00cf4fc964ff
```

### Córtex de Raciocínio Central
```
urn:anima:core:cortex.reasoning@0.1.0:9a31c4b2-4f2d-4e9e-a7f9-8b32d3e1f9a1
```

### Identidade de Esquema
```
urn:anima:core:schema.intent@1.0.0:ba3bf79c-381e-409b-9450-12e241100d3a
```

---

## 12. Extensões Futuras

Revisões futuras PODEM definir:
- Declarações de compatibilidade explícitas
- Regras de vinculação de assinatura
- Mecanismos de resolução
- Federação entre autoridades

Tais extensões NÃO DEVEM violar as garantias de imutabilidade definidas aqui.

---

## 13. Notas Finais

URNs ANIMA são **contratos de identidade**, não referências, não localizações,
e não artefatos de implementação.

Elas existem para preservar significado através do tempo, sistemas e interpretações.
