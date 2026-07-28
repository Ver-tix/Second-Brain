---
tags:
  - IA
  - programação
  - inovação
---
# Resolução — Desafio Prometheus #008

> Aula 8 — *Agentes de IA: Quando o Sistema Começa a Planejar*
> Último desafio do Módulo 4.
> Mentor: `prometheus-mentor`

Antes de começar, uma orientação de método: não leia este documento como "a resposta certa". Leia como **uma linha de raciocínio**. O objetivo do Projeto Prometheus é que você chegue sozinho a essa arquitetura. Use este material para **conferir** o seu próprio pensamento e para preencher lacunas — não para substituir o exercício de pensar.

---

## Questão 1

> **Por que um agente de IA não pode ser definido apenas como "um LLM com ferramentas"?**

Para responder, precisamos construir quatro conceitos. Vou montar cada um do simples para o complexo, na metodologia do mentor (definir → intuitivo → técnico → exemplo → onde aparece → erros comuns → conexão).

A tese central, adiantada:

> Um LLM com ferramentas executa **uma** ação quando mandam. Um agente **decide** quantas ações executar, em que ordem, e quando parar — perseguindo um **objetivo**. A diferença não é "mais inteligente", é **ter um loop de decisão**.

---

### 1.1 — Planejamento

**Definição.** Planejar é decompor um objetivo complexo em uma sequência de passos menores e ordenados antes de agir.

**Intuitivamente.** Imagine o pedido:

> "Organize minha viagem para o Chile considerando orçamento, hotéis, voos e roteiro."

Ninguém "responde" isso num tiro só. Você naturalmente quebra em partes: *primeiro vejo voos, depois vejo o que cabe no orçamento, depois hotel, depois roteiro*. Isso é planejamento.

**Tecnicamente.** O sistema recebe um objetivo de alto nível e precisa produzir (explícita ou implicitamente) uma estrutura do tipo:

```text
objetivo  →  subobjetivo_1  →  subobjetivo_2  →  ...  →  objetivo concluído
```

Um LLM com ferramentas **não faz isso**. Ele recebe um pedido, dispara no máximo uma ou poucas ferramentas sob comando direto, e devolve uma resposta. Não há decomposição autônoma.

**Exemplo.**
- LLM + ferramentas: *"Qual a cotação da PETR4?"* → chama `cotacao("PETR4")` → responde.
- Agente: *"Rebalanceie minha carteira"* → planeja: consultar cotações → consultar alocação atual → calcular desvio → decidir compras/vendas → emitir ordens → confirmar.

**Onde aparece.** É o coração dos frameworks de agentes (LangGraph, AutoGen, CrewAI). O "planner" costuma ser o primeiro componente da arquitetura.

**Erro comum.** Confundir *planejamento* com *prompt grande*. Um prompt de mil linhas não torna nada um agente. Planejamento é **estrutura de execução**, não tamanho de instrução.

**Conexão.** O planejamento é exatamente o que faltava desde a Aula sobre Prompt Engineering: o prompt direciona **uma** inferência; o agente orquestra **várias** inferências com propósito.

---

### 1.2 — Ciclo de decisão (o loop)

**Definição.** Ciclo de decisão é um loop em que o sistema raciocina, age, observa o resultado e decide o próximo passo — repetindo até concluir.

**Intuitivamente.** É a diferença entre uma **pergunta** e uma **tarefa**. Pergunta se responde; tarefa se **cumpre em etapas**.

**Tecnicamente.** O padrão clássico é o **ReAct** (Reason → Act → Observe):

```text
Pensar (Reason)
    ↓
Agir  (Act — chamar uma ferramenta)
    ↓
Observar (Observe — ler o resultado)
    ↓
Pensar novamente  ←  loop
    ↓
...
    ↓
Resposta final
```

**Exemplo.** Pedido: *"Qual ação caiu mais hoje?"*

```text
Pensamento:  "Preciso das cotações de hoje."
Ação:        consultar_mercado()
Observação:  { PETR4: -3%, VALE3: -1%, ... }
Pensamento:  "Agora comparo para achar a maior queda."
Ação:        (raciocínio interno / ordenação)
Resposta:    "PETR4 caiu mais, -3%."
```

