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

# Etapa 2 — Estrutura interna

Dentro dele, implemente a classe básica.

```python
class VectorStore:

    def __init__(self):
        self.documents = []
```

# Etapa 3 - - Adicionar método que Anexa dicionários à lista `documents`

```Python
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

# Etapa 5 - Atualizar o `KnowledgeService`

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
    def count_documents(self) ->int:
        return self.vector_store.count_documents()
    
```

# Etapa 6 - Atualizar o `main.py`

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

    # ===== Teste do Vector Store =====
    knowledge_service.add_document(
        "Bitcoin é um ativo escasso"
        )

    knowledge_service.add_document(
        "Ethereum permite contratos inteligentes"
    )

    print(
        knowledge_service.count_documents()
    )

    # ==============
    
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

# Etapa 7 - Reflexão Arquitetural

Criamos um `VectorStore` separado do `EmbeddingService` porque **cada componente deve possuir uma responsabilidade bem definida**.

O `EmbeddingService` tem uma única função:

```text
Texto
  ↓
EmbeddingService
  ↓
Vetor
```

Ele é responsável por **gerar embeddings**. Ele não deve saber onde esses vetores serão armazenados.

Já o `VectorStore` tem outra responsabilidade:

```text
Documento + Embedding
        ↓
   VectorStore
        ↓
   Armazenamento
```

Ele é responsável por **armazenar os documentos e seus embeddings**.

Se colocássemos o armazenamento dentro do `EmbeddingService`, teríamos algo assim:

```text
EmbeddingService
├── gera embeddings
└── armazena embeddings
```

Isso misturaria duas responsabilidades diferentes. Consequentemente, qualquer mudança no armazenamento poderia exigir alterações no serviço responsável pelos embeddings.

Com a separação:

```text
KnowledgeService
       │
       ├──────────────► EmbeddingService
       │                    │
       │                    ▼
       │                 gera vetor
       │
       └──────────────► VectorStore
                            │
                            ▼
                        armazena
```

o `KnowledgeService` coordena os dois, enquanto cada componente permanece especializado.

### E existe uma segunda vantagem importante: substituição

Hoje nosso armazenamento é:

```text
VectorStore em memória
```

Amanhã poderemos substituí-lo por:

```text
ChromaDB
```

depois talvez:

```text
Qdrant
```

ou outro banco vetorial.

O `EmbeddingService` **não precisa mudar**, porque ele continua tendo a mesma responsabilidade: transformar texto em vetor.

Da mesma forma, podemos trocar o modelo de embeddings sem transformar o `VectorStore` em algo diferente.

Portanto, a decisão segue três princípios:

- **Responsabilidade única:** cada componente faz uma coisa.
    
- **Alta coesão:** as coisas que pertencem umas às outras ficam juntas.
    
- **Substituição:** podemos trocar uma implementação sem contaminar as outras.
    

E o mais importante: **o `KnowledgeService` funciona como a camada de orquestração**, escondendo esses detalhes do restante do sistema.

O `MentorAgent` não precisa saber que existe `EmbeddingService` ou `VectorStore`. Ele só conhece:

```python
knowledge_service
```

Isso permite que a infraestrutura de RAG cresça internamente sem transformar o `MentorAgent` em um componente gigantesco.

---

# Feedback

Ah, sim — agora entendi. **A sua resolução do M7.004 está dentro do arquivo**, e eu já a localizei.

E agora sim posso fazer a correção como professor.

# 🧑‍🏫 Avaliação — Desafio M7.004

## Nota: **10/10** ✅

Você executou exatamente o que a aula pedia, sem antecipar a busca vetorial.

### 1. `VectorStore` — correto

Você criou:

```python
self.documents = []
```

e armazenou cada documento como:

```python
{
    "text": text,
    "embedding": embedding
}
```

Também implementou `count_documents()` e `list_documents()`. Isso está perfeitamente alinhado com o escopo da aula.

### 2. `KnowledgeService` — muito bom

