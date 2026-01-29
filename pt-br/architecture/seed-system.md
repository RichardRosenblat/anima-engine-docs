# Sistema de Seeds — Inicialização de Identidade na ANIMA

**ADR Relacionado:** [ADR-001: Separação do Motor ANIMA e Identidade via Sistema de Seed](../adr/ADR-001.md)

---

## Visão Geral

O **Sistema de Seed** é o mecanismo da ANIMA para separar o motor da identidade. Um seed é uma configuração declarativa usada para inicializar uma identidade ANIMA, garantindo que o motor permaneça agnóstico de identidade enquanto permite personalidades diversas e customizáveis.

**Conceitos-Chave:**
* **ANIMA Seed** - Priors de identidade sem memória (configuração portátil e declarativa)
* **ANIMA Identity** - Seed + Memory (persistente, evolui ao longo do tempo)
* **ANIMA Instance** - Execução em andamento que hospeda uma Identity (efêmera)

**Sentença Canônica:** "Uma ANIMA Instance executa uma ANIMA Identity derivada de um ANIMA Seed e Memórias."

---

## O que é um Seed?

Um **Seed** é um blueprint estruturado, orientado a dados, que define:

* Parâmetros de personalidade (tom, verbosidade, expressividade emocional)
* Políticas comportamentais (tolerância a risco, tratamento de incerteza)
* Listas de allow/deny de capacidades
* Estilo de interação padrão
* Auto-conceito inicial e enquadramento narrativo
* Políticas de decaimento e promoção de memória

Um seed é **dados, não código**. Ele configura o motor sem modificar sua lógica.

Um Seed é **sem memória** - não contém experiências aprendidas, histórico de conversação ou comportamento evoluído.

---

## O que é uma ANIMA Identity?

Uma **ANIMA Identity** é uma identidade materializada composta por:
* **ANIMA Seed** (priors de identidade e configuração)
* **Memory** (camadas episódica, semântica e narrativa que evoluem ao longo do tempo)

Uma Identity é **persistente** entre execuções de Instance e **evolui** através da experiência.

---

## O que é uma ANIMA Instance?

Uma **ANIMA Instance** é uma única execução em andamento do motor ANIMA.

Uma Instance é **efêmera** - existe apenas durante a execução e é destruída no encerramento.

Uma Instance **hospeda** uma ANIMA Identity (Seed + Memory) e fornece o ciclo de vida de execução.

---

## O que um Seed NÃO Deve Incluir

Seeds são estritamente dados de inicialização. Eles NÃO DEVEM conter:

* Memórias aprendidas
* Dados do usuário
* Logs de conversação
* Evolução comportamental privada
* Lógica de execução
* Estado entre instâncias

---

## Motor vs. Identidade

### Motor ANIMA

O motor é um runtime privado responsável por:

* Compreensão e raciocínio de intenção
* Planejamento de tarefas e orquestração de tarefas de longa duração
* MTL (subsistema de memória; armazenamento, recuperação, decaimento, mecânicas de promoção)
* Descoberta, permissionamento e execução de capacidades
* Segurança, fluxos de confirmação e log de auditoria
* Manipulação de entrada/saída agnóstica de adaptador

O motor:

* Não codifica personalidade
* Não contém tom comportamental codificado
* Não armazena memória entre instâncias
* Impõe todos os limites de segurança e permissão

### Identidade ANIMA (via Seed)

Cada instância ANIMA é inicializada com um seed e evolui independentemente usando **apenas memória local da instância**.

Uma vez inicializada:

* A instância desenvolve suas próprias memórias
* O comportamento evolui dentro das restrições definidas pelo seed
* Nenhum compartilhamento ou aprendizado entre instâncias ocorre

---

## Invariantes Arquiteturais

As seguintes restrições agora são invariantes arquiteturais:

* O motor deve permanecer agnóstico de persona
* Seeds podem ajustar ou restringir comportamento mas não podem estender poder de execução
* A imposição de capacidades ocorre exclusivamente no nível do motor
* Memória é estritamente no escopo da instância
* Evolução de identidade deve ser explícita e revisável (sem desvio silencioso)

---

## Casos de Uso

### Produto ANIMA Privado

* Usuários licenciam o runtime do motor
* Usuários inicializam com um seed escolhido
* Usuários são proprietários dos dados e memória de sua instância
* Código-fonte do motor permanece privado

### ANIMA de Streaming / VTuber (ANIMA Prime)

* Implementado como `Motor ANIMA + Prime Seed`
* Prime seed é IP protegida e nunca distribuída
* Não compartilha memória ou estado de identidade com instâncias de clientes

---

## Benefícios

* Permite múltiplas instâncias ANIMA independentes com lógica de motor compartilhada
* Suporta comercialização sem distribuir código-fonte
* Previne vazamento de identidade e memória entre instâncias
* Permite uma identidade "Prime" protegida para uso em streaming / VTuber
* Torna a personalidade totalmente orientada a dados e configurável
* Simplifica testes, atualizações e evolução do motor

---

## Trabalho Futuro

* Definir schema de seed e estratégia de versionamento
* Implementar validação de seed e verificações de compatibilidade
* Formalizar garantias de atualização do motor entre versões de seed
* Definir mecanismos de licenciamento e controle de capacidades
