---
tipo:
  - conceito
dominio: IA
Subdominio: agent-frameworks
tags:
  - IA
  - programação
  - inovação
---
# Framework de Agentes — Economia Ótima de Poder Computacional
### Documento de Arquitetura · Projeto novo, independente do "Hello, LLM!"

---

## 1. Princípio fundamental

Todo componente deste framework existe para responder a uma pergunta só, antes de gastar um token:

> **"Esse gasto é necessário, ou é gordura?"**

Isso não é um objetivo secundário do framework — é o objetivo primário. Performance e qualidade de resposta continuam importando, mas a diferença deste framework pra um orquestrador de agentes genérico é que aqui **custo é um cidadão de primeira classe**, medido e decidido em cada etapa, não uma preocupação de depois.

Assunção de stack: Python, seguindo a mesma linguagem que você já vem aprendendo. Se quiser mudar depois, a arquitetura abaixo não depende de detalhes de sintaxe — só de conceitos (classes abstratas, composição).

---

## 2. Componentes centrais

### 2.1 `Agent`
Representa um papel com responsabilidade **bem definida e estreita** (o "trabalhador especializado" da fábrica de alfinetes). Cada `Agent` carrega:
- Um nome/papel (ex: "Estrategista de Preço")
- Um `system_prompt` mínimo — só o necessário pro papel, nada herdado de outros agentes
- Um modelo atribuído (decidido pelo `ModelRouter`, não fixo no agente)
- Uma lista de ferramentas relevantes só pro seu escopo (nunca as 20 ferramentas do sistema inteiro — voltando ao princípio do "Tool Calling: exponha só o necessário" que já vimos)

### 2.2 `ModelRouter`
Decide **qual modelo** cada chamada usa, com base na complexidade da tarefa — não no papel do agente. Um "Estrategista de Preço" pode usar modelo pequeno pra cálculo de rotina e escalar pra modelo grande só quando a tarefa sinalizar ambiguidade real (ex: dados conflitantes, decisão sem regra clara).

Isso desacopla "quem é o agente" de "quanto ele custa" — a mesma decisão de roteamento que discutimos lá atrás, só que formalizada como componente reutilizável.

### 2.3 `ContextScope`
O guardião do `n`. Antes de qualquer chamada, o `ContextScope` decide **o que exatamente** entra no prompt daquele agente — nunca "tudo que existe", só o que aquele papel específico precisa. Tem um orçamento de tokens (`budget`) por agente, e corta/resume agressivamente o que ultrapassa esse orçamento.

Esse é o componente que aplica na prática a lição da festa: cada agente vê só os crachás relevantes pro seu trabalho, não o salão inteiro.

### 2.4 `HandoffContract`
Em vez de um agente passar um resumo livre pro próximo (prosa solta, sujeita a "telefone sem fio"), a comunicação entre agentes segue um **schema estruturado obrigatório** — campos fixos que nunca podem ficar vazios:

```
{
  "resultado_principal": ...,
  "dados_criticos": [...],
  "fonte_ou_evidencia": ...,
  "incertezas_abertas": [...]
}
```

Isso ataca dois problemas ao mesmo tempo: reduz tokens de saída (formato estruturado é mais enxuto que prosa) e reduz perda de informação no handoff (nada crítico pode "esquecer" de ser mencionado, porque o campo é obrigatório).

### 2.5 `Orchestrator`
Executa o grafo de dependência entre agentes. Sabe distinguir:
- **Sequencial** — quando um agente depende do output do outro (Pesquisa → Objetivos)
- **Paralelo** — quando agentes são "irmãos" independentes (Produto, Preço, Promoção, Praça rodando ao mesmo tempo)

O `Orchestrator` não decide *quantos* agentes existem — só executa o grafo que foi desenhado. Quem decide "quantos agentes faz sentido ter" é o próximo componente.

### 2.6 `CostTracker`
Instrumenta **toda** chamada: tokens de entrada, tokens de saída, modelo usado, tempo de resposta. Não é telemetria decorativa — é o dado bruto que alimenta o componente mais importante do framework:

### 2.7 `FragmentationAdvisor`
Este é o componente que faz o framework ser *seu* e não um orquestrador genérico. Ele aplica, com números reais coletados pelo `CostTracker`, a mesma lógica que chegamos discutindo Smith e Coase:

```
Custo_total(k) ≈ n²/k + k·B + (k−1)·H
```

Onde, medidos empiricamente pelo `CostTracker`:
- `B` = overhead médio fixo por agente (system prompt + ferramentas + boilerplate)
- `H` = custo médio de cada handoff (tokens gastos formatando/interpretando o `HandoffContract`)
- `n` = tamanho total do contexto da tarefa, antes de fragmentar

O `FragmentationAdvisor` não decide sozinho — ele **recomenda**: "com os números atuais, fragmentar esse fluxo em mais um agente provavelmente aumenta o custo total" ou o contrário. Você decide se aplica.