Observe: o sistema **não sabia de antemão** quantas vezes precisaria pensar. O número de iterações é decidido **durante** a execução.

**Por que "LLM + ferramentas" não é isso.** No Tool Calling puro, o fluxo é uma sequência linear pré-determinada pelo orquestrador: `pergunta → ferramenta → LLM → resposta`. Não há decisão de "voltar a pensar", "tentar outra ferramenta" ou "repetir". É um **caminho fixo**, não um **loop**.

**Erro comum.** Achar que chamar duas ferramentas seguidas já é um agente. Se a sequência é fixa e não há decisão sobre *o que fazer em seguida com base no resultado*, é um pipeline, não um agente.

**Conexão.** Esse loop é o "salto" da Aula 7 (Tool Calling) para a Aula 8 (Agentes). A ferramenta continua a mesma; o que muda é **quem decide a próxima jogada**.

---

### 1.3 — Estado

**Definição.** Estado é toda a informação que o sistema carrega **durante** a execução de uma tarefa: onde está, o que já fez, o que descobriu, o que ainda falta.

**Intuitivamente.** É a "memória de trabalho" — o bloco de rascunho onde você anota "*já vi voos, falta hotel*" enquanto organiza a viagem.

**Tecnicamente.** O estado de um agente costuma incluir:

- **histórico da tarefa** (quais ferramentas já rodaram, com quais resultados);
- **memória temporária** (dados parciais coletados);
- **posição no plano** (em qual subobjetivo estou);
- **variáveis de controle** (iteração atual, erros acumulados, limite de tempo).

**Exemplo.** Para o pedido *"gere um relatório financeiro"*, o estado guarda:

```text
estado = {
  passo_atual: "gerar_relatorio",
  dados_coletados: { vendas: [...], despesas: [...] },
  tentativas: 1,
  erros: [],
}
```

Se a ferramenta de despesas falhar, o estado permite **continuar de onde parou** em vez de recomeçar.

**Por que "LLM + ferramentas" não tem isso.** Uma chamada isolada de ferramenta é *stateless* por natureza: entra um pedido, sai uma resposta, nada é lembrado entre chamadas (a menos que um **orquestrador externo** mantenha o estado — e aí já começamos a sair do "puro LLM + ferramentas" e a entrar em território de agente).

**Onde aparece.** Em LangGraph, o estado é um objeto explícito que flui entre os nós do grafo (`StateGraph`). É o que permite *checkpoints*, *replay* e *recuperação de falhas*.

**Erro comum.** Confundir **estado** com **contexto do prompt**. O contexto é o texto que vai ao LLM a cada turno; o estado é a estrutura de dados que o orquestrador mantém e que **alimenta** o contexto. São camadas diferentes.

**Conexão.** O estado substitui, em arquitetura, o papel que o **contexto de conversa** tinha no chatbot — mas muito mais rico: não guarda só mensagens, guarda **o progresso da tarefa**.

---

### 1.4 — Objetivos (e critério de parada)

**Definição.** Objetivo é a condição que define que a tarefa foi concluída. O **critério de parada** é a regra que permite ao agente decidir: *"terminei, posso parar"*.

**Intuitivamente.** Um chatbot para quando termina a frase. Um agente para quando **atinge o objetivo** — o que pode exigir zero ou vinte ações.

**Tecnicamente.** O agente precisa de:

1. uma **representação do objetivo** (ex.: "relatório enviado ao gestor");
2. um **critério de parada verificável** (ex.: "ferramenta `enviar_email` retornou sucesso");
3. um **limite de segurança** (ex.: `max_iterations = 10`) para não rodar eternamente.

```text
Objetivo concluído?  ── Sim ──→  Resposta final
        │
        Não
        ↓
   Nova ação (próxima iteração do loop)
```

**Por que "LLM + ferramentas" não tem isso.** Sem um loop e sem um critério de parada, não existe noção de "ainda falta algo". O sistema só produz uma saída e encerra. Não há a pergunta *"o resultado faz sentido? Preciso refazer?"* — que é justamente o que define um agente (voltam à aula: itens 4 e 5 da lista de decisões do agente).

