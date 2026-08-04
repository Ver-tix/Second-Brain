---
tags:
  - IA
  - programação
  - inovação
tipo:
  - conceito
dominio:
  - IA
Subdominio:
  - agentic-archtecture
  - RAG
---
Chegamos a um dos módulos mais importantes de todo o Projeto Prometheus.

Até aqui, construímos o **corpo do agente**.

Agora vamos construir a **memória de conhecimento**.

A diferença é enorme.

---

## Antes do Módulo 7

Nosso agente atual funciona assim:

```
Usuário
   │
   ▼
MentorAgent
   │
   ▼
PromptBuilder
   │
   ▼
LLM
   │
   ▼
Resposta
```

O conhecimento dele vem exclusivamente do modelo.

Ou seja:

- o que estava no treinamento do GPT;
- o contexto enviado na conversa;
- ferramentas disponíveis.

Mas existe um problema.

O modelo não conhece:

- seus livros;
- suas anotações;
- seu Second Brain;
- seus documentos;
- suas teses;
- suas decisões anteriores.

---

# O problema que o RAG resolve

Imagine que você pergunte:

> "Segundo meus estudos de Marketing, quais são os 5 princípios fundamentais de posicionamento de marca?"

O GPT pode responder algo genérico.

Mas você quer:

> "Segundo os meus 40 livros, minhas notas no Obsidian e minhas próprias conclusões."

Essa é outra categoria de problema.

O agente precisa consultar uma fonte externa.

---

# O que é RAG?

RAG significa:

**Retrieval-Augmented Generation**

Em português:

> Geração aumentada por recuperação.

A ideia é simples:

Antes de responder, o sistema busca informações relevantes.

Fluxo:

```
Usuário faz pergunta

        │

        ▼

Sistema procura conhecimento relevante

        │

        ▼

Encontra documentos relacionados

        │

        ▼

Adiciona esses documentos ao prompt

        │

        ▼

LLM gera resposta baseada nesse contexto
```

---

# Comparando com memória de conversa

Aqui entra uma distinção que estudamos no Módulo 5.

Existem diferentes tipos de memória.

## 1. Memória de conversa

Exemplo:

> "Você lembra que eu perguntei sobre Python ontem?"

É estado da interação atual.

Normalmente:

```
ConversationMemory
```

---

## 2. Memória compartilhada

Exemplo:

> "Usuário prefere respostas com diagramas."

É uma preferência estruturada.

---

## 3. Knowledge Memory (RAG)

Exemplo:

> "Resumo do livro Tração."

É conhecimento.

Não precisa estar sempre carregado.

Precisa ser encontrado quando necessário.

---

# O grande salto arquitetural

No Módulo 6:

Criamos:

```
Agent
   +
Tools
```

No Módulo 7:

Vamos adicionar:

```
Agent
   +
Tools
   +
Knowledge
```

A arquitetura começa a parecer um sistema cognitivo.

---

# A arquitetura que construiremos

Ao final do módulo, teremos algo próximo disso:

```
                 Usuário
                    │
                    ▼
              MentorAgent
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼

 Conversation    Tool       Knowledge
 Memory          Registry      System
                                │
                                ▼
                           Retriever
                                │
                                ▼
                         Vector Database
                                │
                                ▼
                           Documents

                    │
                    ▼
                 LLMService
```

---

# A grande pergunta arquitetural do módulo

Lembra da pergunta que deixei no final da Aula 11?

Era:

> Onde o RAG deveria entrar?

A resposta é:

O RAG **não pertence ao PromptBuilder**.

Por quê?

Porque o PromptBuilder tem uma responsabilidade:

> Montar um prompt.

Ele não deve saber:

- onde os documentos estão;
- como buscar;
- como calcular embeddings;
- qual banco consultar.

Essa responsabilidade pertence ao sistema de conhecimento.

Então:

```
MentorAgent

decide:

"preciso de conhecimento externo"

        ↓

KnowledgeService

busca contexto

        ↓

PromptBuilder

monta prompt final
```

Cada peça continua fazendo uma coisa.

---

# Estrutura do Módulo 7

Teremos aproximadamente:

---

# Aula 1 — O problema da memória externa

Objetivo:

Entender:

- por que LLMs não têm memória real;
- diferença entre contexto e conhecimento;
- por que precisamos de RAG.

---

# Aula 2 — Embeddings: transformando texto em significado

Construiremos:

- embeddings;
- vetores;
- similaridade semântica.

Entenderemos:

Por que uma máquina consegue entender que:

```
"cachorro"
```

é próximo de:

```
"animal doméstico"
```

mesmo sem igualdade textual.

---

# Aula 3 — Banco Vetorial

Vamos criar a infraestrutura:

```
Documento

↓

Embedding

↓

Vector Database
```

Veremos:

- ChromaDB;
- FAISS;
- armazenamento;
- busca por similaridade.

---

# Aula 4 — Pipeline de ingestão

Construiremos o processo:

```
PDF

↓

Extração de texto

↓

Chunks

↓

Embeddings

↓

Banco Vetorial
```

---

# Aula 5 — Retrieval

O agente aprenderá:

Pergunta:

↓

Busca:

↓

Trechos relevantes

---

# Aula 6 — Integrando RAG ao MentorAgent

Nosso fluxo ficará:

```
Usuário

↓

MentorAgent

↓

KnowledgeService

↓

Contexto

↓

PromptBuilder

↓

LLM
```

---

# Aula 7 — Segundo Cérebro Prometheus

Aqui conectamos com algo muito importante:

Seu próprio Second Brain.

Vamos discutir:

- Obsidian;
- notas;
- Git;
- versionamento;
- conhecimento pessoal.

---

# Aula 8 — Arquitetura avançada de Knowledge

Entram conceitos como:

- memória episódica;
- memória semântica;
- curadoria;
- atualização de conhecimento;
- conflitos entre fontes.

---

# Aula 9 — Laboratório final

Construiremos uma versão funcional:

**Prometheus-Knowledge**

Um agente capaz de:

- consultar documentos;
- recuperar conhecimento;
- responder baseado no seu acervo.

---

# Antes de começarmos a Aula 1

Uma observação importante.

Esse módulo é provavelmente o módulo que mais conversa com tudo que você já construiu fora do código:

- seu Obsidian;
- seu Second Brain;
- sua biblioteca de Marketing;
- seus estudos;
- suas notas.

Até agora construímos o "cérebro executivo".

Agora vamos construir a "biblioteca neural".