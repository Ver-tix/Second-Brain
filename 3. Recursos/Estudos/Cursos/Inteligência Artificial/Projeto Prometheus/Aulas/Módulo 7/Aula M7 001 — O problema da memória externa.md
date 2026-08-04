---
tipo:
  - conceito
dominio:
  - IA
Subdominio:
  - agentic-archtecture
  - RAG
tags:
  - IA
  - programação
  - inovação
---
Como nos módulos anteriores, começaremos pela motivação e, em seguida, iremos direto para o código.

Imagine o Prometheus de hoje.

Ele consegue responder:

> "O que é um Transformer?"

Mas não consegue responder:

> "No meu resumo do livro _Tração_, quais canais de aquisição eu destaquei como prioritários?"

Não porque o GPT não seja inteligente, mas porque **esse conhecimento nunca foi entregue a ele durante a inferência**.

O erro mais comum é pensar:

> "Então basta colocar meu Second Brain inteiro no prompt."

Isso não escala. Seu Second Brain já possui milhares de notas e continuará crescendo. O contexto de uma requisição é limitado e caro.

A ideia central do RAG é diferente:

> **Não enviar tudo. Enviar apenas o que é relevante para aquela pergunta.**

Esse princípio vai guiar toda a arquitetura do módulo.

---

## Laboratório 7.1 — Preparando a infraestrutura de conhecimento

Hoje ainda **não** vamos indexar documentos nem gerar embeddings. Vamos preparar a arquitetura para que isso aconteça nas próximas aulas.

## [[🤖 Monitoria M7 001]] 
## [[🛠 Desafio M7 001]] 
### Objetivo

Adicionar ao projeto uma nova camada responsável por conhecimento externo, separada do `MentorAgent`.

### Etapa 1

Crie uma nova pasta:

```
app/
└── knowledge/
```

e adicione um `__init__.py`.

---

### Etapa 2

Dentro dela, crie o arquivo:

```
knowledge_service.py
```

Por enquanto, ele conterá apenas a classe `KnowledgeService`, responsável futuramente por responder perguntas como:

- "Busque informações relevantes."
- "Recupere trechos do Second Brain."
- "Forneça contexto para o PromptBuilder."

Nesta primeira versão, ela será apenas um **esqueleto arquitetural**, sem lógica de RAG.

---

### Etapa 3

Refatore o `MentorAgent` para receber um `KnowledgeService` no construtor, assim como hoje ele já recebe:

- `LLMService`
- `ConversationMemory`
- `ToolManager`

Ainda não o utilizaremos. O objetivo é aplicar **injeção de dependência** desde o início, evitando futuras refatorações grandes.

---

### Etapa 4

Atualize o `main.py` para criar uma instância do `KnowledgeService` e passá-la ao `MentorAgent`.

---

### Etapa 5 — Reflexão

Responda:

> **Por que decidimos introduzir o `KnowledgeService` agora, mesmo sem implementar nenhuma busca de documentos?**

Essa pergunta vale mais do que o código desta aula, porque trata exatamente da evolução arquitetural que permitirá encaixar o RAG sem precisar reescrever o Prometheus depois.

Quando terminar esse laboratório, envie o código como fizemos durante todo o Módulo 6 e faremos a revisão antes de seguir para Embeddings.