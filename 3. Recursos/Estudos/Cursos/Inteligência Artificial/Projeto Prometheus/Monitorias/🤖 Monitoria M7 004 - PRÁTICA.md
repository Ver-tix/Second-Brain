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
# Etapa 1 — Criar `vector_store.py`

Crie um novo arquivo:

```text
app/
└── knowledge/
    └── vector_store.py
```

---

# Etapa 2 — Estrutura interna

Dentro dele, implemente a classe básica.

```python
class VectorStore:

    def __init__(self):
        self.documents = []
```

## O que essa lista armazenará?

Cada elemento será um dicionário simples contendo:

```python
{
    "text": text,
    "embedding": embedding
}
```

Por exemplo, após adicionar dois documentos, a estrutura ficará assim:

```python
self.documents = [
    {
        "text": "Bitcoin é um ativo escasso.",
        "embedding": [...]
    },
    {
        "text": "Ethereum permite contratos inteligentes.",
        "embedding": [...]
    }
]
```

Repare na separação de responsabilidades:
- `EmbeddingService` **gera** o embedding.
- `VectorStore` **armazena** o embedding.
- O `VectorStore` não sabe como aquele vetor foi criado.

Isso é exatamente o princípio da responsabilidade única (SRP) que temos aplicado desde o Módulo 6.

---

# Etapa 3 - Adicionar método que Anexa dicionários à lista `documents`

==**O código que eu fiz:**==

```python

class VectorStore:
    def __init__(
            self
    ):
        self.documents=[] 

    def add_document(
            self,
            text: str,
            embedding: list[float]
    )->dict:

        # cria o dicionário com o texto e o embedding
        document = {
            "text": text,
            "embedding":embedding
        } 

        # (opcional, mas eu recomendo) salvar o documento nalista da classe
        self.documents.append(document)
        
        return document
```

==**correção do monitor:**==
Sim — **a lógica está correta**, e você implementou a Etapa 3 praticamente como esperado. Só faria **uma correção importante de contrato**.

Você colocou:

```python
)->dict:
```

e retorna:

```python
return document
```

Mas o desafio diz que `add_document()` **apenas adiciona o documento à lista**. O retorno não é necessário.

Eu deixaria assim:

```python
class VectorStore:

    def __init__(self):
        self.documents = []

    def add_document(
        self,
        text: str,
        embedding: list[float]
    ) -> None: # O None deixa explícito que ele não retorna nada,apenas anexa à lista

        document = {
            "text": text,
            "embedding": embedding
        }

        self.documents.append(document)
```

### Por quê?

A responsabilidade do método é:

```text
text + embedding
        ↓
   criar documento
        ↓
   armazenar na lista
```

Não:

```text
text + embedding
        ↓
   criar documento
        ↓
   armazenar
        ↓
   devolver documento
```

Esse `return` não chega a ser um erro funcional, mas **é uma responsabilidade que não precisamos criar agora**.

E um detalhe: seu comentário diz:

> `(opcional, mas eu recomendo) salvar o documento na lista`

Nesse caso, **não é opcional**. É justamente a função principal do `add_document()`. 😄

Então, para a nossa arquitetura, eu recomendo a versão com `-> None`.

**Etapa 3 concluída.**

Agora podemos partir para a **Etapa 4 — `count_documents()` e `list_documents()`**.

---

# Etapa 4 - Métodos Auxiliares

  
```Python
class VectorStore:

    def __init__(

            self

    ):

        self.documents=[]

  

    def add_document(

            self,

            text: str,

            embedding: list[float]

    ):

  

        # cria o dicionário com o texto e o embedding

        document = {

            "text": text,

            "embedding":embedding

        }

  

        # (opcional, mas eu recomendo) salvar o documento nalista da classe

        self.documents.append(document)

  

    def count_documents(self) ->int:

        return len(self.documents)

  

    def list_documents(self) ->list:

        return self.documents
       
```

# Etapa 4 - Atualizar o `KnowledgeService`

```Python
from app.knowledge.embedding_service import EmbeddingService
from app.knowledge.vector_store import VectorStore

class KnowledgeService:
    def __init__(
            self,
            embedding_service: EmbeddingService,
            vector_store: VectorStore
            ):
        self.embedding_service = embedding_service
        self.vector_store = vector_store  

    def generate_embedding(
            self,
            text:str
    ) ->list[float]:
        return self.embedding_service.generate_embedding(text)

    def add_document(
            self,
            text:str
    )-> None:
        embedding = self.embedding_service.generate_embedding(text)

        self.vector_store.add_document(
            text,
            embedding
        )
```