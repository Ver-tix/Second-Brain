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
>[! ]
>Como a Monitoria foi bem simples, o desafio está lá. Segue os feedbacks:

# Feedbacks:

## Feedbacks do 🤖 Monitor

E sabe qual é a melhor notícia? **Você errou nos lugares certos.**

Até o Módulo 5, você normalmente me perguntava "como faz?". Agora você tenta implementar primeiro e depois traz para revisão. Essa é exatamente a mudança que eu esperava ver. É assim que um engenheiro evolui.

Quanto ao código, ele está **100% correto**.

## Revisão

### Imports

```python
from app.knowledge.knowledge_service import KnowledgeService
from app.knowledge.embedding_service import EmbeddingService
```

✅ Corretos.

---

### Instanciação do `EmbeddingService`

```python
embedding_service = EmbeddingService()
```

✅ Correta.

Você criou o componente mais interno da árvore primeiro.

---

### Construção do `KnowledgeService`

```python
knowledge_service = KnowledgeService(
    embedding_service
)
```

✅ Perfeita.

O `KnowledgeService` recebe sua dependência por injeção.

---

### Construção do `MentorAgent`

```python
mentor = MentorAgent(
    llm_service,
    memory,
    tool_manager,
    knowledge_service,
)
```

✅ Correta.

Observe como o `MentorAgent` continua "cego" para toda a infraestrutura de conhecimento.

Ele conhece apenas:

```
KnowledgeService
```

Nada mais.

Isso será extremamente importante daqui a poucas aulas.

---

# O que acabou de acontecer (arquiteturalmente)

Você acabou de construir a primeira **árvore de dependências de segundo nível** do Prometheus.

Até agora tínhamos algo assim:

```text
main.py
│
├── LLMService
├── ConversationMemory
├── ToolManager
└── KnowledgeService
```

Agora temos:

```text
main.py
│
├── LLMService
├── ConversationMemory
├── ToolManager
│
├── EmbeddingService
│
├── KnowledgeService
│      │
│      └── EmbeddingService
│
└── MentorAgent
       │
       └── KnowledgeService
```

Nas próximas aulas essa árvore crescerá naturalmente:

```text
main.py
│
├── EmbeddingService
├── VectorStore
├── Retriever
│
├── KnowledgeService
│      ├── EmbeddingService
│      ├── VectorStore
│      └── Retriever
│
└── MentorAgent
```

E repare no mais interessante: **o `MentorAgent` provavelmente não mudará uma única linha**.

Esse é o efeito de uma boa arquitetura.

---

## Sobre sua evolução

Vou destacar uma coisa que achei muito positiva.

Você escreveu:

> "tentei dar os passos sozinho nesse capítulo, e QUASE consegui."

Esse "quase" é um excelente sinal.

No começo do Projeto Prometheus, era natural depender bastante de orientação. Agora você já consegue:

- ler o objetivo arquitetural;
    
- prever como a dependência deve ser injetada;
    
- modificar a estrutura do projeto;
    
- validar mentalmente se a arquitetura continua consistente.
    

Isso significa que você está deixando de apenas implementar código e começando a **raciocinar como arquiteto**. Esse era justamente o objetivo do Módulo 6 e ele está aparecendo no início do Módulo 7.

## Feedback do 🧑‍🏫 Professor Principal 

Professor corrigindo. E, antes de tudo: **mais uma excelente entrega**. O interessante é que o código desta aula é mínimo, mas a mudança arquitetural é significativa.

---

# Avaliação geral

**Nota: 10/10**

Você executou exatamente o que foi proposto, sem antecipar abstrações das próximas aulas.

---

# Etapa 1 — `EmbeddingService`

```python
class EmbeddingService:
    pass
```

Perfeito.

Muita gente teria criado:

- `generate_embedding()`
    
- `embed()`
    
- `create_vector()`
    

Nenhum deles pertence a esta aula.

O objetivo era apenas reservar um lugar na arquitetura para esse componente.

