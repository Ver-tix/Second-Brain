---
tags:
  - IA
---
Até agora, nossa arquitetura era esta:

```text
Usuário

↓

LLM

↓

Resposta
```

Esse é o modelo mental que a maioria das pessoas possui.

Mas ele está incompleto.

---

## A arquitetura real

Na prática, um sistema moderno se parece muito mais com isto:

```text
   Usuário
      │
      ▼
  Aplicação
      │
      ▼
 Orquestrador
      │
 ┌────┼────┐
 ▼    ▼    ▼
LLM  Banco APIs
 │           │
 ▼           ▼
Ferramentas Dados
     │       │
     ▼       ▼ 
      Resposta
```

Perceba uma mudança enorme.

O LLM deixou de ser "o sistema".

Ele virou um componente.

---

# O erro mais comum

Quando alguém começa a estudar IA, pensa:

> "Vou fazer um chatbot."

Empresas não pensam assim.

Elas pensam:

> "Vou construir um sistema que usa um LLM."

A diferença é gigantesca.

---

# Uma analogia

Imagine um arquiteto civil.

O cimento é importante?

Muito.

Mas ninguém diz:

> "Vou construir um cimento."

Ele diz:

> "Vou construir uma casa."

O LLM é o cimento.

O sistema é a casa.

---

# O fluxo de uma aplicação real

Imagine que um usuário pergunte:

> "Qual foi o faturamento da empresa no último trimestre?"

O LLM sabe?

Não.

Então o sistema faz algo parecido com isto:

```text
Pergunta

↓

Interpretar intenção

↓

Consultar banco de dados

↓

Receber números

↓

Enviar dados ao LLM

↓

Gerar resposta
```

Veja.

Quem encontrou a informação não foi o LLM.

Foi o sistema.

---

# O papel do desenvolvedor

Sua função deixa de ser:

> Escrever prompts.

E passa a ser:

> Orquestrar componentes.

Por isso, a palavra "arquitetura" apareceu tantas vezes nos módulos anteriores.

---

# O que vamos construir

Ao longo do módulo, construiremos projetos progressivos.

## Projeto 1

Um cliente de LLM em Python.

Você enviará mensagens para um modelo via API.

---

## Projeto 2

Um sistema com memória de conversa.

---

## Projeto 3

Um sistema que consulta documentos.

---

## Projeto 4

Um pequeno RAG.

---

## Projeto 5

Um agente simples.

---

## Projeto Final

Uma aplicação completa.

---

# As tecnologias

Usaremos principalmente:
- Python;
- APIs;
- JSON;
- variáveis de ambiente;
- bibliotecas oficiais;
- Git (introdução);
- organização de projetos.

Mais adiante, entraremos em:
- embeddings;
- bancos vetoriais;
- RAG;
- ferramentas;
- agentes.

---

# Uma decisão pedagógica

Poderíamos usar frameworks como:
- LangChain
- LlamaIndex

Logo no início. Mas não faremos isso.

Primeiro quero que você entenda o que acontece "por baixo do capô".

É o mesmo motivo pelo qual estudamos o Transformer antes de usar APIs.

Frameworks escondem complexidade.

E só vale esconder aquilo que você já compreende.

---

# Nosso primeiro projeto

Será extremamente simples.

Mas extremamente importante.

Vamos construir um programa que conversa com um LLM através de uma API.

Pode parecer pouco.

Mas esse pequeno programa será a base de tudo o que construiremos depois.

---

# 📜 Princípio LXVI

> **Um LLM raramente é o produto final; quase sempre é um componente dentro de um sistema maior.**

---

# 🛠️ Primeiro Projeto Prático — "Hello, LLM!"

## Objetivo

Construir sua primeira aplicação em Python que envia uma mensagem para um modelo e recebe uma resposta.

Mas antes de escrever uma única linha de código, quero fazer algo que considero muito importante.

Vamos projetar a arquitetura primeiro.

Responda estas perguntas:

1. Quais componentes você acredita que essa aplicação precisará? (Ex.: interface do usuário, módulo de comunicação com a API, gerenciamento da chave de acesso, tratamento de erros...)
    
2. Qual deve ser a responsabilidade de cada componente?
    
3. Se amanhã você decidisse trocar o provedor do modelo (por exemplo, mudar da OpenAI para outro serviço), como organizaria o código para minimizar alterações?
    
[[🛠️ Desafio M4 001]]

---

### Um aviso importante

Conhecendo sua forma de aprender, acho que você vai gostar desta fase.

Nos módulos anteriores, você buscava constantemente o **arché**, o princípio organizador.

Agora veremos que software também possui seus "archés": desacoplamento, responsabilidade única, abstração, interfaces, modularização e composição.

Perceba como tudo começa a convergir.

Você não está aprendendo "mais uma tecnologia".

Está aprendendo uma forma de construir sistemas complexos a partir de princípios simples.

E tenho a impressão de que é exatamente esse tipo de desafio que mais desperta sua curiosidade.