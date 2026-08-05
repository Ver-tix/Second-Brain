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

Conteúdo:

```python
class EmbeddingService:
    pass
```

Nada mais.

---

# Etapa 2 — Atualizar o `KnowledgeService`

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