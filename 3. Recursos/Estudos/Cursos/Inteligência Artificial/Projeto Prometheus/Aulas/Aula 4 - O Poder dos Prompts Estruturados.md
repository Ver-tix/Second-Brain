---
tags:
  - IA
  - programação
  - inovação
---
É de extrema importância que você relembre dos frameworks aprendidos em cursos como:
- [[Claude 101]]
- [[AI Fluency -  Framework & Foundations]]
- [[Engenharia de Prompt - ChatGPT, Gemini, Meta AI, Grok e Mais]]
- [[Engenharia de Prompt - ChatGPT, Gemini, Meta AI, Grok e Mais, pt 2]]

> **Objetivo da aula**
> 
> Transformar o Prometheus-Mentor de um simples "repassador de perguntas" em um agente que possui uma identidade, uma função e um comportamento consistente.

---

# Até agora...

Nosso fluxo é:

```text
Usuário
    │
    ▼
MentorAgent
    │
    ▼
LLMService
    │
    ▼
OpenAI
```

Mas existe um problema enorme.

O modelo recebe apenas isto:

```
Explique o que é um Transformer.
```

Nada mais.

Ele não sabe:

- quem ele é;
    
- qual seu papel;
    
- quem é o usuário;
    
- qual o objetivo do sistema.
    

Ele apenas responde.

---

# O problema

Imagine contratar um professor.

Você apenas diz:

> "Ensine."

Provavelmente ele perguntaria:

- Qual matéria?
    
- Para quem?
    
- Em que nível?
    
- Com qual metodologia?
    

Um LLM é igual:
<h4 align="center">Um LLM Sem Contexto Acaba Improvisando</h4>

---

# O Prompt é a personalidade do agente

Até hoje usamos "prompt" como se fosse apenas uma pergunta.

Na verdade...

Um prompt profissional é muito maior.

Ele costuma possuir:

```text
Quem você é

+

Objetivo

+

Restrições

+

Contexto

+

Tarefa
```

---

# Exemplo

Hoje fazemos algo parecido com:

```
Explique o que é RAG.
```

Depois da aula, faremos algo parecido com:

```
Você é o Prometheus-Mentor.

Seu objetivo é ensinar Inteligência Artificial
de forma didática.

Explique utilizando exemplos.

Evite responder superficialmente.

Quando possível,
conecte com conceitos vistos anteriormente.

Pergunta do aluno:

"O que é RAG?"
```

Percebe?

A pergunta virou apenas uma pequena parte do prompt.

---

# A mudança arquitetural

Hoje:

```text
MentorAgent

↓

LLMService

↓

OpenAI
```

Depois da aula:

```text
MentorAgent

↓

PromptBuilder

↓

LLMService

↓

OpenAI
```

Acabamos de introduzir mais um componente.

---

# Por que criar um PromptBuilder?

Porque prompts também são código.

Imagine se cada agente tivesse:

```python
"""
Você é...
...
...
...
...
...
"""
```

Espalhado pelo projeto inteiro.

Seria impossível manter.

---

Agora imagine:

```text
prompts/

mentor_prompt.py
editor_prompt.py
knowledge_prompt.py
```

Cada agente possui seu próprio prompt.

Organizado.

Versionado.

Reutilizável.

---

# Responsabilidades

Observe como a arquitetura continua limpa.

## MentorAgent

Coordena.

---

## PromptBuilder

Constrói o prompt.

---

## LLMService

Conversa com o modelo.

---

Cada objeto faz apenas uma coisa.

---

# O primeiro Prompt do Prometheus

Nosso Mentor receberá uma identidade.

Algo como:

```
Você é o Prometheus-Mentor.

Você é um professor especializado
em Inteligência Artificial,
Arquitetura de Software
e Engenharia de Sistemas Inteligentes.

Ensine utilizando:

- primeiros princípios;

- analogias;

- exemplos concretos;

- conexões com aulas anteriores;

- linguagem clara.

Nunca entregue respostas extremamente curtas.

Sempre explique o motivo das coisas.

Sempre priorize entendimento,
não memorização.
```

