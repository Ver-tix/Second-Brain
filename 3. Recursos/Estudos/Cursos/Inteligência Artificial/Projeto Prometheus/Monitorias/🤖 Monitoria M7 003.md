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

# Etapa 1 — Atualizar `settings.py`

Adicione a constante do modelo de embeddings, ao lado da constante do modelo de chat.

```python
OPENAI_API_KEY = ...

MODEL_NAME = "gpt-5"

EMBEDDING_MODEL = "text-embedding-3-small"
```

A ideia é a mesma utilizada para `MODEL_NAME`: evitar "strings mágicas" espalhadas pelo projeto.

---

# Etapa 2 — Implementar o `EmbeddingService`
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
## Por Que retorno `response.data[0].embedding`?
Excelente pergunta. Essa é uma daquelas linhas que vale a pena entender profundamente, porque ela mostra como ler a resposta da API da OpenAI.

Vamos por partes.

---

### O que enviamos para a API?

Nós fazemos:

```python
response = self.client.embeddings.create(
    model=EMBEDDING_MODEL,
    input=text
)
```

Ou seja, enviamos um texto:

```text
"Bitcoin é um ativo escasso."
```

A OpenAI responde com um objeto muito maior do que apenas o embedding.

É algo conceitualmente parecido com isto:

```python
{
    "object": "list",

    "data": [

        {
            "object": "embedding",

            "index": 0,

            "embedding": [
                0.012,
                -0.432,
                0.983,
                ...
            ]
        }

    ],

    "model": "text-embedding-3-small",

    "usage": {
        ...
    }
}
```

Perceba que existem várias informações.

Nós só queremos uma delas.

---

### Por que existe `data`?

Porque a API foi projetada para poder gerar embeddings de **vários textos de uma vez**.

Imagine:

```python
input=[
    "Bitcoin",
    "Ethereum",
    "Solana"
]
```

A resposta seria parecida com:

```python
data = [

    {
        "index": 0,
        "embedding": [...]
    },

    {
        "index": 1,
        "embedding": [...]
    },

    {
        "index": 2,
        "embedding": [...]
    }

]
```

Ou seja:

```
data
│
├── embedding do texto 0
├── embedding do texto 1
└── embedding do texto 2
```

Mesmo enviando apenas um texto, a API mantém esse formato por consistência.

---

### Por que usamos `[0]`?

Porque enviamos **apenas um** texto.

Então:

```python
response.data
```

contém uma lista com um único elemento:

```python
[
    {
        "embedding": [...]
    }
]
```

O primeiro elemento é:

```python
response.data[0]
```

---

### E por que `.embedding`?

Porque o objeto possui várias propriedades:

```python
response.data[0]

↓

{
    "object": "embedding",
    "index": 0,
    "embedding": [...]
}
```

Nós queremos somente:

```python
response.data[0].embedding
```

Que é justamente o vetor.

---

### Visualizando o caminho

```
response
    │
    ▼
data
    │
    ▼
[0]
    │
    ▼
embedding
    │
    ▼
[0.012, -0.432, 0.983, ...]
```

---

### Por que retornamos apenas isso?

Porque o **`EmbeddingService` possui uma única responsabilidade**.

Ele deve transformar:

```
Texto

↓

Embedding
```

Nada mais.

Se retornássemos o objeto inteiro:

```python
return response
```

quem chamasse o serviço teria que saber navegar por:

```python
response.data[0].embedding
```

Isso faria com que detalhes da API da OpenAI "vazassem" para o restante do sistema.

Ao retornar apenas:

```python
return response.data[0].embedding
```

o contrato do `EmbeddingService` fica muito simples:

```
Entrada:
texto

↓

Saída:
list[float]
```

E isso é uma excelente prática de arquitetura: **esconder detalhes de implementação atrás de uma interface simples**.

Essa mesma ideia já apareceu no `LLMService`. O restante do Prometheus não sabe como a OpenAI organiza sua resposta; ele apenas recebe um dicionário padronizado com `"type"` e `"content"`. O `EmbeddingService` faz a mesma coisa: esconde a estrutura da resposta da API e entrega exatamente o que os demais componentes precisam.

---

# Etapa 3 - Atualização do `KnowledgeService`
Atualizar o `KnowledgeService` para expor o método `generate_embedding()` como uma fachada sobre o `EmbeddingService`.

## O que queremos fazer?

Hoje o restante do sistema conhece apenas:

```text
MentorAgent
        │
        ▼
KnowledgeService
```

Ele **não deve conhecer** o `EmbeddingService`.

Portanto, o `KnowledgeService` passa a oferecer uma interface pública para geração de embeddings.

A arquitetura fica:

```text
MentorAgent
        │
        ▼
KnowledgeService
        │
        ▼
EmbeddingService
```

---

# Alteração em `knowledge_service.py`

Seu arquivo hoje está assim:

```python
from app.knowledge.embedding_service import EmbeddingService


class KnowledgeService:

    def __init__(
        self,
        embedding_service: EmbeddingService
    ):
        self.embedding_service = embedding_service
```

Agora basta acrescentar um método:

```python
def generate_embedding(
    self,
    text: str
) -> list[float]:

    return self.embedding_service.generate_embedding(text)
```

O arquivo completo ficará:

```python
from app.knowledge.embedding_service import EmbeddingService


class KnowledgeService:

    def __init__(
        self,
        embedding_service: EmbeddingService
    ):
        self.embedding_service = embedding_service

    def generate_embedding(
        self,
        text: str
    ) -> list[float]:

        return self.embedding_service.generate_embedding(text)
```

---

# Por que fazer isso?

À primeira vista parece desnecessário:

```python
KnowledgeService

↓

EmbeddingService

↓

Embedding
```

"Por que não chamar logo o `EmbeddingService`?"

Porque queremos que o restante da aplicação conheça apenas **uma porta de entrada** para a camada de conhecimento.

Hoje:

```text
KnowledgeService
        │
        ▼
EmbeddingService
```

Daqui a algumas aulas será algo como:

```text
KnowledgeService
        │
 ┌──────┼───────────────┐
 ▼      ▼               ▼

Retriever
EmbeddingService
VectorStore
```

E o `MentorAgent` continuará fazendo apenas:

```python
knowledge_service.generate_embedding(...)
```

Ele nunca precisará saber quantos componentes existem dentro da camada de conhecimento.

---

Essa é uma aplicação clássica do princípio do **Encapsulamento**: esconder a complexidade interna atrás de uma interface simples.

Implemente esse método e me envie o `knowledge_service.py`. Depois partiremos para a **Etapa 4**, onde faremos o teste temporário no `main.py`.