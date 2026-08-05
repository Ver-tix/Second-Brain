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
Perfeito. Com essas respostas, temos o escopo totalmente delimitado. Vamos implementar exatamente o que a aula pede, sem antecipar abstrações.

## Etapa 1 — Atualizar `settings.py`

Adicione a constante do modelo de embeddings, ao lado da constante do modelo de chat.

```python
OPENAI_API_KEY = ...

MODEL_NAME = "gpt-5"

EMBEDDING_MODEL = "text-embedding-3-small"
```

A ideia é a mesma utilizada para `MODEL_NAME`: evitar "strings mágicas" espalhadas pelo projeto.

---

## Etapa 2 — Implementar o `EmbeddingService`
```text
app/
└── knowledge/
    ├── __init__.py
    ├── knowledge_service.py
    └── embedding_service.py   ← aqui    
```

Agora podemos transformar aquele esqueleto em um serviço funcional.

A estrutura esperada é praticamente um espelho do `LLMService`.

```python
from openai import OpenAI

from app.config.settings import (
    OPENAI_API_KEY,
    EMBEDDING_MODEL
)


class EmbeddingService:

    def __init__(self):
        self.client = OpenAI(api_key=OPENAI_API_KEY)

    def generate_embedding(
        self,
        text: str
    ) -> list[float]:

        response = self.client.embeddings.create(
            model=EMBEDDING_MODEL,
            input=text
        )

        return response.data[0].embedding
```

Observe alguns detalhes importantes:
- a classe cria seu próprio `OpenAI()` (consistência com o `LLMService`);
- usa `EMBEDDING_MODEL` centralizado no `settings.py`;
- retorna diretamente `response.data[0].embedding`;
- não trata exceções;
- retorna `list[float]`, deixando explícito o contrato do serviço.