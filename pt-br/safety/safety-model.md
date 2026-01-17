# SAFETY_MODEL.md

## Propósito

Este documento descreve **como a ANIMA aplica segurança mecanicamente**.

Não define valores morais, teorias éticas ou ideais comportamentais.
A segurança na ANIMA é alcançada através de **restrições explícitas, etapas de verificação e limites de execução** aplicados pelo motor.

---

## Segurança como uma Propriedade do Motor

A segurança na ANIMA **não é baseada em prompts** e **não é baseada em personalidade**.

É aplicada por:

* Controle de capacidades
* Verificações de permissão
* Fluxos de confirmação
* Validação epistêmica
* Tratamento explícito de incerteza
* Isolamento de execução

Nenhum componente é confiável por padrão — incluindo adaptadores, entradas ou o modelo de linguagem.

---

## Validação Epistêmica

A ANIMA rastreia a proveniência de todo conhecimento usado na tomada de decisões.

### Estados de Conhecimento

Todo conhecimento é classificado como um de um conjunto de estados (pode evoluir ao longo do tempo):

* **Observado:** Dados percebidos diretamente ou de entrada.
* **Lembrado:** Recuperado de camadas de memória com metadados de confiança.
* **Inferido:** Derivado através de raciocínio com níveis de incerteza.

Todo conhecimento é marcado com sua fonte e nível de confiança.

### Compreendendo significado e proveniência sem confiança cega

A ANIMA previne confiança cega total (ex: utilizar outros modelos de linguagem como aplicadores) através de técnicas de processamento de linguagem natural e detecção de não conformidade.

Através desses mecanismos, a ANIMA pode identificar:
* Contradições
* Inconsistências
* Ambiguidades
* Quando a confiança do conhecimento é insuficiente para uma decisão

## Controle de Capacidades

A ANIMA não pode executar ações arbitrárias.

Todas as ações devem mapear para um **módulo de capacidade declarado**.

### Regras de Capacidade

* As capacidades são:

  * Explícitas
  * Descobríveis
  * Auditáveis

* As capacidades devem ser:

  * Habilitadas pelo motor
  * Permitidas pela seed ativa
  * Permitidas para a função do usuário atual

### Restrição Importante

> **Seeds podem restringir capacidades, mas nunca conceder novo poder de execução.**

Isso garante:

* Nenhuma identidade pode exceder os limites rígidos do motor
* Limites de licenciamento e segurança permanecem aplicáveis
* Capacidades não podem ser contrabandeadas através de personalidade ou memória

---

## Fluxos de Confirmação

Algumas capacidades requerem **confirmação humana** antes da execução.

### Avaliação de Nível de Risco

A ANIMA avalia cada capacidade ao longo de dois eixos independentes:

* **Nível de Risco do Sistema** — Impacto potencial máximo à integridade, segurança, disponibilidade ou limites de confiança do sistema se a operação se comportar incorretamente ou for abusada. (Veja [Glossário Canônico](../foundations/canonical-glossary.md))

* **Nível de Risco do Usuário** — Dano potencial máximo à privacidade, finanças, reputação, autonomia, saúde (mental ou física) ou resultados irreversíveis do usuário se a operação for executada incorretamente ou sem compreensão completa. (Veja [Glossário Canônico](../foundations/canonical-glossary.md))

Estes níveis de risco expressam **severidade do impacto**, não probabilidade, e são independentes um do outro.

### Quando a Confirmação É Necessária

Requisitos de confirmação são determinados pelo **Nível de Risco do Usuário**:

* Operações de alto Nível de Risco do Usuário (ex: transações financeiras, ações irreversíveis)
* Operações afetando privacidade do usuário ou dados pessoais
* Ações com consequências externas significativas
* Operações onde a intenção do usuário é ambígua

O Nível de Risco do Sistema influencia sandboxing, isolamento e mecanismos de aplicação, mas não desencadeia diretamente confirmação do usuário.

### Propriedades de Confirmação

* Explícita (não inferida)
* Limitada por tempo
* Multi-fator quando o Nível de Risco do Usuário justificar
* Registrada
* Vinculada a uma instância de ação específica

Adaptadores **não podem auto-afirmar consentimento**.
A confirmação deve ser validada pelo motor através de um ou mais canais confiáveis.

---

## Sem Execução Direta a Partir de Linguagem Natural

Linguagem natural **nunca desencadeia execução diretamente**.

O pipeline de execução é estritamente:

```
Entrada → Interpretação → Intenção → Proposta de Capacidade → Validação → Confirmação → Execução
```

Isso previne:

* Injeção de prompt
* Ambiguidade linguística causando ações
* Adaptadores escalando intenção
* Modos de falha "apenas faça"

Se a intenção não pode ser mapeada com confiança, **a execução não ocorre**.

---

## Alucinações São Tratadas como Falhas

A ANIMA trata alucinações como **erros de sistema**, não como peculiaridades estilísticas.

### O Que Conta como Alucinação

* Afirmar fatos sem evidência suficiente
* Reivindicar capacidades que não estão disponíveis
* Inventar memórias ou permissões
* Adivinhar intenção do usuário

### Aplicação

* O motor rastreia proveniência de conhecimento:

  * observado
  * lembrado
  * inferido

* Se a confiança é insuficiente:

  * A ANIMA deve dizer isso explicitamente
  * A ação é bloqueada ou rebaixada

> Uma mentira confiante é considerada pior que uma resposta recusada.

---

## Tratamento de Incerteza

A incerteza é **explicitamente representada**, não escondida.

### Mecanismos

* Limiares de confiança para classificação de intenção
* Clarificação necessária quando a ambiguidade é alta
* Estados explícitos de "Eu não sei"
* Recusa quando o risco de execução excede a certeza

A incerteza não é tratada como falha —
**agir sem reconhecer a incerteza é.**

---

## Segurança de Memória

A memória é restringida para prevenir inferência insegura e deriva.

* Sem memória entre instâncias
* Sem promoção implícita de memória
* Memória narrativa requer curadoria explícita
* Seeds são imutáveis após a inicialização

Isso previne:

* Corrupção de identidade
* Deriva silenciosa de personalidade
* Formação acidental de crenças

---

## Auditabilidade

Toda decisão relevante para segurança é registrada:

* Verificações de capacidade
* Negações de permissão
* Solicitações de confirmação
* Tentativas de execução
* Recusas

Os registros são projetados para responder:

> "Por que a ANIMA agiu — ou recusou — neste caso?"

---

## O Que o Modelo de Segurança *Não* Faz

Este modelo **não** tenta:

* Aplicar alinhamento moral
* Julgar intenção do usuário além do risco de execução
* Prevenir uso indevido em hosts comprometidos
* Agir como sandbox ou enclave seguro
* Substituir responsabilidade humana

Essas preocupações estão explicitamente fora do escopo.

---

## Resumo

O modelo de segurança da ANIMA é baseado em:

* Limites explícitos de capacidade
* Aplicação no nível do motor
* Confirmação humana para risco
* Incerteza transparente
* Tratar alucinações como erros

A segurança não é comportamento emergente.
É **projetada, aplicada e auditável**.
