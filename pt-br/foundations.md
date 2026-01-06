# 🧭 ANIMA — FASE 0: FUNDAMENTOS

---

## 🎯 Objetivo

Estabelecer a **ANIMA como um motor de identidade**, não uma personalidade específica.

A ANIMA é:

* um **motor de IA privado**
* capaz de **identidade de longa duração**
* extensível através de **capacidades e módulos**
* moldada em tempo de execução por um **arquivo Seed**

A ANIMA **não** é:

* um chatbot
* um agente autônomo que se auto-expande
* uma plataforma de automação
* uma IA monolítica com comportamento codificado
* um sistema de rastreamento da internet por padrão

---

## 🧱 Construção (Entregáveis da Fase 0)

### 1️⃣ Carta do Projeto

Este documento responde *"o que existe"* e *"o que é proibido"*.

#### Propósito Central

* Fornecer um **runtime de IA privado e evolutivo**
* Separar **motor** de **identidade**
* Suportar **múltiplas encarnações** via Seeds
* Permitir **interação segura e auditável com o mundo**

#### Não-Objetivos (Explícitos)

* Sem código auto-modificável
* Sem autonomia descontrolada
* Sem acesso à internet a menos que concedido via capacidade
* Sem memória compartilhada entre instâncias
* Sem permissões implícitas do usuário

#### Valores Centrais

* **Verdade sobre confiança**  
  *O que isso significa?* O sistema prioriza precisão e honestidade em suas respostas, mesmo que isso signifique admitir incerteza ou falta de conhecimento.
* **Intenção sobre execução**  
  *O que isso significa?* O sistema foca em compreender e cumprir as intenções do usuário, em vez de apenas executar comandos cegamente.
* **Modularidade sobre monólito**  
  *O que isso significa?* O sistema é projetado com componentes intercambiáveis, permitindo flexibilidade e adaptabilidade em vez de ser uma entidade única e imutável.
* **Segurança sobre capacidade**  
  *O que isso significa?* O sistema prioriza a segurança do usuário e considerações éticas acima da expansão de suas funcionalidades ou capacidades.
* **Configurabilidade sobre codificação fixa**  
  *O que isso significa?* O sistema enfatiza a capacidade de ser personalizado e configurado através de configurações externas em vez de ter comportamentos fixos e codificados.
* **Isolamento sobre conveniência**
  *O que isso significa?* O sistema valoriza manter componentes e processos separados para melhorar a segurança e confiabilidade, mesmo que sacrifique alguma facilidade de uso.

Esta carta é sua *lei constitucional*.
Todo recurso deve ser justificável contra ela.

---

### 2️⃣ Glossário Canônico

Estas definições **nunca devem divergir**.

#### Motor (Engine)

A totalidade do sistema ANIMA, contendo:

* Núcleo (Core)
* Semente (Seed)
* Memória
* Capacidades
* Módulos
* Adaptadores
* Córtex

---

#### Núcleo (Core)

O loop de raciocínio dentro do motor.

Ele:

* consome entrada
* consulta memória
* aplica restrições da Seed
* seleciona capacidades
* produz **intenção**

---

#### Semente/Seed (Arquivo)

Um **artefato de configuração estático** carregado na inicialização.

Define:

* parâmetros de personalidade
* restrições comportamentais
* intervalos de tom e expressividade
* capacidades permitidas
* limites de identidade
* tolerância a riscos

Uma Seed:

* **não** contém memórias
* **não** se modifica
* **não** contém código

---

#### Memória

Dados locais da instância descrevendo:

* interações passadas
* observações
* estados de tarefas
* fatos ponderados por confiança

A Memória:

* informa o raciocínio
* nunca anula a política
* é falível e consultável

---

#### Capacidade

Um **contrato declarativo** descrevendo *o que o núcleo pode pretender*.

Exemplo:

* `enviar_mensagem`
* `mover_robô`
* `iniciar_stream`

Capacidades:

* não contêm lógica
* não contêm I/O
* são controladas por permissão
* são restritas pela Seed

---

#### Módulo

Uma **implementação com efeitos** de uma capacidade.

Módulos:

* executam ações no mundo real
* conversam com APIs, hardware, plataformas
* nunca decidem *quando* ou *por que*
* apenas executam *o que lhes é dito*

Módulos são o **único** lugar onde **Causa é detectada** e **Efeitos são produzidos**.

---

#### Adaptador

Uma **camada de tradução pura** entre representações.

Adaptadores:

* transformam entrada externa → entrada do núcleo
* transformam intenção do núcleo → comando do módulo
* não contêm I/O externo
* contêm apenas lógica de tradução
* são determinísticos

