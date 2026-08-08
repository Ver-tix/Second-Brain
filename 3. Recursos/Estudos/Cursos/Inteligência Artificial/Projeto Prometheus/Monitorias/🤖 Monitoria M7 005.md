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
Perfeito. Vamos **uma etapa por vez**, sem antecipar as próximas.

## Etapa 1 — `cosine_similarity()`

Crie o arquivo:

```text
app/
└── knowledge/
    └── similarity.py
```

Dentro dele, teremos **uma única função**:

```python
def cosine_similarity(
    vector_a: list[float],
    vector_b: list[float]
) -> float:
```

A função deverá receber dois vetores e retornar um número `float` representando a similaridade de cosseno entre eles.

### O que você precisa implementar

A ideia matemática é:

$$   
\text{similaridade} =  
\frac{A \cdot B}  
{|A||B|}  
$$

Você deverá fazer isso **manualmente em Python**, sem NumPy ou qualquer biblioteca externa.

Por enquanto, não se preocupe com `VectorStore`, `KnowledgeService` ou `main.py`.

**Seu único objetivo agora é fazer `similarity.py`.**

Tente escrever sozinho primeiro. Mande o código aqui e eu reviso linha por linha antes de passarmos à **Etapa 2**.

```Python
import math

def cosine_similarity(
    vector_a: list[float],
    vector_b: list[float]
) -> float:

    dot_product = sum(
        a * b
        for a, b in zip(vector_a, vector_b)
    )

    magnitude_a = math.sqrt(
        sum(a * a for a in vector_a)
    )

    magnitude_b = math.sqrt(
        sum(b * b for b in vector_b)
    )
    
    return dot_product / (magnitude_a * magnitude_b)
```

# Etapas 2 e 3 - Adicionar `search()` ao `VectorStore`

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

    def search(
            self,
            query_embedding: list[float],
            top_k: int=3
    ):
        pass

    def count_documents(self) ->int:
        return len(self.documents)

    def list_documents(self) ->list:
        return self.documents
```

A lógica será:

1. Percorrer `self.documents`;
2. calcular a similaridade;
3. criar um resultado contendo o documento + `score`;
4. ordenar pelo score;
5. devolver os `top_k`.

A estrutura conceitual é:

```
query_embedding
       │
       ▼
VectorStore.search()
       │
       ├── Documento A → score
       ├── Documento B → score
       ├── Documento C → score
       │
       ▼
    ordenar
       │
       ▼
   top_k
```

### Tente agora implementar **somente o `search()`**.

Não mexa ainda no `KnowledgeService`.

Mande seu `vector_store.py` depois que implementar, e eu reviso antes de irmos para a **Etapa 3 — adicionar o `score` aos resultados**.

## Etapa 2.1 preencher o `search()`

```Python
from app.knowledge.similarity import cosine_similarity

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

    def search(
        self,
        query_embedding: list[float],
        top_k: int = 3
):
        results = []

        for document in self.documents:
            score = cosine_similarity(
                query_embedding,
                document["embedding"]
            )
            
            result = {
                "text": document["text"],
                "embedding": document["embedding"],
                "score": score
            }
            results.append(result)

        results.sort(
            key=lambda result: result["score"],
            reverse=True
        )

        return results[:top_k]

    def count_documents(self) ->int:
        return len(self.documents)

    def list_documents(self) ->list:
        return self.documents
```

### O que esse método está fazendo?

O fluxo é:

```
query_embedding
       │
       ▼
 percorre documentos
       │
       ▼
cosine_similarity()
       │
       ▼
   score
       │
       ▼
 adiciona resultado
       │
       ▼
ordena maior → menor
       │
       ▼
   results[:top_k]
```

O detalhe importante é:

```
reverse=True
```

Sem isso, o Python ordenaria do **menor score para o maior**. Queremos o contrário: os documentos semanticamente mais próximos primeiro.

E:

```
results[:top_k]
```

significa "pegue somente os primeiros `top_k` resultados".

Por exemplo, se tivermos:

```
Documento A → 0.21
Documento B → 0.87
Documento C → 0.42
Documento D → 0.91
```

Depois da ordenação:

```
Documento D → 0.91
Documento B → 0.87
Documento C → 0.42
Documento A → 0.21
```

Com `top_k=2`:

```
Documento D
Documento B
```

**Isso já implementa a Etapa 2.**

E note uma coisa importante: você já tinha criado corretamente a estrutura necessária na aula anterior. Estamos apenas acrescentando a capacidade de **recuperação** ao armazenamento.
### Um detalhe importante

Você colocou:

```python
result = {
    "text": document["text"],
    "embedding": document["embedding"],
    "score": score
}
```

Isso está correto e atende diretamente à **Etapa 3** do desafio, que pede que o resultado carregue também o `score`.

Então, tecnicamente, **Etapas 2 e 3 já estão resolvidas no seu código**.

Não precisamos alterar isso agora.

# Etapa 4 - Atualizar o `KnowledgeService`:

Hoje temos:

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
            text: str
    ) -> list[float]:

        return self.embedding_service.generate_embedding(text)

    def add_document(
            self,
            text: str
    ) -> None:

        embedding = self.embedding_service.generate_embedding(text)

        self.vector_store.add_document(
            text,
            embedding
        )
```