Aqui está o ponto arquitetural mais importante:

```text
KnowledgeService
       │
       ├── EmbeddingService
       │
       └── VectorStore
```

O `KnowledgeService` gera o embedding e depois entrega **texto + embedding** ao `VectorStore`.

Você não colocou o `EmbeddingService` dentro do `VectorStore`, o que demonstra que entendeu a separação de responsabilidades.

### 3. `main.py` — correto

Você construiu a árvore de dependências:

```text
main
│
├── EmbeddingService
├── VectorStore
│
└── KnowledgeService
      ├── EmbeddingService
      └── VectorStore
```

E testou exatamente como esperado:

```python
knowledge_service.add_document(...)
knowledge_service.add_document(...)

print(knowledge_service.count_documents())
```

Isso demonstra que o fluxo completo está funcionando conceitualmente.

---

# 🧠 E a sua reflexão foi excelente

Sua resposta acertou o núcleo:

> "`EmbeddingService` gera embeddings; `VectorStore` armazena."

E você foi além ao perceber a possibilidade de substituir:

```text
VectorStore em memória
        ↓
     ChromaDB
        ↓
      Qdrant
```

sem precisar modificar o `EmbeddingService`.

Isso é exatamente o tipo de raciocínio que quero que você desenvolva no Prometheus: **não pensar apenas em "como faço funcionar?", mas "como faço uma parte do sistema poder mudar sem destruir as outras?"**

---

# ⚠️ Uma pequena observação

Você colocou:

```python
def list_documents(self) -> list:
    return self.documents
```

Está **correto para esta aula**.

Em uma aplicação de produção, poderíamos discutir se devemos expor diretamente a estrutura interna. Mas se eu começasse a exigir isso agora, estaria fazendo exatamente aquilo que combinamos evitar: **antecipar complexidade antes da hora**.

Portanto: **não altere.**

---

# 🔥 O que você acabou de construir

Compare o Prometheus antes e depois.

### Antes

```text
KnowledgeService
       │
       ▼
EmbeddingService
```

### Agora

```text
KnowledgeService
       │
       ├──────────────► EmbeddingService
       │                       │
       │                       ▼
       │                    vetor
       │
       └──────────────► VectorStore
                               │
                               ▼
                           armazenamento
```

E isso é importante porque **o próximo passo não precisará mexer no `MentorAgent`**.

Vamos começar a adicionar inteligência à camada de conhecimento **por baixo da fachada**.

---

# 💎 Insight Prometheus da M7.4

Existe uma mudança conceitual acontecendo:

Até agora:

> **"Eu tenho um texto e consigo transformá-lo em vetor."**

Agora:

> **"Eu tenho uma representação vetorial persistível dentro de uma estrutura de conhecimento."**

Ainda não temos RAG.

Mas temos a infraestrutura necessária para começar a **recuperar conhecimento por significado**.

E é justamente isso que vem agora.

---

# 📝 CHANGELOG — M7.4

```markdown
## M7-004 — Primeiro Vector Store

### Adicionado
- Criado `VectorStore` em `app/knowledge/vector_store.py`.
- Implementado armazenamento de documentos e embeddings em memória.
- Implementados métodos `add_document()`, `count_documents()` e `list_documents()`.

### Alterado
- `KnowledgeService` passou a receber `EmbeddingService` e `VectorStore`.
- Implementado fluxo de ingestão:
  Texto → EmbeddingService → VectorStore.
- `main.py` atualizado para instanciar e injetar o VectorStore.
- Adicionado teste de armazenamento de múltiplos documentos.

### Arquitetura
- Consolidada a separação entre geração de embeddings e armazenamento vetorial.
- `KnowledgeService` estabelecido como camada de orquestração da infraestrutura de conhecimento.
- Preparada a arquitetura para futura substituição do VectorStore em memória por uma implementação persistente.
```

**M7.4 concluída.** 🎬

Agora sim, filhão: estamos a **uma aula da primeira busca semântica do Prometheus**.