*(Lembrando a ressalva que já fizemos: a fórmula é ilustrativa da forma da curva, não literal em dólares. O valor real do `FragmentationAdvisor` é comparar execuções passadas entre si — k=1 vs k=2 vs k=4 — não prever um número absoluto.)*

### 2.8 `PromptCache`
Camada que identifica blocos repetidos entre chamadas (system prompts fixos, definições de ferramentas, contexto-base do domínio) e os marca para cache, evitando reprocessamento pago a cada chamada.

### 2.9 `KnowledgeConnector` (ponto de extensão, não implementado ainda)
Uma interface abstrata — proposital e propositalmente vazia por enquanto — pela qual, no futuro, qualquer agente poderia consultar uma fonte de conhecimento externa (o RAG hierárquico do Second Brain, por exemplo) sem o framework precisar saber os detalhes de implementação daquela fonte. Como você decidiu não fixar caso de uso ainda, esse componente fica como uma "tomada" pronta pra ser usada mais tarde, sem forçar a forma final agora.

### 2.10 `TaskClassifier` *(adicionado na revisão — fechava uma lacuna)*
O `ModelRouter` (2.2) decide "com base na complexidade da tarefa" — mas nada no desenho original definia quem *mede* essa complexidade. O `TaskClassifier` fecha isso: normalmente um modelo pequeno/barato (ou até regras heurísticas simples, tipo "a tarefa tem fontes conflitantes?", "quantos passos de raciocínio o histórico sugere?") que gera um sinal de complexidade, consumido pelo `ModelRouter` antes dele escolher entre modelo caro ou barato.

Vale notar a recursividade bonita aqui: rodar um classificador barato pra decidir se vale a pena um modelo caro é o próprio princípio do framework (seção 1) se aplicando a si mesmo.

### 2.11 `CostGuardrail` *(adicionado na revisão)*
Um teto de gasto por tarefa/sessão inteira — diferente do `ContextScope` (que limita tokens por chamada individual), este é um limite **global**, acumulado pelo `CostTracker`, que interrompe a execução inteira se o custo ultrapassar um valor definido por você. É a resposta prática pra "economia que não gasta a ponto de falir alguém" que você levantou — um circuit breaker ativo, não só uma métrica passiva que você olha depois do fato.

---

## 3. Fluxo de execução (visão geral)

1. Uma tarefa chega ao `Orchestrator`.
2. Pra cada agente no grafo, o `ModelRouter` decide o modelo.
3. O `ContextScope` monta o contexto mínimo necessário pra aquele agente específico.
4. O `PromptCache` verifica se partes desse contexto já estão cacheadas.
5. A chamada acontece; o `CostTracker` registra tudo.
6. O output vira um `HandoffContract` estruturado, passado ao(s) próximo(s) agente(s) — em paralelo quando não há dependência entre eles.
7. Periodicamente (ou sob demanda), o `FragmentationAdvisor` analisa o histórico do `CostTracker` e sugere ajustes na granularidade do grafo de agentes.

Veja o diagrama complementar (`framework_agentes_fluxo.mermaid`) pra visualizar essas relações.

---

## 4. De onde cada peça veio (mapa de rastreabilidade)

| Componente | Conceito da conversa que originou |
|---|---|
| `ModelRouter` | Roteamento por modelo — "nem toda tarefa precisa do modelo caro" |
| `ContextScope` | Eixo de custo de entrada (O(n²)) + escopo mínimo por agente |
| `HandoffContract` | Correção do "telefone sem fio" + nota `sintese` do Second Brain (resolver antes, não inferir depois) |
| `PromptCache` | Prompt caching discutido no fluxo de decisão original |
| `CostTracker` | Necessidade de medir em vez de assumir (você mesmo apontou isso) |
| `FragmentationAdvisor` | A fórmula `k_ótimo`, Adam Smith (especialização) e Coase (custo de transação) |
| `KnowledgeConnector` | RAG hierárquico do Second Brain (tier/status/autoridade), deixado como extensão futura |
| `TaskClassifier` | Lacuna encontrada na revisão: "complexidade" nunca tinha um responsável por medi-la |
| `CostGuardrail` | Sua preocupação explícita: "economia ótima, nem a ponto de falir alguém" |

---

## 5. O que este documento propositalmente NÃO define ainda

- Nome do projeto/pacote
- Estrutura exata de pastas
- Se roda em processo único ou distribuído
- Qual caso de uso concreto valida o framework primeiro

Essas decisões ficam pro próximo passo, depois que a arquitetura acima estiver validada com você.

## 6. Próximos passos sugeridos

1. Validar/ajustar os 9 componentes acima — algum sobra, algum falta?
2. Escolher um nome pro projeto.
3. Desenhar a primeira classe abstrata (provavelmente `Agent`, usando `abc`, do mesmo jeito que fizemos no Hello, LLM!) — só depois de aprovado o desenho geral.