**Erro comum.** Esquecer o **limite de segurança**. Um agente sem `max_iterations` pode entrar em loop infinito gastando tokens indefinidamente — um dos erros mais caros e comuns.

**Conexão.** O critério de parada fecha o ciclo que começou no planejamento: o **objetivo** diz *para onde* ir; o **plano** diz *como*; o **loop** *executa*; o **critério de parada** diz *quando parar*.

---

### Síntese da Questão 1

| Aspecto             | LLM + ferramentas (Tool Calling)              | Agente                                             |
|---------------------|-----------------------------------------------|----------------------------------------------------|
| Recebe              | um pedido                                     | um **objetivo**                                    |
| Executa             | uma ação (ou sequência fixa)                  | **loop de decisões**                               |
| Decide próxima jogada | o orquestrador, de forma fixa               | o **próprio sistema**, com base nos resultados     |
| Memória             | pouca ou nenhuma entre chamadas              | **estado** explícito de tarefa                     |
| Para quando         | termina a resposta                            | **atinge o objetivo** (ou atinge o limite seguro)  |
| Analogia            | uma calculadora que você opera               | um funcionário a quem você **delega** a tarefa     |

Conclusão amarrada ao **Princípio Prometheus XLVI**:

> Um agente não é um LLM mais inteligente. É uma **arquitetura** que combina raciocínio, memória, planejamento, ferramentas e controle de execução para perseguir objetivos complexos.

"Ferramentas" é apenas **um dos cinco ingredientes**. Tirar planejamento, estado, loop de decisão e critério de parada — e o que sobra é só Tool Calling.

---

## Questão 2

> Projete um agente para administrar uma pequena empresa (consultar estoque, emitir pedidos de compra, responder clientes, atualizar planilhas, gerar relatórios financeiros, enviar alertas ao gestor).

### Visão geral da arquitetura

```text
                 ┌─────────────────────────────────────────────┐
   Objetivo ───▶ │              ORQUESTRADOR                    │
  (do gestor)    │  • mantém o ESTADO da tarefa                 │
                 │  • chama o LLM para planejar/decidir         │
                 │  • aplica limites, guardrails, validação     │
                 │  • decide: continuar? parar? escalar?        │
                 └───────────────┬─────────────────────────────┘
                                 │  (loop ReAct)
            ┌────────────────────┼────────────────────┬───────────┐
            ▼                    ▼                    ▼           ▼
    ┌───────────────┐   ┌────────────────┐   ┌──────────────┐  ┌────────────┐
    │ consultar_    │   │ emitir_pedido_ │   │ responder_   │  │ atualizar_ │
    │   estoque     │   │    compra ⚠    │   │   cliente    │  │  planilha  │
    │ (leitura)     │   │ (escrita/risco)│   │ (comunicação)│  │ (escrita)  │
    └───────────────┘   └────────────────┘   └──────────────┘  └────────────┘
            │                    │                    │                │
            └────────────────────┴─────────┬──────────┴────────────────┘
                                          ▼
                              ┌───────────────────────┐
                              │ gerar_relatorio_      │
                              │       financeiro      │
                              └───────────┬───────────┘
                                          ▼
                              ┌───────────────────────┐
                              │ enviar_alerta (gestor)│
                              └───────────┬───────────┘
                                          ▼
                                  Objetivo concluído
```

As ferramentas marcadas com ⚠ têm **efeitos colaterais reais** (gastam dinheiro ou enviam algo em nome da empresa) — serão tratadas com cuidado especial no item 2.4.

---

### 1) Quais decisões pertencem ao agente?

Decisão = momento em que o sistema precisa **julgar**, não apenas executar. Aqui elas são:

- **Priorizar** solicitações concorrentes (um cliente reclamando vs. um relatório mensal programado).
- **Escolher qual ferramenta** chamar ([[Aula 7 - Ferrament Calling, Quando o Modelo Deixa de Ser Apenas Responder e Passa a Utilizar Ferramentas|Ferrament Calling]]) a cada passo do loop (e em que ordem).
- **Detectar informação faltante:** "preciso emitir pedido de compra, mas não sei o estoque mínimo — devo consultar configuração antes".
- **Avaliar se o resultado faz sentido** (validação semântica): o estoque voltou negativo? Há inconsistência entre vendas e estoque?
- **Decidir se continua, refaz ou para** (critério de parada do loop).
- **Escalar para o gestor** quando o risco ou a incerteza ultrapassam o que o agente está autorizado a decidir sozinho.

> Resumindo: **tudo que envolve juízo sob incerteza** é decisão do agente; tudo que é **operação mecânica confiável** é tarefa de ferramenta.

---

### 2) Quais ações pertencem às ferramentas?

Cada ferramenta é **determinística, testável e de responsabilidade bem definida**. O agente orquestra; a ferramenta executa uma coisa bem feita.

| Ferramenta                 | Entrada                          | Saída                        | Efeito colateral? |
|----------------------------|----------------------------------|------------------------------|-------------------|
| `consultar_estoque`        | SKU / produto                    | `{qtd, mínimo, ...}`         | Não (leitura)     |
| `emitir_pedido_compra` ⚠   | SKU, quantidade, fornecedor      | `id_pedido`, status          | **Sim** (gasto)   |
| `responder_cliente`        | mensagem do cliente, contexto    | texto da resposta            | Sim (comunicação) |
| `atualizar_planilha`       | dados estruturados, aba          | confirmação                  | Sim (escrita)     |
| `gerar_relatorio_financeiro` | período, contas                | relatório (texto/tabela)     | Não (cálculo)     |
| `enviar_alerta`            | severidade, mensagem, destinatário | confirmação de envio       | Sim (notifica)    |

**Princípio de ouro:** a ferramenta nunca "decide" — ela executa o que o agente pediu e devolve um resultado observável. Se a resposta precisa de interpretação, isso é trabalho do LLM dentro do agente.

---

### 3) Qual é o papel do LLM dentro desse sistema?

O LLM é **um componente**, não o sistema inteiro. Ele aparece no orquestrador como a parte que "pensa":

- **Interpretar** pedidos em linguagem natural do gestor e dos clientes.
- **Planejar** a sequência de passos (decompor o objetivo).
- **Escolher** a próxima ferramenta e montar seus argumentos.
- **Interpretar e validar** os resultados retornados pelas ferramentas.
- **Redigir** comunicações (respostas a clientes, alertas, relatórios).
- **Justificar** decisões em linguagem humana (para auditoria e confiança).

O que o LLM **não** faz sozinho aqui: manter o estado entre passos, garantir limites de iteração, validar tipos de saída, controlar acesso às ferramentas. Tudo isso é **orquestrador**. A frase da aula vale ouro:

> *O LLM continua sendo apenas um componente do sistema.*

---

### 4) Como o orquestrador controla o fluxo (erros, loops infinitos, decisões inadequadas)?

Esta é a parte que separa um agente **de produção** de uma demonstração. Os mecanismos:

**a) Limites duros (contra loops infinitos e custo descontrolado)**
- `max_iterations` por tarefa — ex.: no máximo 10 voltas do loop ReAct.
- `timeout` total e por ferramenta — ex.: 60 s por chamada.
- Orçamento de tokens — aborta se ultrapassar o teto.

**b) Tratamento de erros**
- **Retry com backoff** para falhas transitórias (API fora do ar), com limite de tentativas.
- **Propagação controlada** de erros persistentes: registra, avisa o gestor e *não* inventa um resultado.
- O LLM só pode agir sobre o que as ferramentas de fato devolveram — nunca "alucinar" um estoque.

**c) Validação de saída (contra decisões inadequadas)**
- Cada ferramenta tem um **contrato** (schema de entrada/saída); o orquestrador valida antes de prosseguir.
- **Guardrails de negócio**: ex., `emitir_pedido_compra` recusa quantidade negativa ou acima de um teto; `responder_cliente` passa por filtro de conteúdo e por lista de "assuntos proibidos" (preços, promessas).

