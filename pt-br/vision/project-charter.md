# Carta do Projeto

**Documentação Relacionada:**
* [Visão](vision.md) - Visão e objetivos de longo prazo
* [Não-Objetivos](non-goals.md) - Não-objetivos e limites explícitos
* [Visão Geral da Arquitetura](architecture/anima-architecture.md) - Arquitetura completa do sistema

---

## O que é ANIMA?

ANIMA é um **motor de IA privado e modular** projetado para hospedar identidades artificiais de longa duração e em evolução sob rigorosas restrições de segurança, memória e capacidade.

ANIMA é:

* Um **runtime de IA privado** para identidades em evolução
* Um **motor agnóstico de identidade** (personalidade vem de Seeds)
* Um **kernel cognitivo** que supervisiona comportamento
* Um **sistema de raciocínio por eventos** que opera sobre fatos estruturados
* Uma **arquitetura hexagonal** com limites de domínio limpos
* Um **sistema de módulos baseado em lease** sobre gRPC com mTLS

ANIMA **não** é:

* Um chatbot ou wrapper de conversação
* Um agente autônomo que se auto-expande
* Uma personalidade única ou personagem hardcoded
* Um sistema de rastreamento de internet por padrão
* Um sistema de memória compartilhada ou sincronizado na nuvem
* Um substituto para julgamento humano

---

## Propósito Central

ANIMA existe para:

1. **Fornecer um runtime de IA privado e evolutivo**
   * Hospedar identidades artificiais de longa duração
   * Habilitar memória e aprendizado local à instância
   * Suportar operação contínua e gerenciamento de tarefas

2. **Separar motor de identidade**
   * Motor: raciocínio, planejamento, mecânica de memória, segurança
   * Identidade: personalidade, tom, políticas comportamentais (definidas por Seed)
   * Habilitar múltiplas instâncias independentes de um único motor

3. **Suportar múltiplas encarnações via Seeds**
   * Instâncias de usuário privadas com Seeds customizados
   * Identidades protegidas (e.g., ANIMA Prime para streaming)
   * Sem compartilhamento de memória ou estado entre instâncias

4. **Habilitar interação segura e auditável com o mundo**
   * Autorização baseada em lease para todas as capacidades
   * Observabilidade baseada em eventos (fatos estruturados e rastreáveis)
   * Limites de segurança criptográficos
   * Confirmação humana para ações perigosas
   * Trilhas de auditoria completas para todas as decisões e efeitos

---

## Valores Centrais

Estes valores são **constitucionais**. Cada funcionalidade deve ser justificável contra eles.

### Verdade sobre Confiança

ANIMA prioriza precisão e honestidade sobre aparentar certeza.

* Rastrear se a informação é observada, lembrada, inferida ou desconhecida
* Admitir incerteza em vez de fabricar
* Alucinação confiante é tratada como um bug
* Memória é falível e consultável, não confiad

a cegamente

### Intenção sobre Execução

ANIMA foca em entender e cumprir intenções em vez de executar comandos cegamente.

* Core produz **intenção**, não efeitos diretos
* Intenção inclui: o quê, quando, onde, quanto, por quê, contingências, confiança
* Execução é delegada a módulos supervisionados
* Intenção é auditável, registrável e reproduzível

### Modularidade sobre Monólito

ANIMA é projetada com componentes intercambiáveis para flexibilidade e evolução de longo prazo.

* Clara separação de responsabilidades (domínios, infraestrutura, módulos)
* Portas e adaptadores (arquitetura hexagonal)
* Módulos executam fora de processo do Core
* Sem código de terceiros no Core
* Implementações trocáveis a quente via interfaces

### Segurança sobre Capacidade

ANIMA prioriza segurança do usuário e considerações éticas sobre expandir funcionalidade.

* Permissões impostas no nível do motor
* Listas de allow/deny de capacidades
* Confirmação humana para ações perigosas
* Autorização baseada em lease (sem lease, sem execução)
* Falha fechada em incerteza
* Permissões explícitas, nunca implícitas

### Configurabilidade sobre Hardcoding

ANIMA enfatiza configuração externa em vez de comportamentos fixos.

* Personalidade definida por dados (Seeds), não código
* Políticas comportamentais configuráveis por instância
* Restrições de capacidade definidas declarativamente
* Políticas de memória configuráveis por seed
* Sem personalidade ou tom hardcoded

### Isolamento sobre Conveniência

ANIMA valoriza manter componentes e processos separados para aumentar segurança e confiabilidade.

