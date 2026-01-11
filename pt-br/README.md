# Documentação do ANIMA Engine

![Status](https://img.shields.io/badge/status-desenvolvimento%20ativo-blue)
![Architecture](https://img.shields.io/badge/architecture-hexagonal%20%2F%20ports--and--adapters-purple)

**ANIMA** é um motor de IA privado e modular projetado para hospedar identidades artificiais de longa duração e em evolução, sob restrições rígidas de segurança, memória e capacidades.

Não é um chatbot, não é um wrapper de prompt e não é uma personalidade única.

ANIMA é um **motor para cultivar identidades**.

---

## Princípios Fundamentais

* **Motor ≠ Identidade**
  Raciocínio e execução são estritamente separados de personalidade e comportamento.

* **Privado por Design**
  Cada instância ANIMA possui sua própria memória e evolui independentemente.

* **Segurança sobre Capacidade**
  Permissões, confirmações e limites são aplicados no nível do motor.

* **Modularidade em Primeiro Lugar**
  Entradas, saídas, capacidades e incorporação são conectáveis.

* **Continuidade sobre Recordação**
  ANIMA prioriza identidade consistente e confiança sobre memória perfeita.

---

## Visão Geral da Arquitetura

ANIMA segue uma arquitetura hexagonal / portas-e-adaptadores.

```
┌────────────────────┐
│  Módulos de Entrada │  (texto, voz, discord, sensores)
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│   Motor ANIMA      │
│                    │
│  - Raciocínio      │
│  - Planejamento    │
│  - Sistema de      │
│    Tarefas         │
│  - Camadas de      │
│    Memória         │
│  - Segurança       │
│  - Capacidades     │
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│    Adaptadores     │  (Conversão de intenção para comando)
└────────────────────┘
          │
┌─────────▼──────────┐
│ Módulos de         │  (ferramentas, streaming, robôs, etc)
│ Capacidade         │
└────────────────────┘
```

O motor **não tem conhecimento** de plataformas, personalidades ou incorporação.

---

## O Sistema de Seeds

Um **Seed** define uma identidade ANIMA.

Um seed é **dados**, não código.

### Um seed pode definir:

* Parâmetros de personalidade (tom, verbosidade, expressividade emocional)
* Políticas comportamentais (tolerância a riscos, tratamento de incertezas)
* Listas de permissão/negação de capacidades
* Estilo de interação padrão
* Enquadramento narrativo inicial
* Regras de decaimento e promoção de memória

### Um seed não deve incluir:

* Memórias aprendidas
* Logs de conversação
* Dados de usuário
* Lógica de execução
* Estado entre instâncias

Após a inicialização, cada instância ANIMA desenvolve **memória exclusiva da instância**.

---

## ANIMA Prime vs Instâncias de Usuário

* **ANIMA Prime**
  Uma identidade protegida usada para incorporação em streaming / VTuber.
  Implementada como:

  ```
  Motor ANIMA + Seed Prime
  ```

  O Seed Prime nunca é distribuído.

* **Instâncias ANIMA de Usuário**
  Instâncias privadas inicializadas com seeds selecionados pelo usuário.
  Os usuários possuem a memória e a evolução de suas instâncias.

Todas as instâncias executam no **mesmo motor**, mas permanecem totalmente isoladas.

---

## Modelo de Memória

ANIMA usa memória em camadas para equilibrar realismo, segurança e custo.

Tipos de memória, políticas de promoção e decaimento especificadas no [documento de integridade de memória](safety/memory-integrity.md)

ANIMA rastreia se o conhecimento é:

* observado
* lembrado
* inferido
* desconhecido

Ela nunca supõe silenciosamente.

---

## Capacidades

Capacidades são módulos explícitos e descobríveis.

O motor:

* raciocina sobre capacidades
* verifica permissões
* solicita confirmação quando necessário
* executa em uma sandbox controlada

Seeds podem **restringir** capacidades, mas nunca conceder novo poder de execução.

---

## Segurança & Permissões

* Identidades de usuário estáveis
* Acesso baseado em função
* Verificações de permissão em nível de capacidade
* Confirmação humana para ações perigosas
* Registro de auditoria completo

ANIMA explica *por que* ela pode ou não pode agir.

---

## O que ANIMA Não É

* ❌ Um agente autônomo de propósito geral
* ❌ Uma IA conectada à internet por padrão
* ❌ Um sistema de memória compartilhada
* ❌ Uma personalidade codificada em prompts
* ❌ Uma substituição para o julgamento humano

---

## Status do Projeto

ANIMA está em desenvolvimento ativo.

Foco atual:

* Correção do motor central
* Integridade da memória
* Limites de capacidades
* Validação de seeds

Este projeto está evoluindo intencionalmente de forma lenta.

---

## Documentos de Design

Decisões arquiteturais importantes são rastreadas como ADRs em [`/adr`](adr):

* ADR-001: Separação do Motor / Identidade através do Sistema de Seeds
* ADR-002: Esquemas Auto-Validáveis com Restrições Somente Internas
* ADR-003: Protocolo de Comunicação Core ↔ Módulo, Tipos de Módulo e Ciclo de Vida de Lease
* ADR-004: Observabilidade, Registro de Eventos e Rastreabilidade de Execução
* ADR-005: Modelo de Interrupção e Preempção
* ADR-006: Regras de Dependência e Compartilhamento de Domínio
* ADR-007: Camada de Infraestrutura e Regras de Adaptador
* ADR-008: ANIMA Core se Comporta como um Kernel Cognitivo
* ADR-009: Topologia de Modelo de IA Configurável com Córtex Obrigatório e Arcuate Opcional
* ADR-010: Divisão Adaptador–Atuador com Fronteira de Processo Estrita
* ADR-011: Arquitetura de Entrada Baseada em Eventos

### Documentação de Arquitetura

Documentação abrangente de arquitetura derivada dos ADRs:

* [Visão Geral da Arquitetura](architecture/directory-overview.md) - Guia de navegação completo
* [Arquitetura ANIMA](architecture/anima-architecture.md) - Visão geral abrangente do sistema
* [Sistema de Seeds](architecture/seed-system.md) - Inicialização e separação de identidade
* [Tipos de Módulo e Leases](architecture/module-types-and-leases.md) - Ciclo de vida e autorização de módulos
* [Arquitetura de Eventos](architecture/event-architecture.md) - Observabilidade e sistema de entrada
* [Kernel Cognitivo](architecture/cognitive-kernel.md) - Core como supervisor multitarefa
* [Domínio e Infraestrutura](architecture/domain-and-infrastructure.md) - Implementação de arquitetura hexagonal
* [Topologia do Modelo de IA](architecture/ai-model-topology.md) - Córtex e Arcuate
* [Divisão Adaptador-Atuador](architecture/adapter-actuator-split.md) - Estrutura de módulo

### Documentos Fundamentais

Princípios constitucionais e limites:

* [Glossário Canônico](foundations/canonical-glossary.md) - Terminologia e conceitos definitivos
* [Limites do Sistema](foundations/system-boundaries.md) - O que ANIMA pode e não pode fazer

### Documentação Adicional

* [Visão](vision/vision.md) - Visão e objetivos de longo prazo
* [Carta do Projeto](vision/project-charter.md) - Propósito central, valores e não-objetivos
* [Não-Objetivos](vision/non-goals.md) - Não-objetivos e limites explícitos
* [Integridade da Memória](safety/memory-integrity.md) - Gerenciamento e integridade da memória
* [Modelo de Segurança](safety/safety-model.md) - Arquitetura de segurança
* [Modelo de Ameaças](safety/threat-model.md) - Análise e mitigação de ameaças
* [Modelo de Licenciamento](governance/licensing-model.md) - Licenciamento e distribuição
* [Roadmap](roadmaps/roadmap.md) - Roteiro de desenvolvimento
* [Anúncios](announcements) - Anúncios e atualizações do projeto
* [Especificações](specs) - Especificações técnicas

---

## Filosofia

ANIMA não é projetada para parecer inteligente a todo custo.

Ela é projetada para parecer **consistente, honesta e segura para crescer junto**.

---

## Perguntas Frequentes (FAQ)

### O que é ANIMA, exatamente?

ANIMA é um **motor de IA privado e modular** projetado para hospedar identidades artificiais de longa duração e em evolução sob restrições estritas de segurança, memória e capacidades.

O que isso significa é que você pode criar sua própria identidade de IA única fornecendo um **Seed** e dando a ela capacidades específicas, garantindo ao mesmo tempo que ela opere com segurança e respeite sua privacidade.

Ela **não é**:

* um chatbot
* uma personalidade única
* um agente autônomo que age sozinho

ANIMA é um *motor para cultivar identidades*, não um personagem pré-embalado.

---

### Onde posso encontrar o motor ANIMA?

O motor ANIMA está em um **repositório separado** chamado `engine-core`.

Esse repositório está atualmente **não público** enquanto os limites centrais de segurança, memória e identidade ainda estão em desenvolvimento.

Este repositório de documentação pública existe para:

* explicar a arquitetura
* documentar decisões de design
* tornar a visão de longo prazo transparente

Uma vez que o motor atinja fases estáveis, mais partes do ecossistema podem ser abertas.

---

### Como faço para baixar ou executar ANIMA?

No momento, ANIMA **ainda não está disponível para download**.

Quando lançado:

* ANIMA será distribuído como um runtime de motor
* os usuários inicializarão suas próprias instâncias privadas usando seeds aprovados
* a memória sempre permanecerá local da instância

Detalhes sobre instalação e plataformas suportadas serão publicados mais perto do lançamento.

---

### ANIMA será gratuito uma vez lançado?

O plano atual é um **modelo misto**:

* Alguns componentes (documentação, formatos de seed, adaptadores) podem ser públicos
* Executar o próprio motor ANIMA provavelmente exigirá uma licença ou assinatura
* Certos seeds curados podem ser premium

ANIMA é projetada como um **sistema de longo prazo**, não um aplicativo único, e a sustentabilidade é uma consideração central.

O preço exato ainda não está finalizado.

---

### ANIMA é segura para usar?

Segurança é um **requisito arquitetural central**, não uma reflexão tardia.

ANIMA impõe:

* permissões explícitas
* listas de permissão/negação de capacidades
* confirmação humana para ações perigosas
* registro de auditoria
* separação estrita entre identidade e execução

Se ANIMA não puder executar uma ação com segurança, ela deve explicar *por que* e recusar.

---

### ANIMA pode excluir meus arquivos?

Não, a menos que você permita explicitamente.

ANIMA:

* **não tem acesso ao seu sistema por padrão**
* não pode executar capacidades que não possui
* não pode escalar permissões por conta própria

Qualquer capacidade que possa afetar arquivos, dispositivos ou sistemas deve ser:

1. explicitamente instalada
2. explicitamente permitida pelo seed
3. explicitamente permitida pelo usuário por pelo menos dois canais separados
4. confirmada em tempo de execução se perigosa

---

### ANIMA pode me espionar, gravar ou acessar a internet?

Não — não por padrão.

ANIMA:

* não tem acesso à internet a menos que uma capacidade o forneça
* não grava áudio ou vídeo a menos que um adaptador esteja instalado
* não compartilha memória entre instâncias
* não envia seus dados para instâncias de outros usuários
* não monitora você passivamente a menos que você peça explicitamente

Se ANIMA puder perceber algo, deve ser porque você **conectou explicitamente** essa entrada e disse a ela para usá-la.

---

### ANIMA compartilha memória entre usuários?

Não. Nunca.

Cada instância ANIMA:

* tem sua própria memória isolada
* evolui independentemente
* não pode acessar ou influenciar outras instâncias

Não há aprendizado entre usuários ou "memória global" compartilhada além do treinamento do modelo.

---

### ANIMA vai alucinar ou inventar coisas?

ANIMA é projetada para **evitar alucinações silenciosas**.

O motor rastreia se a informação é:

* observada
* lembrada
* inferida
* desconhecida

Se ANIMA não tiver certeza, espera-se que ela diga.

Precisão perfeita não é garantida, mas **fabricação confiante é tratada como um bug**.

---

### ANIMA é um agente autônomo?

Não.

ANIMA não:

* se auto-implanta
* auto-modifica seu núcleo
* persegue objetivos sem envolvimento do usuário
* age sem intenção e permissão explícitas

Tarefas de longa duração (como streaming ou monitoramento) existem, mas são:

* iniciadas pelo usuário
* interrompíveis
* auditáveis
* limitadas por permissões

---

### O que é um "Seed"?

Um seed é um **modelo de identidade estruturado** usado apenas na inicialização.

Ele define:

* parâmetros de personalidade
* restrições comportamentais
* capacidades permitidas
* enquadramento narrativo inicial

Um seed **não** inclui:

* memórias
* histórico de conversação
* comportamento aprendido
* lógica de execução

Após a inicialização, cada instância cultiva sua própria identidade através da experiência.

---

### O que é ANIMA Prime?

ANIMA Prime é uma **identidade interna protegida futura** usada para incorporação em streaming / VTuber.

Ela executará no mesmo motor que todas as outras instâncias, mas usa um seed privado e não distribuído.

ANIMA Prime não será vendida ou compartilhada.

---

### ANIMA é código aberto?

Partes do ecossistema ANIMA podem ser abertas.

O núcleo do motor em si está atualmente **fechado** para garantir:

* segurança de identidade
* integridade da memória
* evolução controlada de capacidades

Isso pode mudar com o tempo, mas o motor nunca será lançado de uma forma que comprometa a confiança ou privacidade do usuário.

---

### Quando ANIMA será lançada?

ANIMA está em desenvolvimento ativo, mas não há **data de lançamento fixa** ainda.

O foco atual é em:
* Correção do motor central
* Integridade da memória
* Limites de capacidades
* Validação de seeds
* Recursos de segurança

Anúncios futuros serão feitos à medida que marcos forem alcançados e fases estáveis forem aproximadas.
Demos serão compartilhadas quando apropriado.

---

### Por que o desenvolvimento é lento?

Intencionalmente.

ANIMA prioriza:

* correção sobre velocidade
* segurança sobre novidade
* confiança de longo prazo sobre demos de curto prazo

Algumas coisas são fáceis de construir rapidamente — e muito difíceis de consertar depois.
ANIMA está sendo construída para durar.

---

### Como posso acompanhar o desenvolvimento?

Atualizações, decisões arquiteturais e filosofia são documentadas neste repositório.

Marcos importantes serão anunciados uma vez que o motor atinja fases estáveis.

Anúncios serão adicionados na pasta `/announcements` e compartilhados através de canais oficiais futuros.

---

**Veja também**: [Documentação completa em Inglês](../README.md)
