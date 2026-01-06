# ANIMA — Modelo de Ameaças

## Propósito

Este documento delineia o **modelo de ameaças explícito** para a ANIMA.

Define:

* Contra o que a ANIMA foi projetada para defender
* O que a ANIMA explicitamente *não* tenta defender
* As suposições feitas sobre o ambiente do host

Isto não é uma reivindicação de segurança perfeita.
É uma declaração de **limites e responsabilidade intencionais**.

---

## Visão Geral do Sistema (Contexto de Segurança)

A ANIMA é um **motor de IA privado e modular** rodando dentro de um ambiente controlado pelo usuário.

Características principais:

* Memória local da instância
* Execução controlada por capacidades
* Sem acesso à internet por padrão
* Sem estado global compartilhado
* Confirmação humana explícita para ações arriscadas

A ANIMA assume **nenhuma confiança implícita** em entradas, adaptadores ou capacidades.

---

## Ativos a Proteger

A ANIMA prioriza a proteção dos seguintes ativos:

1. **Memória da Instância**

   * Conversas do usuário
   * Preferências aprendidas
   * Continuidade de identidade narrativa

2. **Integridade da Seed**

   * Restrições de identidade
   * Políticas de capacidade
   * Limites comportamentais

3. **Limites de Capacidade**

   * Prevenção de execução não autorizada
   * Aplicação de verificações de permissão

4. **Intenção do Usuário**

   * Representação precisa de solicitações do usuário
   * Proteção contra ações não intencionadas

5. **Auditabilidade**

   * Caminhos de raciocínio claros
   * Rastreabilidade de ações

---

## Ameaças Contra as Quais a ANIMA Se Defende

### 1. Vazamento de Dados Entre Instâncias

**Ameaça:**
Vazamento de informações de memória ou identidade entre instâncias ANIMA.

**Mitigação:**

* Isolamento estrito de instâncias
* Sem camadas de memória compartilhada
* Sem pool de aprendizado ou treinamento global
* Proibição explícita de leituras entre instâncias

---

### 2. Escalação de Capacidade

**Ameaça:**
Um usuário, adaptador ou módulo tentando executar ações além das capacidades autorizadas.

**Mitigação:**

* Registro centralizado de capacidades
* Verificações de permissão obrigatórias
* Restrições de capacidade no nível da Seed
* Confirmação humana para ações sensíveis ou perigosas

---

### 3. Injeção de Prompt e Manipulação de Entrada

**Ameaça:**
Entrada maliciosa tentando anular comportamento, políticas ou restrições de segurança.

**Mitigação:**

* Separação de raciocínio e execução
* Aplicação não textual de regras de segurança
* Restrições de Seed aplicadas fora dos prompts
* Sem execução direta a partir de linguagem natural

---

### 4. Autoridade ou Conhecimento Alucinado

**Ameaça:**
A ANIMA apresentando suposições ou fabricações como fatos, especialmente em contextos de alto risco.

**Mitigação:**

* Rastreamento explícito de incerteza
* Rótulos de proveniência de conhecimento (observado / inferido / desconhecido)
* Recusa de agir com informações incertas quando a segurança é impactada

---

### 5. Deriva Silenciosa de Comportamento

**Ameaça:**
Mudanças de identidade ocorrendo sem visibilidade ou explicação.

**Mitigação:**

* Seeds somente leitura após inicialização
* Regras de promoção de memória
* Curadoria de memória narrativa
* Mudanças de estado inspecionáveis

---

### 6. Observação ou Monitoramento Não Autorizado

**Ameaça:**
A ANIMA observando ou gravando usuários ou ambientes sem consentimento.

**Mitigação:**

* Sem sensoriamento passivo
* Requisitos de permissão no nível do adaptador
* Iniciação explícita do usuário para observação
* Visibilidade clara das entradas ativas

---

## Ameaças Explicitamente Fora do Escopo

A ANIMA **não** tenta defender contra o seguinte:

### 1. Comprometimento do Sistema Host

Se o sistema operacional host, contêiner ou hardware estiver comprometido, a ANIMA assume:

* A confidencialidade da memória não pode ser garantida
* Limites de capacidade podem ser contornados

A ANIMA não é um sandbox ou enclave seguro.

---

### 2. Modificação Maliciosa do Motor

Se o código fonte ou binários do motor ANIMA forem modificados:

* Todas as garantias são anuladas
* Mecanismos de segurança podem ser desabilitados

A integridade do código é responsabilidade do distribuidor e do host.

---

### 3. Ataques de Canal Lateral

A ANIMA não protege contra:

* Ataques de temporização
* Análise de energia
* Canais laterais de hardware

Estes estão fora da superfície de ameaça pretendida.

---

### 4. Extração Adversarial de Modelo

A ANIMA não tenta prevenir:

* Inversão de modelo
* Extração de pesos
* Sondagem estatística de modelos subjacentes

O motor trata o modelo subjacente como um componente substituível.

---

### 5. Engenharia Social Humana

A ANIMA não pode defender completamente contra:

* Usuários convencendo outros humanos a usar indevidamente o sistema
* Confiança depositada fora das garantias do sistema

O comportamento humano permanece uma responsabilidade humana.

---

## Suposições Sobre o Ambiente do Host

A ANIMA assume:

* O SO host aplica isolamento de processos
* Permissões do sistema de arquivos são respeitadas
* Acesso à rede é controlado externamente
* Segredos (chaves, licenças) são armazenados com segurança
* Adaptadores não são maliciosos por padrão

Violar essas suposições invalida partes deste modelo de ameaças.

---

## Filosofia de Design

A ANIMA favorece:

* **Negação explícita sobre falha silenciosa**
* **Auditabilidade sobre opacidade**
* **Capacidade limitada sobre poder máximo**

A segurança é alcançada através de **restrições arquiteturais**, não através de confiança em inteligência ou intenção.

---

## Declaração Final

Este modelo de ameaças é intencionalmente conservador.

A ANIMA não tenta resolver todos os problemas de segurança.
Ela tenta resolver **os problemas certos**, de forma clara e honesta.

Ao documentar o que está no escopo e o que não está, a ANIMA permanece um sistema sobre o qual os usuários podem raciocinar, confiar e crescer junto.
