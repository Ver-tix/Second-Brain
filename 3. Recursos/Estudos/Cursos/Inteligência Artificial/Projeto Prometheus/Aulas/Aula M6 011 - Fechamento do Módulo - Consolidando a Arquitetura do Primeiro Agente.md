---
tipo:
  - conceito
dominio:
  - IA
Subdominio:
  - agentic-archtecture
tags:
  - IA
  - programação
  - inovação
---
Chegamos ao fim do primeiro módulo realmente prático do Projeto Prometheus.

Até aqui, pode parecer que construímos apenas um "chat melhor organizado". Mas, arquiteturalmente, construímos algo muito maior.

Hoje vamos entender **o que realmente foi criado**.

---

# Objetivos da aula

Ao final desta aula você deverá ser capaz de responder:

- Por que o `MentorAgent` é tão pequeno?
    
- Onde cada responsabilidade da arquitetura mora?
    
- O que aconteceria se amanhã trocássemos a OpenAI por outro provedor?
    
- O que aconteceria se criássemos mais 20 agentes?
    
- O que precisaremos adicionar para transformar esse projeto em um sistema realmente inteligente?
    

---

# Parte 1 — Olhando para trás

Vamos observar a arquitetura construída.

```text
Usuário
    │
    ▼
main.py
    │
    ▼
MentorAgent
    │
    ├──────────────┐
    ▼              ▼
PromptBuilder   ToolManager
    │              │
    ▼              ▼
Conversation   ToolRegistry
Memory             │
    │              ▼
    ▼         CalculatorTool
LLMService
    │
    ▼
OpenAI
```

Observe algo curioso.

O `MentorAgent` praticamente **não faz trabalho**.

Ele apenas coordena.

Isso é proposital.

---

# Parte 2 — O Mentor virou um orquestrador

Compare mentalmente com a Aula 3.

Naquela época ele fazia tudo.

Hoje ele apenas coordena:

```python
def ask(question):

    memória

    prompt

    llm

    tools

    resposta
```

Ou seja:

Ele não sabe:

- como falar com a OpenAI;
    
- como construir prompts;
    
- como guardar memória;
    
- como executar ferramentas.
    

Ele apenas coordena essas peças.

Isso recebe um nome muito importante:

> **Orquestração.**

Foi exatamente esse conceito que estudamos no Módulo 5.

Agora ele apareceu em código.

---

# Parte 3 — O verdadeiro ganho não é organização

Muitos iniciantes pensam:

"Ah, separar arquivos deixa o projeto bonito."

Não.

Esse não é o objetivo.

O verdadeiro objetivo é:

**reduzir acoplamento.**

Vamos imaginar.

Hoje usamos:

```text
OpenAI
```

Amanhã:

```text
Anthropic
```

Quem muda?

Somente:

```text
LLMService
```

Todo o restante continua igual.

Esse é o poder da arquitetura.

---

Outro exemplo.

Hoje temos:

```text
CalculatorTool
```

Amanhã teremos:

```text
WebSearchTool
```

Quem muda?

Somente:

```text
ToolRegistry
```

O Mentor continua igual.

---

Mais um.

Hoje existe:

```text
MentorAgent
```

Amanhã teremos:

```text
EditorAgent
KnowledgeAgent
ResearchAgent
```

O que reutilizamos?

Tudo:

- PromptBuilder
    
- ToolManager
    
- Registry
    
- LLMService
    
- Memory
    

É exatamente isso que significa criar infraestrutura.

---

# Parte 4 — O que ainda falta?

Apesar de toda essa arquitetura, nosso agente ainda é extremamente limitado.

Ele ainda:

- esquece tudo quando o programa fecha;
    
- não consulta documentos;
    
- não pesquisa na internet;
    
- não usa múltiplos modelos;
    
- não conversa com outros agentes;
    
- não executa workflows;
    
- não possui planejamento.
    

Isso é importante.

Porque mostra que:

> Um agente inteligente não nasce de um único componente.