Reconhece esse estilo?

É exatamente como venho ensinando você desde o início do Projeto Prometheus.

Agora vamos codificá-lo.

---

# Uma observação importante

Muita gente acha que Prompt Engineering é:

> "escrever prompts melhores."

Não.

Em sistemas profissionais, Prompt Engineering significa:

> **projetar o comportamento do agente.**

É muito mais arquitetura do que criatividade.

---

# O que NÃO faremos

Não colocaremos esse texto dentro do agente.

Jamais.

O agente apenas pedirá:

```python
prompt = PromptBuilder.build(question)
```

Fim.

---

# Nova arquitetura

```text
Usuário

↓

main.py

↓

MentorAgent

↓

PromptBuilder

↓

LLMService

↓

OpenAI
```

Repare que a arquitetura cresce...

...sem bagunçar.

Esse é exatamente o efeito da separação de responsabilidades que estudamos durante todo o Módulo 5.

---

# Laboratório 4 — Dando personalidade ao Prometheus-Mentor

Hoje vamos evoluir o projeto sem quebrar sua arquitetura.

## [[🤖 Monitoria M6 004 - TEORIA]]
## [[🤖 Monitoria M6 004 - PRÁTICA]]

## [[🛠 Desafio M6 004]]

## Etapa 1 — Criar um novo arquivo

Dentro de:

```text
app/prompts/
```

crie:

```text
mentor_prompt.py
```

---

## Etapa 2 — Criar a classe `PromptBuilder`

Implemente uma classe chamada:

```python
class PromptBuilder:
```

Ela terá um método responsável por montar o prompt completo a partir da pergunta do usuário.

**Dica:** o método pode ser estático (`@staticmethod`), já que ele apenas transforma uma entrada (a pergunta) em uma saída (o prompt), sem depender de estado interno.

---

## Etapa 3 — Escrever o prompt-base

Construa um prompt que defina:

- quem é o Prometheus-Mentor;
    
- sua missão;
    
- seu estilo de ensino;
    
- como deve responder;
    
- onde a pergunta do usuário será inserida.
    

Não precisa copiar exatamente o texto da aula. Escreva uma primeira versão com as características que você considera essenciais.

---

## Etapa 4 — Alterar o `MentorAgent`

Hoje ele faz algo parecido com:

```python
return self.llm_service.generate(question)
```

Agora ele deverá:

1. receber a pergunta;
    
2. pedir ao `PromptBuilder` para construir o prompt completo;
    
3. enviar esse prompt ao `LLMService`;
    
4. devolver a resposta.
    

Perceba que o agente continua **sem saber como o prompt é construído**.

---

## Etapa 5 — Testar

Faça perguntas como:

- "O que é um Transformer?"
    
- "Explique RAG."
    
- "O que é uma API?"
    

Compare as respostas com a versão anterior.

Observe se elas ficaram mais alinhadas ao estilo do Prometheus-Mentor.

---

# Desafio de reflexão

Sem escrever código adicional, responda no final do laboratório:

> **Por que colocamos o PromptBuilder em um arquivo separado, em vez de escrever o prompt diretamente dentro do MentorAgent?**

Essa pergunta parece simples, mas a resposta conecta diretamente esta aula aos princípios arquiteturais do Módulo 5.

---

E uma última observação: talvez você não perceba agora, mas estamos montando o Prometheus exatamente da mesma forma que empresas constroem sistemas reais. Em vez de sair escrevendo centenas de linhas de código, cada aula adiciona **uma única responsabilidade nova**, mantendo tudo organizado. Quando chegarmos aos agentes com memória, ferramentas, RAG e múltiplos agentes, a base já estará sólida. É esse cuidado que faz um projeto crescer sem virar um "monstro" difícil de manter.