**d) Human-in-the-loop para risco alto (o mais importante deste exercício)**
- Ações com efeito colateral sério — **compra** e, dependendo do caso, **resposta a cliente** — são colocadas em modo de **aprovação**: o agente prepara, mostra ao gestor, e só executa após OK.
- Isso transforma o agente em um **copiloto** nas áreas sensíveis e autônomo nas áreas rotineiras.

**e) Auditoria e rastreabilidade**
- Todo passo é logado: qual ferramenta, com quais argumentos, qual resultado, qual "pensamento" do LLM motivou a chamada.
- Permite *replay* e diagnóstico de qualquer decisão — essencial em ambiente empresarial.

**f) Checkpoint de estado**
- O estado é persistido a cada passo, de modo que uma falha no meio da tarefa permite **retomar de onde parou**, e não recomeçar do zero (nem repetir compras já feitas).

**g) Critério de parada bem definido (visto na Questão 1)**
- "Objetivo concluído" só é aceito quando uma condição **verificável** foi satisfeita — não quando o LLM "decide" que acabou.

---

## Conexão com a indústria (quando usar e quando NÃO usar)

**Onde se usa agentes na prática:**
- Suporte ao cliente com escalonamento (responde, e quando não sabe, chama humano).
- Assistentes financeiros pessoais (orçamento, alertas de gasto).
- SDRs de vendas (qualificam leads, agendam, atualizam CRM).
- Operações/DevOps (auto-remediação: detecta incidente, diagnostica, aplica runbook).

**Quando vale a pena um agente:**
- O objetivo é **complexo e multi-etapa**, com dependências entre passos.
- O número de caminhos é grande demais para um pipeline fixo.
- É aceitável (e desejável) que o sistema **decida** sob supervisão.

**Quando NÃO vale (use algo mais simples):**
- Tarefa de **um passo só** → basta Tool Calling direto, sem loop.
- Resposta depende só de **conhecimento** → **RAG** resolve, sem agente.
- Fluxo **totalmente previsível** → um pipeline/programa comum é mais barato, rápido e auditável que um agente.
- Alto custo de erro com baixa capacidade de supervisão → prefira automação determinística.

> Regra prática: **só vire agente o que precisa decidir. O resto vira pipeline.**

---

## Amarrando com tudo o que já estudamos (o quebra-cabeça completo)

```text
Pré-treinamento   →  ensina linguagem
SFT / RLHF        →  ensinam comportamento
Prompt Engineering →  direciona a inferência
Arquitetura de    →  organiza a comunicação
  prompts
RAG               →  fornece conhecimento atualizado
Tool Calling      →  permite agir sobre sistemas externos
Agentes           →  coordenam tudo isso para perseguir objetivos
```

Nenhum desses conceitos substitui o anterior — **eles se acumulam em camadas**. O agente da Questão 2 só funciona porque, por baixo dele, há um LLM bem treinado, bem instruído (prompt), com acesso a conhecimento (RAG), capaz de chamar ferramentas (Tool Calling) — e agora **coordenado por um orquestrador com estado e loop de decisão**. É o topo da pirâmide que construímos módulo a módulo.

---

## Mini-exercício (para você resolver)

Antes de fechar a aula, teste seu raciocínio — sem consultar a IA:

> Reescreva o fluxo da Questão 2 para um cenário diferente: **um agente de triagem médica** que lê sintomas, consulta uma base de protocolos, recomenda conduta e, em casos de risco, encaminha a um profissional.
>
> Identifique: (1) quais decisões são do agente; (2) quais ferramentas ele teria; (3) onde o *human-in-the-loop* é **obrigatório**; (4) qual seria o critério de parada.

Se conseguir estruturar isso com a mesma lógica que usamos acima, o conceito de **agente** está realmente sólido — e você não está mais dependendo da IA para desenhá-lo.

---

*Princípio Prometheus XLVI — um agente é arquitetura, não um LLM mais esperto. E arquitetura se aprende projetando.*

---


# Complemento — O Papel do Humano

> Anexo à resolução do **Desafio Prometheus #008** (Questão 2).
> Mentor: `prometheus-mentor`