* Memória com escopo de instância (sem compartilhamento entre instâncias)
* Limites de processo entre Core e Módulos
* Particionamento de execução para observabilidade
* Limites de lease criptográficos
* Isolamento de domínio (arquitetura hexagonal)

---

## Não-Objetivos (Explícitos)

Estes estão **explicitamente excluídos** do propósito da ANIMA:

### Não-Objetivos Técnicos

* **Sem código auto-modificável**
  * Código do motor permanece estável
  * Comportamento evolui através de memória e Seeds, não mutação de código
  * Sem geração dinâmica de código ou carregamento de plugins no Core

* **Sem autonomia descontrolada**
  * Todas as tarefas de longa duração são iniciadas pelo usuário
  * Tarefas são interruptíveis e auditáveis
  * Sem objetivos ou capacidades auto-expansivos
  * Restringido por permissões e Seeds

* **Sem acesso à internet a menos que concedido via capacidade**
  * Acesso à rede é opt-in, nunca padrão
  * Requer capacidade e permissão explícitas
  * Sujeito à imposição de lease e auditoria

* **Sem memória compartilhada entre instâncias**
  * Cada instância tem memória isolada, local à instância
  * Sem aprendizado entre usuários ou memória global
  * Sem compartilhamento de dados além do treinamento do modelo
  * Previne vazamento de identidade e privacidade

* **Sem permissões implícitas de usuário**
  * Todas as permissões devem ser explícitas
  * Capacidades declaradas e controladas
  * Ações perigosas requerem confirmação humana
  * Sem escalada silenciosa de privilégios

### Não-Objetivos Arquiteturais

* **Não é um chatbot de requisição-resposta**
  * Sistema cognitivo com estado de longa duração
  * Execução concorrente de tarefas
  * Dirigido por interrupções, não bloqueante

* **Não é um sistema monolítico**
  * Arquitetura modular, ports-and-adapters
  * Limites de domínio limpos
  * Infraestrutura substituível

* **Não é um serviço de nuvem por padrão**
  * Runtime privado, local-first
  * Usuário possui dados da instância
  * Sem telemetria ou upload de dados sem capacidade explícita

---

## Invariantes Arquiteturais

Estas restrições são **não-negociáveis** e impostas por design:

1. **Motor é agnóstico de identidade** — personalidade vem de Seeds, não código
2. **Core nunca carrega código de terceiros** — módulos executam fora de processo sobre gRPC+mTLS
3. **Toda execução requer leases válidos** — sem lease, sem execução (Zero-Lease State)
4. **Domínios nunca importam infraestrutura** — apenas portas/interfaces (arquitetura hexagonal)
5. **Toda observabilidade é baseada em eventos** — sem logs tradicionais, apenas fatos estruturados
6. **Memória é estritamente com escopo de instância** — sem compartilhamento entre instâncias jamais
7. **Cortex é obrigatório, Arcuate é opcional** — cognição essencial, linguagem é uma capacidade
8. **Core supervisiona, não executa** — modelo de kernel cognitivo
9. **Todas as ações são interruptíveis** — interrupção cooperativa por design
10. **Eventos são a fonte da verdade** — fatos imutáveis, auditáveis e com escopo de execução

---

## Critérios de Sucesso

ANIMA é bem-sucedida quando habilita:

* **Identidades de longa duração** que evoluem consistentemente ao longo do tempo
* **Interação segura** com o mundo através de capacidades auditáveis
* **Instâncias privadas** onde usuários possuem seus dados e memória
* **Múltiplas encarnações** de um único motor via Seeds
* **Confiança e transparência** através de comportamento observável e explicável
* **Estabilidade arquitetural** que suporta evolução de longo prazo

---

## Governança

Esta carta é **lei constitucional** para ANIMA.

* Todas as decisões arquiteturais devem se alinhar com estes valores
* Todas as funcionalidades devem ser justificáveis contra esta carta
* Todos os não-objetivos devem ser ativamente prevenidos por design
* Todas as invariantes devem ser impostas por arquitetura, não convenção

Violações desta carta são bugs arquiteturais, não requisições de funcionalidades.

---

## Resumo

> **ANIMA é um motor para identidades crescentes, não uma personalidade única.**
>
> **ANIMA não executa instruções. Ela supervisiona comportamento.**
>
> **ANIMA não registra texto. Ela registra fatos.**
>
> **ANIMA prioriza consistência, honestidade e segurança sobre capacidade.**
