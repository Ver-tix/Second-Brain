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
# Etapa 1 — Criar o `EmbeddingService`

Crie:

```
app/
└── knowledge/
    └── embedding_service.py
```

---
# Etapa 2 - Conteúdo de `embedding_service.py` 

Conteúdo:

```python
class EmbeddingService:
    pass
```

Nada mais.

---

# Etapa 3 — Atualizar o `KnowledgeService`

Hoje ele deve estar parecido com:

```python
class KnowledgeService:
    pass
```

Agora ele passará a armazenar sua primeira dependência.

```python
from app.knowledge.embedding_service import EmbeddingService

class KnowledgeService:
    def __init__(
        self,
        embedding_service: EmbeddingService
    ):
        self.embedding_service = embedding_service
```

Observe a mudança arquitetural:

Antes:

```
KnowledgeService
```

Agora:

```
KnowledgeService
        │
        ▼
EmbeddingService
```

O `KnowledgeService` deixa de ser um componente isolado e passa a ser um **orquestrador da camada de conhecimento**, exatamente como o `MentorAgent` é um orquestrador da aplicação.

---

# Etapa 4 - Alterar a Construção das Dependências no `main.py`

Perfeito. Agora só precisamos alterar a construção das dependências no `main.py`.

Hoje você deve ter algo semelhante a:

```python
llm_service = LLMService()

memory = ConversationMemory()

tool_manager = ToolManager()

knowledge_service = KnowledgeService()

mentor = MentorAgent(
    llm_service,
    memory,
    tool_manager,
    knowledge_service
)
```

Agora ficará assim:

```python
from app.knowledge.embedding_service import EmbeddingService
from app.knowledge.knowledge_service import KnowledgeService

# Cria o serviço responsável por conversar com a OpenAI
llm_service = LLMService()

# Cria a memória da conversa
memory = ConversationMemory()

# Cria o gerenciador de ferramentas
tool_manager = ToolManager()

# Cria o serviço de embeddings
embedding_service = EmbeddingService()

# Cria o serviço de conhecimento
knowledge_service = KnowledgeService(
    embedding_service
)

# Cria o agente
mentor = MentorAgent(
    llm_service,
    memory,
    tool_manager,
    knowledge_service
)
```

## O que mudou arquiteturalmente?

Até a aula anterior, todas as dependências eram criadas diretamente pelo `main.py` e entregues ao `MentorAgent`:

```text
main.py
│
├── LLMService
├── ConversationMemory
├── ToolManager
├── KnowledgeService
└── MentorAgent
```

Agora surgiu a primeira **árvore de dependências**:

```text
main.py
│
├── LLMService
├── ConversationMemory
├── ToolManager
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

Essa evolução é importante porque o `MentorAgent` continua conhecendo apenas o `KnowledgeService`. Quando adicionarmos `Retriever`, `VectorStore`, `DocumentLoader` e os demais componentes do RAG, eles ficarão "escondidos" atrás dessa camada, mantendo o agente simples e desacoplado.

Depois que você fizer essa alteração, restará apenas a **Etapa 5 (Reflexão Arquitetural)** e então geraremos o CHANGELOG da Aula M7.2.