Este documento fecha uma lacuna proposital da resolução principal: descrevemos o que o **LLM** e o **orquestrador** fazem, mas deixamos o humano implícito. Aqui formalizamos onde, como e por que o ser humano é parte **estrutural** da arquitetura de um agente — e não um enfeite que se remove quando o sistema "fica bom".

---

## A tese

> Um agente autônomo de verdade é raro e, quase sempre, um erro disfarçado de eficiência. O humano não compete com o agente: ele **emoldura** o espaço dentro do qual o agente pode agir. Tirar o humano é remover a borda do quadro.

---

## A chave conceitual: camadas diferentes de decisão

O LLM raciocina **sobre a tarefa** ("qual a próxima ferramenta?").
O humano raciocina **sobre o sistema** ("este agente deveria existir? com quais limites?").

São níveis distintos. O mapa completo de um sistema com agentes:

```text
CAMADA 0  —  ANTES de tudo         →  HUMANO desenha o sistema
                                       (quais ferramentas, guardrails, orçamento, risco tolerado)

CAMADA 1  —  a cada tarefa          →  HUMANO define objetivo + restrições
                                       ("faça o relatório mensal, mas não compre nada acima de R$ 5 mil")

CAMADA 2  —  durante a execução     →  AGENTE raciocina dentro dos limites
                                       (o LLM, o loop ReAct, o estado)

CAMADA 3  —  portões de risco       →  HUMANO aprova ações sensíveis
                                       (compra, e-mail ao cliente, decisão financeira)

CAMADA 4  —  depois / sempre        →  HUMANO monitora, audita, corrige o rumo
```

**Observação central:** o LLM aparece **só na Camada 2**. Todas as outras pertencem ao humano. É por isso que dizer "o LLM faz tudo" é uma leitura incompleta: ele faz tudo **dentro de uma caixa que o humano construiu e supervisiona**.

---

## Os seis papéis concretos do humano

Do mais óbvio ao mais sutil:

1. **Definir o objetivo e as restrições.** O agente não inventa metas. Ele *recebe* um objetivo e um conjunto de limites (orçamento, prazo, escopo). Sem isso, não há para onde correr.

2. **Projetar as fronteiras (guardrails).** É o humano quem decide: o agente pode enviar e-mail sozinho? Pode emitir pedido de compra até quanto? Pode responder sobre preços? Isso é **política**, não tecnologia — e é prerrogativa humana.

3. **Ser o alvo de escalonamento.** Quando a incerteza ou o risco excedem o que o agente está autorizado a decidir, ele **para e chama você**. O humano é o *fallback* de decisão.

4. **Aprovar ações irreversíveis.** Comprar, pagar, enviar comunicação em nome da empresa — o agente **prepara**, o humano **libera**. Aqui o agente é *copiloto*, não piloto.

5. **Monitorar e auditar.** Ler os logs, perceber que o agente está viciado em um padrão, detectar *drift*, avaliar qualidade. Nenhum agente se auto-audia de forma confiável.

6. **Prestar contas.** O ponto mais importante e o mais negligenciado: **a responsabilidade é sempre humana**. Se o agente erra e custa dinheiro ou ofende um cliente, quem responde é a empresa, não o LLM. O agente não tem responsabilidade — tem **permissão**.

---

## O espectro de autonomia (a indústria formaliza isso)

A indústria organiza o nível de intervenção humana em três modelos:

| Nível | Modelo | Onde o humano está | Quando usar |
|-------|--------|--------------------|-------------|
| **Human-in-the-loop** (HITL) | agente age, humano aprova cada passo sensível | **dentro** do fluxo, em portões | ações irreversíveis, alto risco, baixa confiança |
| **Human-on-the-loop** (HOTL) | agente age sozinho num escopo delimitado, humano supervisiona | **monitorando**, intervindo só em exceção | operações reversíveis e rotineiras |
| **Human-out-of-the-loop** | autonomia total | ausente | **raro e perigoso** — quase nunca é o objetivo certo |

### Aplicado à pequena empresa da Questão 2

O desenho saudável é **híbrido**, não um nível único:

