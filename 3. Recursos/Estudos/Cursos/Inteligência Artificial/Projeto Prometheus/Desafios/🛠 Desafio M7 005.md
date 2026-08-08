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
# Etapa 1 — Criar a função de similaridade

Criar: 
```text
app/
└── knowledge/
    └── similarity.py
```

Dentro dele, teremos **uma única função**:
```python
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
# Etapas 2 e 3 — # Etapas 2 e 3 - Adicionar `search()` ao `VectorStore`

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

    def search(    # <-------FUNÇÃO IMPLEMENTADA AGORA
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


# Etapa 4 — Atualizar o `KnowledgeService`

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

    def search(             # <-------FUNÇÃO IMPLEMENTADA AGORA
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

# Etapa 5 — Teste

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

# Etapa 6 - Reflexão


**1. Por que precisamos transformar a pergunta do usuário em um embedding antes de consultar o Vector Store?**

Porque o `VectorStore` não compara diretamente o significado de textos. Ele trabalha com **vetores numéricos**.

Então, se o usuário pergunta:

> "Qual é a quantidade máxima de Bitcoins?"

precisamos transformar essa pergunta em um embedding:

```text
Pergunta
   ↓
EmbeddingService
   ↓
Query Embedding
   ↓
VectorStore
```

Assim podemos comparar o vetor da pergunta com os vetores dos documentos armazenados usando a similaridade de cosseno.

Isso permite encontrar documentos **semanticamente relacionados**, mesmo quando eles não utilizam exatamente as mesmas palavras.

---

**2. Por que o `MentorAgent` não deveria calcular a similaridade diretamente?**

Porque calcular similaridade é uma responsabilidade da **camada de conhecimento**, não do agente.

O `MentorAgent` deve apenas coordenar o sistema. Ele não precisa conhecer:

- como embeddings são gerados;
    
- como vetores são armazenados;
    
- como a similaridade de cosseno funciona;
    
- como os documentos são ordenados.
    

Ele simplesmente poderá pedir:

```python
knowledge_service.search(query)
```

E o fluxo interno será:

```text
MentorAgent
     ↓
KnowledgeService
     ↓
EmbeddingService
     ↓
query_embedding
     ↓
VectorStore
     ↓
cosine_similarity
     ↓
documentos relevantes
```

Isso mantém o **baixo acoplamento** que construímos desde o Módulo 6.

Se amanhã substituirmos a similaridade de cosseno por outra estratégia, ou trocarmos nosso `VectorStore` em memória por Qdrant, FAISS ou outro sistema, o `MentorAgent` não deveria precisar saber disso.

**Esse é justamente o ganho arquitetural que estamos construindo.**