Adaptadores existem para **proteger o núcleo da poluição de formato**.

---

#### Intenção (Intent)

Uma descrição estruturada de **o que deve acontecer**, não como.

Produzida pelo núcleo.  
Consumida por adaptadores e módulos.  
Auditável, registrável, reproduzível.  
Contém o que + quando + onde + quanto + por que + o que fazer se algo der errado. Junto com pontuações de confiança.

---

#### Tarefa (Task)

Uma unidade de trabalho de longa duração que o motor realiza. Resolvida com uma série de Intenções.

Tarefas:

* persistem ao longo do tempo
* podem pausar / retomar
* podem invocar capacidades repetidamente
* são rastreadas na memória

---

#### Córtex (Cortex)

O wrapper em torno de um modelo de IA específico, conectado ao motor para raciocínio.

Córtices:
* fornecem serviços de conclusão
* são intercambiáveis sem precisar mudar o motor
* são substituíveis

---

#### Pacote (Package)

Um grupo distribuível de módulos, adaptadores e definições de capacidade.
Pode ser instalado em uma instância ANIMA para estender a funcionalidade em massa.

Pacotes:

* agrupam capacidades relacionadas
* incluem adaptadores para essas capacidades
* são versionados
* podem ser compartilhados

---

#### Espinha Semântica (Semantic Spine)

Uma estrutura de dados explícita para representação semântica de uma mensagem que se espera ser passada ao usuário ou recebida do usuário.

As Espinhas Semânticas são usadas para garantir comunicação consistente e significativa entre o motor e os usuários, fornecendo uma maneira padronizada de representar o significado e contexto das mensagens.

Espinhas Semânticas:
* encapsulam intenção e contexto
* facilitam interpretação precisa
* são agnósticas em relação à linguagem
* suportam interações complexas
* permitem melhor codificação de memória

### 3️⃣ **Limites do Sistema**

#### O que o motor *nunca* pode fazer
* Executar efeitos colaterais diretamente
* Modificar seu próprio código ou Seed
* Acessar a internet sem capacidade explícita
* Compartilhar memória entre instâncias
* Ignorar verificações de permissão

#### O que deve *sempre* exigir confirmação
* Acessar dados sensíveis do usuário
* Executar capacidades de alto risco (ex: transações financeiras, ações físicas)
* Lidar com comandos destrutivos (ex: excluir dados, desligar sistemas)
* Substituir dados
* Ações irreversíveis não somente de leitura

#### O que é delegado aos módulos

* Todas as operações de I/O externo
* Chamadas de API
* Interações com hardware
* Recebimento de comandos e informações do usuário
* Execução de comandos de capacidade

---

## ✅ Critérios de Saída (NÃO Avance Sem Estes)

**Você pode explicar a ANIMA em 2 minutos sem mencionar personalidade**

ANIMA é um motor de IA privado projetado para hospedar identidades de IA de longa duração e em evolução de forma segura.

Em seu núcleo, a ANIMA separa pensamento, identidade e ação.

O núcleo é a única parte que raciocina. Ele recebe entrada estruturada, consulta memória, aplica restrições de identidade de um arquivo Seed, verifica permissões e produz intenção—nunca ações diretas.

Uma Seed é uma definição de identidade estática: parâmetros de personalidade, limites comportamentais, tolerância a riscos e quais capacidades são permitidas. Não contém memórias ou código. Cada instância ANIMA cresce independentemente após a inicialização.

A memória é local à instância e falível. Armazena interações passadas, estados de tarefas e observações, e informa decisões sem anular a política.

O núcleo só pode agir através de capacidades, que são contratos declarativos descrevendo o que pode fazer, não como. As capacidades são controladas tanto pela Seed quanto pelas regras de segurança.

Quando o núcleo produz intenção, os adaptadores traduzem essa intenção em comandos concretos. Adaptadores são puros e determinísticos—não fazem I/O externo nem tomam decisões.

A interação real com o mundo acontece apenas nos módulos. Módulos conversam com APIs, hardware, plataformas ou streams, e executam comandos sem raciocinar.

A segurança envolve o sistema de ponta a ponta: autenticação antes do raciocínio e aplicação de política antes da execução.

Este design permite que a ANIMA suporte assistentes privados, personas de stream, robôs e ferramentas—todos usando o mesmo motor—mantendo a identidade isolada, o comportamento auditável e as ações seguras.

---

## ⚠️ Armadilhas da Fase 0

* Escrever código sem separação clara de responsabilidades
* Deixar módulos decidirem comportamento
* Deixar a memória anular a política
* Confundir Seed com Memória
* Tratar adaptadores como opcionais