Ele nasce da composição de dezenas deles.

---

# Parte 5 — O mapa completo

Hoje nossa arquitetura pode ser resumida assim:

```text
                 Usuário
                     │
                     ▼
               MentorAgent
                     │
     ┌───────────────┼───────────────┐
     ▼               ▼               ▼
 PromptBuilder   Conversation    ToolManager
                     Memory           │
                                      ▼
                               ToolRegistry
                                      │
                                      ▼
                             CalculatorTool

                     │
                     ▼
                 LLMService
                     │
                     ▼
                 OpenAI API
```

Observe:

Cada bloco possui uma responsabilidade.

Nenhum bloco conhece detalhes internos do outro.

Esse é o verdadeiro objetivo da engenharia de software.

---

# Parte 6 — A pergunta que muda tudo

Até agora o fluxo sempre foi:

```text
Usuário pergunta

↓

LLM responde
```

Mesmo usando ferramentas.

Mesmo usando memória.

Sempre existe:

uma pergunta

→ uma resposta

Mas pense.

E se o usuário pedir:

> "Pesquise empresas do setor de IA, leia os últimos resultados trimestrais, compare com os concorrentes, gere uma tese de investimento e salve tudo no meu Second Brain."

Isso claramente **não cabe** em um único ciclo pergunta → resposta.

O que precisamos agora?

Precisamos que o sistema:

- planeje;
    
- execute vários passos;
    
- decida o próximo passo;
    
- use ferramentas diferentes;
    
- valide resultados;
    
- só então entregue a resposta.
    

Em outras palavras...

Estamos chegando ao momento em que um **chat** deixa de ser um chat e começa a se tornar um **agente**.

Esse será exatamente o foco do próximo módulo.

---

# Encerramento do Módulo 6

Se o Módulo 5 ensinou você a **pensar como arquiteto**, o Módulo 6 ensinou você a **transformar arquitetura em código**.

Você construiu, peça por peça, um agente que já possui:

- separação de responsabilidades;
    
- memória de conversa;
    
- construção modular de prompts;
    
- infraestrutura de ferramentas;
    
- Tool Calling;
    
- registro centralizado de ferramentas;
    
- serviços reutilizáveis.
    

Mais importante do que qualquer classe criada foi a mudança de mentalidade: em vez de escrever um programa que "faz tudo", você passou a construir componentes independentes que colaboram entre si.

---

# Laboratório — Aula 11 (Fechamento)

Desta vez, em vez de adicionar uma nova funcionalidade, o objetivo é consolidar a arquitetura.

## Parte 1 — Faça um diagrama da sua arquitetura

Desenhe (em ASCII ou em uma ferramenta de diagramas) a arquitetura atual do seu Prometheus-Mentor.

Inclua todos os componentes que existem hoje e mostre como eles se comunicam.

---

## Parte 2 — Identifique os pontos de extensão

Para cada componente abaixo, responda:

- O que pode mudar sem afetar o restante do sistema?
    
- Dê um exemplo concreto.
    

Componentes:

- `LLMService`
    
- `PromptBuilder`
    
- `ConversationMemory`
    
- `ToolRegistry`
    
- `ToolManager`
    
- `MentorAgent`
    

O objetivo é perceber, na prática, o valor do baixo acoplamento.

---

## Parte 3 — Planejando o próximo módulo

Imagine que agora queremos adicionar um **RAG com o seu Second Brain**.

Sem escrever código, explique:

1. Quais componentes existentes poderão ser reaproveitados?
    
2. Quais novos componentes precisarão ser criados?
    
3. Em que ponto do fluxo você faria a consulta ao RAG?
    
4. Por que essa funcionalidade pode ser adicionada sem reescrever toda a arquitetura?
    

---

Essa atividade encerra o Módulo 6 e prepara o terreno para o próximo grande salto do Projeto Prometheus: fazer o agente deixar de responder apenas com o que "sabe" e passar a responder com base no conhecimento do seu próprio ecossistema.