- **HITL** (aprovação obrigatória): `emitir_pedido_compra` ⚠ e `responder_cliente` em assuntos sensíveis (preço, reclamação, contrato).
- **HOTL** (autônomo + supervisionado): `gerar_relatorio_financeiro`, `atualizar_planilha`, `consultar_estoque`, `enviar_alerta` de baixa severidade.
- **Out-of-the-loop**: **nenhuma** função nesta empresa deveria rodar totalmente sem supervisão humana possível.

Mapeado sobre o fluxo original da Questão 2:

```text
Objetivo (gestor)
        ↓
   Orquestrador  ◀── (Camada 2: agente raciocina)
        │
        ├── consultar_estoque ────────►  [HOTL] roda sozinho
        ├── responder_cliente ────────►  [HITL] passa por portão de aprovação
        ├── emitir_pedido_compra ⚠ ───►  [HITL] portão obrigatório antes de gastar
        ├── atualizar_planilha ───────►  [HOTL] roda sozinho, auditável depois
        ├── gerar_relatorio_financeiro ► [HOTL] roda sozinho
        └── enviar_alerta (alto risco) ► [HITL] se severo, confirma destinatário
        ↓
   Log de auditoria  ◀── (Camada 4: humano revisa)
```

---

## Os dois erros clássicos (e como evitá-los)

- **Excesso de automação:** remover o humano dos portões achando que o agente "já está bom". Resultado: o primeiro caso fora do padrão vira desastre, e ninguém estava olhando.
- **Excesso de desconfiança:** transformar o agente numa máquina de pedir aprovação para *tudo*. Resultado: o humano vira um carimbo que aprova sem ler, e o agente perde toda a utilidade.

**Regra prática de equilíbrio:** coloque o humano **onde a decisão é irreversível ou onde falta informação ao agente** — e solte-o onde a operação é reversível e rotineira.

---

## Conexão com o ecossistema que já estudamos

- **RAG / Prompt Engineering:** definem *o que* o agente sabe e *como* ele interpreta — mas não definem **para que** ele existe. Isso é decisão humana (Camada 0).
- **Tool Calling:** define *as mãos* do agente. Os **limites** dessas mãos (permissões, teto de compra, filtros de conteúdo) são **guardrails** definidos pelo humano.
- **Orquestrador (Aula 8):** é onde o *human-in-the-loop* se materializa tecnicamente — como um "nó de aprovação" no fluxo, antes de ações com efeito colateral.

Em uma frase: **todo o aparato técnico do Módulo 4 opera dentro de uma moldura de governança que é, e continua sendo, humana.**

---

## Conexão com a missão do próprio mentor

O `system prompt` do `prometheus-mentor` diz: *"reduzir a dependência da IA ao longo do tempo"*.

Reduzir dependência **não** é remover o humano do sistema — é o oposto: é deixar o humano mais **afiado** justamente nas camadas que importam (0, 1, 3, 4, 6), e delegar ao agente o trabalho repetitivo da camada 2. O bom uso de agentes **eleva** o papel humano, não o elimina. Você deixa de digitar relatórios para se dedicar a *desenhar, supervisionar e prestar contas* — trabalho que máquinas não assinam.

---

## Mini-exercício (para você resolver)

Retome o agente de **triagem médica** proposto na resolução principal. Agora a pergunta é de **governança**, não de arquitetura:

> Para esse agente de triagem, classifique cada uma destes pontos no espectro HITL / HOTL / out-of-the-loop e justifique:
> 1. Ler sintomas e sugerir uma conduta de baixo risco (repouso, hidratação).
> 2. Recomendar medicação com possível interação medicamentosa.
> 3. Encaminhar a um profissional.
> 4. Identificar um sinal de emergência (ex.: dor torácica).
>
> Depois, responda: **qual é o portão de aprovação que jamais pode ser removido**, mesmo que o agente atinja 99% de acerto?

Se você conseguir estruturar isso sozinho, o conceito de **autonomia controlada** está sólido — e você está pensando como quem *projeta* sistemas de IA, não como quem apenas *usa*.

---

*O agente executa. O humano responde. Trocar essas posições é o erro mais caro da adoção de IA em empresas.*