Vamos acrescentar:
```Python
def search(
        self,
        query: str,
        top_k: int = 3
):
    query_embedding = self.embedding_service.generate_embedding(query)

    return self.vector_store.search(
        query_embedding,
        top_k
    )
```

O arquivo completo fica:
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
            text: str
    ) -> list[float]:

        return self.embedding_service.generate_embedding(text)

    def add_document(
            self,
            text: str
    ) -> None:

        embedding = self.embedding_service.generate_embedding(text)

        self.vector_store.add_document(
            text,
            embedding
        )

    def search(
            self,
            query: str,
            top_k: int = 3
    ):

        query_embedding = self.embedding_service.generate_embedding(query)

        return self.vector_store.search(
            query_embedding,
            top_k
        )
```

### O ponto mais importante desta etapa

Perceba que o `KnowledgeService` **não calcula similaridade**.

Ele também não sabe como o `VectorStore` funciona internamente.

Ele simplesmente coordena:

```
"Qual é a quantidade máxima de Bitcoins?"
                    │
                    ▼
          KnowledgeService.search()
                    │
                    ▼
          EmbeddingService
                    │
                    ▼
             query_embedding
                    │
                    ▼
          VectorStore.search()
                    │
                    ▼
             resultados
```

Isso é exatamente o papel de uma **camada de orquestração**.

E aqui aparece novamente a arquitetura que estamos construindo desde o início do Prometheus:

> **O componente de alto nível coordena; os componentes especializados executam.**

Depois de colocar esse método, **não vamos mexer no `MentorAgent` ainda**. A próxima etapa será o teste da recuperação no `main.py`.

# Etapa 5 - Teste de recuperação semântica
## 1. No `main.py`:

Você pode substituir o teste temporário que tínhamos usado para verificar a dimensão do embedding.

Na região onde você cria o `KnowledgeService`, coloque:

```Python
# ===== Teste de recuperação semântica =====

knowledge_service.add_document(
    "Bitcoin possui uma oferta máxima de 21 milhões de moedas."
)

knowledge_service.add_document(
    "Python é uma linguagem de programação de alto nível."
)

knowledge_service.add_document(
    "Redes neurais artificiais são modelos computacionais."
)

results = knowledge_service.search(
    "Qual é a quantidade máxima de Bitcoins?"
)

for result in results:
    print("\nDocumento:")
    print(result["text"])

    print("Score:")
    print(result["score"])

# ==========================================
```

E, assim, `main.py` fica:
```Python
from app.agents.mentor_agent import MentorAgent
from app.services.llm_service import LLMService
from app.memory.conversation_memory import ConversationMemory
from app.tools.tool_manager import ToolManager
from app.knowledge.knowledge_service import KnowledgeService
from app.knowledge.embedding_service import EmbeddingService
from app.knowledge.vector_store import VectorStore

def main():
    # Cria o serviço responsável por conversar com a OpenAI
    llm_service = LLMService()

    # Cria a memória dentro do programa
    memory = ConversationMemory()

    # Cria o gestor de ferramentas (tool manager)
    tool_manager = ToolManager()

    # Instanciar o serviço de embedding
    embedding_service = EmbeddingService()

    # Instanciar o VectorStore
    vector_store = VectorStore()

    # Instanciar o serviço de conhecimento
    knowledge_service = KnowledgeService(
        embedding_service,
        vector_store
    )

    # ===== Teste de recuperação semântica =====
    knowledge_service.add_document(
        "Bitcoin possui uma oferta máxima de 21 milhões de moedas."
    )

    knowledge_service.add_document(
        "Python é uma linguagem de programação de alto nível."
    )

    knowledge_service.add_document(
        "Redes neurais artificiais são modelos computacionais."
    )

    results = knowledge_service.search(
        "Qual é a quantidade máxima de Bitcoins?"
    )

    for result in results:
        print("\nDocumento:")
        print(result["text"])
        print("Score:")
        print(result["score"])

# ==========================================

    # Cria o agente, recebendo o serviço
    mentor = MentorAgent(
        llm_service,
        memory,
        tool_manager,
        knowledge_service,
        )

    # Loop de Questionamento:
    while True:
        # Recebe a pergunta do usuário
        question = input("\nPergunte alguma coisa: ")

        # Condição para sair
        if question.lower() == "sair":
            print("\nAté a próxima!")
            break

        # O agente responde
        answer = mentor.ask(question)

        # Exibe a resposta
        print("\nPrometheus-Mentor:\n")
        print(answer)

if __name__ == "__main__":
    main()
```

# Etapa 