---

# Etapa 2 — `KnowledgeService`

Você evoluiu de:

```python
class KnowledgeService:
    pass
```

para:

```python
class KnowledgeService:

    def __init__(
        self,
        embedding_service: EmbeddingService
    ):
        self.embedding_service = embedding_service
```

Exatamente o esperado.

Observe que agora o `KnowledgeService` já começa a parecer um verdadeiro serviço.

Hoje ele coordena:

```text
KnowledgeService
      │
      ▼
EmbeddingService
```

Amanhã coordenará vários componentes.

---

# Etapa 3 — `main.py`

Aqui aconteceu a principal mudança da aula.

Antes:

```text
main.py
│
├── LLMService
├── Memory
├── ToolManager
├── KnowledgeService
└── MentorAgent
```

Agora:

```text
main.py
│
├── EmbeddingService
│
├── KnowledgeService
│      │
│      └── EmbeddingService
│
└── MentorAgent
       │
       └── KnowledgeService
```

Essa diferença parece pequena.

Mas arquiteturalmente ela muda tudo.

Você começou a construir um **grafo de dependências**, e não apenas uma lista de objetos.

---

# Reflexão da aula

A pergunta era:

> Por que o `EmbeddingService` não foi colocado diretamente dentro do `MentorAgent`?

A resposta que eu esperava gira em torno de:

- separação de responsabilidades;
    
- baixo acoplamento;
    
- crescimento modular.
    

O `MentorAgent` não precisa saber **como** conhecimento é obtido.

Ele só precisa saber:

> "Existe um serviço responsável por conhecimento."

Esse é exatamente o tipo de abstração que permitirá trocar futuramente:

```text
OpenAI Embeddings
```

por

```text
Voyage AI
```

ou

```text
Nomic
```

ou

```text
BGE
```

sem alterar uma única linha do `MentorAgent`.

---

# Um detalhe interessante

Quero que você observe uma evolução que aconteceu quase silenciosamente.

No início do Módulo 6, todas as dependências eram "folhas" da árvore.

Hoje algumas delas já possuem vida própria.

Veja:

```text
MentorAgent
│
├── LLMService
├── ConversationMemory
├── ToolManager
└── KnowledgeService
         │
         └── EmbeddingService
```

Mais adiante teremos algo parecido com:

```text
KnowledgeService
│
├── EmbeddingService
├── Retriever
├── VectorStore
├── DocumentLoader
└── TextChunker
```

Perceba que o `MentorAgent` continuará enxergando apenas um único objeto.

Essa é a essência do encapsulamento arquitetural.

---

# Conclusão

A Aula **M7.2** está concluída com sucesso.

E, como combinado no Projeto Prometheus, segue o **CHANGELOG**:

```markdown
## M7-002 — Fundação da Infraestrutura de Embeddings

### Adicionado
- Novo `EmbeddingService` em `app/knowledge/embedding_service.py`.

### Alterado
- `KnowledgeService` passou a receber `EmbeddingService` por injeção de dependência.
- `main.py` atualizado para construir a primeira árvore de dependências da camada de conhecimento.

### Arquitetura
- A infraestrutura de RAG começou a ser estruturada em camadas.
- `MentorAgent` permanece desacoplado da geração de embeddings.
- `KnowledgeService` evoluiu de um placeholder para o primeiro orquestrador da camada de conhecimento.
```

---

## Uma prévia da próxima aula

Até agora nós apenas desenhamos a arquitetura.

Na **M7.3** acontecerá a primeira integração "real" com IA.

Você finalmente verá um texto como:

```text
"Bitcoin é um ativo escasso."
```

ser transformado em algo parecido com:

```python
[-0.0182, 0.4431, -0.9917, 0.1265, ...]
```

Será a primeira vez que o Prometheus deixará de trabalhar apenas com texto e passará a operar sobre **representações matemáticas de significado**. Esse é um dos momentos mais marcantes de todo o curso.