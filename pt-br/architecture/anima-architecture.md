# 🧩 ANIMA — DIAGRAMA DE ARQUITETURA REFINADO (ALINHADO)

---

## 🌍 MUNDO EXTERNO

```
[ Usuário ]        [ Plataformas / Hardware / APIs ]
```

Nenhuma inteligência aqui. Apenas realidade.

---

## 🟦 CAMADA DE MÓDULOS (Fronteira com Efeitos)

> **Única camada que toca o mundo real**

```
┌───────────────────────────────────────────┐
│               MÓDULOS                     │
│                                           │
│  • Módulo de Entrada Discord              │
│  • Módulo de Entrada CLI                  │
│  • Módulo de Entrada Microfone            │
│                                           │
│  • Módulo de Saída Discord                │
│  • Módulo de Saída CLI                    │
│  • Módulo de Saída TTS                    │
│  • Módulo de Saída Live2D                 │
│                                           │
│  (APIs, hardware, streaming, dispositivos)│
└───────────────────────────────────────────┘
```

📌 Regra:

* Módulos **capturam** ou **executam**
* Módulos **não pensam**
* Módulos **não decidem**

---

## 🟨 CAMADA DE ADAPTADORES (Anel de Tradução Pura)

> **Primeiro anel protetor em torno do núcleo**

```
┌───────────────────────────────────────────┐
│               ADAPTADORES                 │
│                                           │
│  Adaptadores de Entrada:                  │
│   • Discord → EntradaNúcleo               │
│   • CLI → EntradaNúcleo                   │
│   • Mic → EntradaNúcleo                   │
│                                           │
│  Adaptadores de Saída:                    │
│   • Intenção → ComandoDiscord             │
│   • Intenção → ComandoTTS                 │
│   • Intenção → ComandoLive2D              │
│                                           │
│  (Puro, determinístico, sem I/O)          │
└───────────────────────────────────────────┘
```

📌 Regra:

* Adaptadores **apenas traduzem**
* Sem efeitos colaterais
* Sem memória
* Sem permissões


---

## 🟩 ANEL DE CAPACIDADES (Poder Declarativo)

> **O que o núcleo pode querer**

```
┌───────────────────────────────────────────┐
│              CAPACIDADES                  │
│                                           │
│  • enviar_texto                           │
│  • falar_áudio                            │
│  • renderizar_avatar                      │
│  • mover_robô                             │
│                                           │
│  (Contratos, não implementações)          │
└───────────────────────────────────────────┘
```

📌 Regra:

* Capacidades são simbólicas
* Seed + Segurança as controlam
* Elas não executam nada

---

## 🧠 NÚCLEO (Motor de Raciocínio)

> **O único lugar onde decisões são tomadas**

```
┌───────────────────────────────────────────┐
│                   NÚCLEO                  │
│                                           │
│  • Loop de Raciocínio                     │
│  • Planejamento de Intenção               │
│  • Gerenciamento de Tarefas               │
│  • Seleção de Capacidades                 │
│                                           │
│  Entradas:                                │
│   • EntradaNúcleo                         │
│   • Resultados de Consulta de Memória    │
│   • Restrições de Seed                    │
│   • Permissões                            │
│                                           │
│  Saída:                                   │
│   • Grafo de Intenção / Plano             │
└───────────────────────────────────────────┘
```

📌 Regra:

* Núcleo **nunca toca o mundo**
* Núcleo produz **intenção**, não efeitos

---

## 🟦 CONTEXTO INTERNO (Influência, Não Controle)

Estes cercam o núcleo mas **não executam**.

### 🧬 Seed (Identidade Estática)

```
┌───────────────────────────┐
│           SEED            │
│                           │
│  • Parâmetros de          │
│    personalidade          │
│  • Tom / expressividade   │
│  • Tolerância a risco     │
│  • Capacidades permitidas │
│  • Limites de identidade  │
└───────────────────────────┘
```

* Carregada na inicialização
* Imutável durante execução

---

### 🧠 Memória (Dinâmica, Falível)

```
┌───────────────────────────┐
│          MEMÓRIA          │
│                           │
│  • Interações passadas    │
│  • Observações            │
│  • Estados de tarefas     │
│  • Fatos ponderados por   │
│    confiança              │
└───────────────────────────┘
```

* Local à instância
* Consultada, nunca confiad a cegamente

---

## 🔐 SEGURANÇA E POLÍTICA (Transversal)

```
┌───────────────────────────┐
│          SEGURANÇA        │
│                           │
│  • Autenticação           │
│  • Autorização            │
│  • Aplicação de permissão │
│  • Portões de ação        │
│    perigosa               │
└───────────────────────────┘
```

Segurança:

* envolve **entrada antes do núcleo**
* valida **intenção antes da execução**

---

## 🔁 FLUXO COMPLETO (LIMPO E LINEAR)

```
Usuário
 ↓
Módulo de Entrada
 ↓
Adaptador de Entrada
 ↓
Autenticação / Segurança
 ↓
NÚCLEO
  ↔ Memória
  ↔ Seed
  ↔ Capacidades
 ↓
Intenção
 ↓
Adaptador de Saída
 ↓
Módulo de Saída
 ↓
Efeito
```

Sem atalhos. Sem vazamentos.

---

## 🧠 Teste Litmus Arquitetural
Pergunte:

* Posso simular tudo sem módulos? → Sim
* Posso trocar Discord por Slack sem tocar o núcleo? → Sim
* Posso executar múltiplas Seeds no mesmo motor? → Sim
* Posso auditar intenção antes da execução? → Sim
